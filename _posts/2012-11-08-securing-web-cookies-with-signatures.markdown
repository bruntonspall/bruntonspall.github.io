---
layout: default
status: publish
published: true
title: Securing web cookies with signatures
featured:
  preview: /images/uploads/2012/11/cookies-300x225.jpg
  large: /images/uploads/2012/11/cookies.jpg
  caption: cookie for elsa by Marshsu
  url: http://www.flickr.com/photos/marshsu/27342366
author: Michael Brunton-Spall
author_login: bruntonspall
author_email: michael@brunton-spall.co.uk
author_url: http://www.brunton-spall.co.uk
wordpress_id: 4613
wordpress_url: http://www.brunton-spall.co.uk/?p=4613
date: '2012-11-08 23:38:14 +0000'
date_gmt: '2012-11-08 23:38:14 +0000'
categories:
- technical
- featured
tags:
- web development
- security
- cookies
comments: true
---
How can you authenticate a user in a web system with a "Shared-Nothing" architecture when you are not sure what webserver you'll come back to for any given request?

<!--more-->
<em>[Update: <a href="http://twitter.com/jabley">James Abley</a> points out via twitter that there is no mention of <a href="http://vudang.com/2012/03/md5-length-extension-attack/" target="_blank">MD5/HMAC extension attacks</a> here.  I'd love to claim it was deliberate, but was actually because I hadn't thought of them as connected with this.  I've added a postscript to try to address them]</em>

Let's assume that you are happy to use cookies.

<ol>
<li>Make a request to a protected resource (lets say /article/create), get detected as unauthenticated and therefore unauthorised and get redirected to our login page, /login</li>
<li>Submit username and password to /login, which checks them against a database of usernames and passwords (using a secure slow one-way hash like bcrypt), if your username and password match then redirect back to the original url</li>
<li>Request the original url, now with an authentication token, which detects that you are authorised to access the url.</li>
</ol>
The user is totally in control of all 3 of these requests, and we cannot keep any state on the server side so we are going to need to provide the user with the relevant data to use for all the conversation. So for example:

<ol>
<li>/article/create returns a redirect to /login, with a "return-url" cookie set to "/article/create"</li>
<li>The user POST's to /login with username, password and the return-url cookie. If successful set a "username" cookie to the username and redirect to return-url</li>
<li>The user requests /article/create now with the username cookie.</li>
</ol>
Assuming that understanding how to actually authenticate a user on the /login handler is a solved problem, the trick here is to ensure that this user flow is secure. There are two important parts.

<h3>Authentication is not authorisation</h3>
Just because the user has been authenticated, by having a username token, doesn't mean that they are authorised to access a url. This is a relatively common issue where having a user account is confused with having the access to any given url. You need to ensure that the /article/create controller does a check to ensure that the given username is allowed to access the resource requested. This is normally best done completely serverside, and you should probably look into Role Based Authentication for any reasonably complex system

<h3>Authentication must be secure</h3>
The simplest thing we can do with an authentication cookie is to set a cookie that containers the user-id or username. So once I've logged in, we set a username cookie containing "Michael Brunton-Spall", or uid=1 or something.

The problem with this is that the user is in total control of this cookie, so I could change my cookie value to "Alan Rusbridger" and the system would then assume that I had been authenticated as the Editor of the Guardian. That's probably not what we want.

The question therefore becomes how do I make sure that the value in the cookie hasn't been tampered with?

To solve this the simplest way we just need to provide a fingerprint or checksum for the cookie. We could just MD5/SHA1 or use some other signing algorithm and append it to the cookie, so instead of just Michael Brunton-Spall we could set "Michael Brunton-Spall|71c937e3aafbc99ded390aad306ac594e8cf969d"

To verify this cookie, we split it at the pipe character, SHA1 the first part and compare that SHA1 to the second part. If they match then the first part wasn't modified at all.

The problem with this is that a signature is not secure since anyone can generate a new SHA1, so I could change the cookie to "Alan Rusbridger|71c937e3aafbc99ded390aad306ac594e8cf969d" and the cookie wouldn't be verified, but if I change it to "Alan Rusbridger|f30bd13d1226d47899a3ed31feb33daa77fcff93" then the cookie is valid and again I an impersonate someone else. I can do this because SHA1 is a known algorithm and easy to do: 'echo "Michael Brunton-Spall" | shasum' for example.

So we need a secret, some piece of information that isn't transmitted to the client but is shared by the servers. The handy thing about signatures is that they are designed to be a one way function. There are potentially thousands of possible inputs that can result in the SHA1 of 71c937e3aafbc99ded390aad306ac594e8cf969d, so it's not mathmatically possible to take the signature and go backwards to the original string to know that it was "Michael Brunton-Spall" that generated it.

So how to do we use a secret and verify it? The algorithm works something like this:

<ol>
<li>Take the username, append the secret, so "Michael Brunton-Spall" =&gt; "Michael Brunton-Spall|Kittens".</li>
<li>SHA1 the combined string to get "bf650c31ed39e7dcdd26462eac8b22a99be339df".</li>
<li>Send the cookie to the user as username and SHA1, i.e. "Michael Brunton-Spall|bf650c31ed39e7dcdd26462eac8b22a99be339df".</li>
</ol>
To verify we simply repeat the same process,

<ol>
<li>Split the cookie at the pipe into username and signature</li>
<li>Take the username and add the secret</li>
<li>SHA1 the resultant string and compare against the user supplied signature, if they are a match then the username wasn't modified.</li>
</ol>
This is the basis of a signed cookie, and deals with the simplest of attack vectors in your system. The nice part is that there is nothing server dependent about this system, a cookie set by one server applies to another server, and the cookie can be maintained indefinately between server restarts and it doesn't need any database access to verify.

The hard part here is that you need to make sure that each server has the secret key on it, something that can be tricky to do securely. Secondly this only protects against a malicious client attempting to modify their cookie, it does not prevent somebody stealing a cookie and re-using it, or in their place.

If you want to make this system more secure you could

<h3>Use a different secret per user</h3>
This solves the problem that if I figure out the secret I can simply change my name or userid to somebody elses, sign it and impersonate them. That means an attacker who breaks into one account can only access that one account.

<h3>Use a time signature in the cleartext</h3>
Use "Michael Brunton-Spall|2012-11-08T23:00:00" as the plaintext part of the key. You can then expire the cookie after a set time perdiod and require a server to re-issue the cookie after that.<br />
This solves the problem of stealing the cookie from a user and then reusing it later. Essentially you are limiting the usefulness of a cloned authentication token to a minumum time period.

<h3>Add a nonce, or one time random number, to each cookie.</h3>
Essentially generate a really random number, say a random 64bit number. Keep track of whether you've seen any given random number before and only allow any number to be used once within the time period that the time signature is usable by.<br />
This prevents what's called a replay attack, where someone steals your cookie and keeps using it for a short period.

There are of course drawbacks to these as well.

<ol>
<li>The cookie cannot be used to authenticate without access to the user database, so if your database is down your entire system is down.</li>
<li>The longer you allow a cookie to be valid for, the more misused a cookie could be, but the shorter it is, the more reauthentication needs to be done, and that tends to interupt user flow (especially if the user just spent an hour crafting a blog post and you needed to reauthenticate their http POST request)</li>
<li>It's very remotely possible to generate the same large random number twice in short succession, and therefore invalidly refuse someone.</li>
<li>You need to generate a new nonce on every request which destroys cachability and you need to track used nonces across servers, which implies shared state (which is a pain if you are crossing data centers for example).</li>
</ol>
<h3></h3>
<h3>Post Script: What about extension attacks?</h3>
There is a category of attacks that can be done on signatures because they are not designed to be cryptographically secure. Signatures like MD5 and SHA1 are designed to be checksums on content, and there is a drawback to that.

<a href="http://vudang.com/2012/03/md5-length-extension-attack/" target="_blank">Extension attacks</a> are able to exploit a weakness in the signature algorithms by using some knowledge about the kind of key you have and the signature sent over the wire. Essentially the problem is that if you are signing "KEY|DATA" =&gt; SIGNATURE, then it's possible to calculate "KEY|DATA|DATA2" =&gt; SIGNATURE2 given only DATA and SIGNATURE.

Our cookie based system is not vulnerable to this form of attack because of two things:

Firstly we append the secret key at the end, meaning that it's not possible to add further data to the string and since you'd be modifying the key not the value string.

Secondly, this sort of attack works only when adding extra data to the string has a valid meaning. If we had put the KEY first, and then appending the name, adding extra data would in most cases not have helped, (and although one could theoretically go from "Michael Brunton" to "Michael Brunton-Spall", the opportunity for exploit is much reduced).

HTTP requests are far more vulnerable to length extension because HTTP allows the same query parameter multiple times, so /login?param1=foo&amp;username=bob&amp;signature=SIG can become /login?param1=foo&amp;username=bob&amp;signature=SIG&amp;username=eric and a lot of web libraries, if you ask for the parameter username will return the second parameter without warning (unless you explicitly ask for multiple values).

Since the signature for API's is often done by removing the signature parameters, arranging in alphabetical order and signing the string, we'd sign "KEY|param1=foo,username=bob" and the attacker can change it to "KEY|param1=foo,username=bob,username=eric".

Thirdly, in order to initialise the signature vector correctly you need to know the length of the secret key.  In our system that's a secret, but it would be trivial to bruteforce search for the key length so I don't think that's much of a defense.

If you are thinking of implementing signed cookies, the way to ensure that you are protected against extension attacks is to ensure that you cannot add extra data to the end of the string. The simplest way to do this is to sign DATA|KEY, and to verify that data does not contain a pipe character. Secondly, ensure that however you build your data string, it should not support multiple key=value parameters, with later instances overriding the values of the earlier parameters without warning.

