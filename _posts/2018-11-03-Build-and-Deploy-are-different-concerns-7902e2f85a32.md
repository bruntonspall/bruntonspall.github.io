---
title: Build and Deploy are different concerns
description: >-
  You’ve spent days crafting the perfect bit of code, and you are ready to put
  it in front of real users.
date: '2018-11-03T23:39:41.743Z'
---

You’ve spent days crafting the perfect bit of code, and you are ready to put it in front of real users.

You hit the build button, and some process takes your code and all the tests and makes sure that it works. In the same place, you hit the deploy button and the code is compiled and shipped to a production machine and everything works just fine right?

It’s 2am, and you are on call. Someone is ringing. You pick up and are told that the site search system isn’t working anymore. Nobody has changed this in months, but something isn’t working. You log in and look at the graphs. Memory use and disk use has been steadily climbing for months, and now the service can’t run. You restart the service but it doesn’t help, it just reloads the indexes from disk. In a fit of pique, you look at the changelog, and yup the last deploy matches with the steadily climbing memory usage, so a rollback will fix your problem.

You hit the deploy button to redeploy the app, but the deploy fails. During the deployment process, the system tried to download an old dependency that doesn’t exist anymore. You search fruitlessly, but trying to roll forward the dependency means branching the code, and the updated version of the library has compatibility issues.

This shouldn’t ever happen. A deployment shouldn’t require rebuilding the thing you are deploying. It should be constant and require no external dependencies.

A build process is exactly what it says, designed to build a relocatable binary package of some form. In the C and C++ days, this meant doing the compile and the link phase, to produce an executable.

But in Python or Ruby, JVM’s and .NET, the blurring of the boundary between compile time dependency and runtime dependency has been blurred. Do you need libssh installed by the operating system or do you do pip install -r requirements.txt at deploy time?

I’d argue that building a relocatable, redeployable artifact is the job of the build process. The only communication between the build process and the deployment process should be the creation of the artifact, and the registration of the artifact metadata.

At deploy time, the only action should be to take the artifact and put it into the running system. It doesn’t matter whether this is to rsync over the php and NOHUP the server, or transfer a self executing jar and run it on the server. From docker containers to go executables, elf binaries to zip files of code, the deployable artifact should remain inviolate for its entire life.

The reason for this is that deployments should be repeatable, consistent and reliable each and every time. Any build step that gets put into that deployment tends to break that model, and cause deployments that cannot be relied upon.

_\[This blogpost is part of an attempt to blog once a day for the entirety of November (#NaBloPoMo), inspired by_ [_Terence Eden_](https://medium.com/u/1b3280d2beeb)_. This series of blogposts are therefore unedited and written on the day. Errors and Omissions Excepted\]_
