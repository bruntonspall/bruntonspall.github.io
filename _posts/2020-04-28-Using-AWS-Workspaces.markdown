---
layout: default
title: "Using AWS Workspaces to control access to documents"
date: 2020-04-28 11:00:00 +0100
---

I've recently worked on a project where we had to have some documents that needed to be kept reasonably secure, and on the clients computers for our project.  We needed our developers to have some access to the documents, to visually inspect them, and to be able to run code on them, but we didn't want the developers to have copies on their local laptops or computers.

We decided that [AWS Workspaces](https://aws.amazon.com/workspaces/) would be a good fit for this usage.  For those of you who haven't used it, AWS Workspaces allows you to create desktop computers in AWS's data centers which you can connect to via  remote desktop protocol.  It looks like your computer, but in fact your mouse and keyboard are attached to a remote computer.  These are standard windows or linux desktop computers and the users can do anything on them that you'd expect to be able to do on a local computer, but of course nothing about the computer leaves the Amazon data center.

### Locking down the data

We had a smallish set of data that we wanted to do some data science on, that is running some scripts that might parse the files, look for commonalities and so on.  Of course, in order to do that the developers need to be able to look at the files, so they know what they're looking at.  We could allow individual developers to see a handful of files by using a client laptop, but that laptop is restricted in running code, so we selected AWS Workspaces as a way of triaging access.

Our first step therefore is to migrate data into somewhere accessible by the Workspaces computers.  We picked an S3 bucket for this.  We were able to create the S3 bucket with ACL's set to disallow public access.

Workspaces can be [deployed inside an Amazon Virtual Private Cloud (VPC)](https://docs.aws.amazon.com/workspaces/latest/adminguide/amazon-workspaces-vpc.html#configure-vpc-nat-gateway), which means that we can create a private subnet that cannot be accessed from the internet, create the workspaces in there, and then configure the AWS NAT Gateway to enable the amazon client to access the machine and to allow certain data transfer out.

This means that the developer can connect to our Workspaces client, and they get bought up on a machine inside the private subnet, totally isolated from the internet.  However, using [AWS Gateway Endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html), we can enable the private subnet to access the S3 bucket.  On the S3 bucket, we can set an allow policy that allows access from the private subnet, and we can configure the [Endpoint Gateway with an endpoint policy](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-s3.html#vpc-endpoints-policies-s3) that allows access only to our specified bucket.  This means that the devs can download files from the S3 endpoint, but cannot upload those to a new public bucket in any way.

This is pretty much all configured the way that the Workspaces documentation recommends, however we wanted a little extra security.

### Authenticating the users

While it would be nice to enable MFA, requiring developers to authenticate not just with a password, but with a token from their mobile phone, this requires configuring the Managed Active Directory to work with a RADIUS server, and that's beyond my rather basic technical ability.  

But we really don't want anyone who tries to connect to our desktops to just try passwords in a password spraying attack.  I really want some form of second factor.  Luckily, AWS provides something called [Trusted Devices within AWS Workspaces](https://docs.aws.amazon.com/workspaces/latest/adminguide/trusted-devices.html).  This means that a device requires a machine installed certificate that has been signed by a defined certificate authority.

This sounds pretty simple, we've got a couple of steps we need to take:

1. Create a certificate authority
2. Create certificates per device/user
3. Get the certificates to the developers
4. Let them authenticate to AWS Workspaces
5. ???
6. Profit

Of course, this sounded easy, but it turned out to be much harder than it should be.  The biggest problem here is that AWS Workspaces client doesn't actually seem to provide any logging of any form.  I spent a lot of time looking at an error screen telling me that it couldn't connect with no explanation as to why.

### Create a certificate authority

If we were running a large estate or big corporate installation of this, we might want to use a full PKI process.  In my case, we were looking for less than 5 developers having access to infrastructure that is going to be destroyed in a few months, so we can roll out own Certificate Authority.

Our first step is to create the Certificate Authority private key: `openssl genrsa -aes256 -out CA.key 2048`.  This private key is going to be our root key, so give it a good password.  However, you are going to be typing that password a lot, so make it something you can type!  I recommend using three simple words, so something like `workspace signing easy` might work just fine.

Next we need to create the public version of the key, `openssl req -x509 -new -nodes -key CA.key -sha256 -days 1024 -out CA.pem -subj "/C=GB/ST=England/L=London/O=ORGHERE/OU=OUHERE/CN=CNHERE"`.  You'll want to update ORGHERE, OUHERE and CNHERE with relevant names.  In reality, very little of this is going to matter, but the CN is used in a few places, so make it something memorable.  `projectname.local` makes a good CN here. 

Now you've got a CA.pem file, you can upload it to the AWS dashboard in AWS Workspaces and Workspaces will only allow clients that have a certificate signed by the CA to be allowed.  Note that you only need to send the public portion of the key, `CA.pem`, to AWS.  You still need to keep the private key, `CA.key`, secure on a local device somewhere.

### Creating a client certificate
You can do this one of two ways.  The easiest is to generate the certificates on the CA machine and then copy both the private and public keys to the laptop or desktop that needs to use them.  The harder way is to generate the private key on the laptop and only copy the intermediate files around. The second, harder way is marginally safer, but with decent passwords and a limited risk exposure, you may be willing to use the easier method.

You'll need to replace the word CLIENT throughout with the name of the machine or some other identifier

First we need to generate a certificate on the local machine using `openssl genrsa -aes256 -out CLIENT.local.key 2048`.  Again, you want to set a decent password here, as anyone who gets hold of this key can authenticate to your AWS Workspaces client.

Once you've got a private key, you need to generate a signing request using `openssl req -new -key CLIENT.local.key -out CLIENT.local.csr -subj "/C=GB/ST=MyCounty/L=MyTown/O=MyOrganisation/OU=MyOrganisationUnit/CN=CLIENT.local.client"`.  This signing request, the csr file, is the thing that you need to get to your CA computer to be signed, so copy it over, or if you are lazy, do this all on the same machine!

Now we need to sign the certificate.  This caused me a lot of pain in AWS Workspaces, I had to ask friends who had done similar things to get the exact options setup.  The command is `openssl x509 -req -in CLIENT.local.csr -CA CA.pem -CAkey CA.key -CAcreateserial -out CLIENT.local.crt -days 365 -sha256   -extensions v3_req -extfile ext.txt`.  This reads in the csr file, and the CA key, and it output a signed certificate in the crt file.  It tells openssl to use the sha256 signing algorithm (needed by AWS Workspaces), and it tells it to use some v3_req extensions.  This final part is really important, the whole thing wont work if you are missing this.

In order to generate the right extensions, you'll need an ext.txt file, which should contain the following contents:

```
[v3_req]
keyUsage = digitalSignature
extendedKeyUsage = clientAuth
```

This tells the CA that it wants the certificate to be signed for client authentication and signature purposes.

You can now copy the certificate back over to your laptop.

### Getting OSX to use the certificate

You'd think that this would be enough, but we also had problems getting OSX Keychain to actually import the CRT file.  I'm not 100% sure that this step is necessary, but it's what got it working for me, so I recommend it for you as well.

What we need for Keychain to easily import the certificate is to make a bundle of the key, the certificate and the public key for the CA.  This bundle can be created with the pkcs12 tool as follows: `openssl pkcs12 -export -aes256 -out  CLIENT.local.full.pfx -inkey CLIENT.local.key -in  CLIENT.local.crt -certfile CA.pem`.

This will create a pfx file, which you can then double click on to import into your local keyring.  If you do this, you'll import the certificate into the users keychain, which means that whenever AWS Workspaces wants to access it, you'll be prompted for your login password (or if you've got it setup, touchid).  You can also import this certificate into the login keychain, which means it wont need your password to access, but that also means that anyone with access to your device can use it without your password.

Once you've got this in, you should be able to start the AWS Workspaces client, and it'll authenticate properly and let you in.

I've [created some simple scripts to create CA and client keys](https://github.com/bruntonspall/AWSWorkspacesCA) for you which you might find useful if you are doing the same thing.

I hope this helps you, I'm mostly impressed with Workspaces as a remote desktop solution, it does what it says on the tin, and ensures that my cloud data stays in the cloud, and not on the laptop that can be left in a pub, stolen on the train or otherwise lost.