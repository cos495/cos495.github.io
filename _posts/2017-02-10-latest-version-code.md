---
layout: post
title:  "Getting the latest version of the code"
date:   Fri Feb 10 09:45:44 EST 2017
categories: general
author: "Sebastian Seung"
---

We are occasionally modifying the sample code for the homework assignment as people report bugs/issues. 
If you downloaded the code previously, and now would like to get the latest version, type
```
$ cd code
$ git pull
```
(The first command assumes that you start in the directory containing the `code` folder.  You may need to customize depending on which directory you are currently in.)
Your repo will be updated with the latest changes if there are any.
If there are no changes, you will get the message `Already up-to-date.`

If you are using JuliaBox, go to the Notebook Dashboard and select "Console". You will get a command line prompt, at which you can type the commands above.
(I found Console to be buggy in the Chrome browser, but Firefox works fine.)
