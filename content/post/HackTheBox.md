---
title: "Hack The Box Registration"
date: 2018-12-08T22:55:13-05:00
draft: false
toc: false
comments: false
categories:
- Pentesting
tags:
- HackTheBox
---

Registering for this was a lot harder than I thought...
<!--more-->

![Asks for invite code.](/HackTheBoxPost/InitialRequest.png)

As you can see, one must first provide an invite code in order to register for an account. Unfortunately for me, I have never received one...

Because this is a website dedicated to running boxes and other challenges to crack, it would make sense that they require you to first prove your resolve before having any access to the site's goodies.

From previous front-end development experience, I knew the Google Chrome browser has Developer Tools that allow its users to inspect a webpage's elements as well as the JavaScript files they run. Upon opening the Tools, their console generated this motivational spin on the "Keep Calm" meme.

![Funny console screen.](/HackTheBoxPost/ConsoleScreen.png)

I first checked out the registration form element and saw that it contained a complex token embedded in it:

![Form element.](/HackTheBoxPost/Form.png)

I had no idea what the token did, so I first tried entering the value of the token, which failed. I then tried changing the value of the token and received a warning message:

![Warning message.](/HackTheBoxPost/XSRFFail.png)

Guess I should've not fooled around...

After looking through the other elements on the webpage, I began to look at its JavaScript files, starting with `/js/inviteapi.min.js`. I opened the file to find an ugly-looking, complex function that desperately needed a makeover. 

![Ugly function.](/HackTheBoxPost/UglyFunc.png)

After browsing some sites and trying a few different software, I found the Online JavaScript Beautifier. I copied the ugly function onto it, pressed the "Beautify JavaScript or HTML" button, and learned that there were actually two independent functions within that min.js file!

 ![Beautiful function.](/HackTheBoxPost/OnlineJavascriptBeautifier.png)

Since I needed some way to either retrieve or make the invite code I required to register a new account on hackthebox.eu, I looked at the `makeInviteCode()` function and saw that it makes a `POST` request to `https://www.hackthebox.eu/api/invite/how/to/generate`. I then looked up the linux command that would allow me to make a post request to a website and [found](https://askubuntu.com/questions/299870/http-post-and-get-using-curl-in-linux) that the `curl` command with the `-X POST` provides this functionality.

I sent the initial `curl` POST request and retrieved some `"data"` that was encrypted using `base64` encryption. One quick Google search later, and I used the `echo` command to pipe the encrypted data into the `base64` command with the `--decode` flag set to true. I then received the message to make another POST request to `https://www.hackthebox.eu/api/invite/generate'.

![Initial curl post.](/HackTheBoxPost/InitialCurlPOST.png)

After making this second POST request, I received a code that I then tried to us as the invite code, which was unsuccessful. 

![Second curl post.](/HackTheBoxPost/SecondCurlPOST.png)

Assuming correctly, I tried the `base64` decoding method I used above and retrieved what I hoped to be the final code.

![Final code!](/HackTheBoxPost/FinalCode.png)

And SUCCESS!!! :D I could now successfully register an account on hackthebox.eu!

![SUCCESS](/HackTheBoxPost/SUCCESS.png)

I'll be trying some of their easier CTF challenges, and maybe boxes, once I finish Bandit up, but I wanted to give this a go and it only took a few hours. :) I hope you enjoyed reading this as enjoyable as I found solving the challenge.