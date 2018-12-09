---
title: "Bandit Part 3"
date: 2018-12-09T16:43:07-05:00
draft: false
toc: false
comments: false
categories:
- Pentesting
tags:
- OverTheWire
---

Part 3 of my Bandit write-up, starting at level 16.
<!--more-->

Level 16:

The password for level 17 can be found by submitting this level's password to a port on `localhost` in the range `31000` to `32000`. We must first find out which of these ports have a server listening on them, then find out which of these servers speak `SSL`, and finally get a response from the only server in those remaining that will give me level 17's password.

The webpage suggests we use the `nmap` command. After looking at an [nmap cheat sheet](https://hackertarget.com/nmap-cheatsheet-a-quick-reference-guide/), we learn that we can specify the range of ports `-p` to use and detect what standard services `-sV` are being run on the server. Putting this knowledge together, we type the command `nmap -sV -p 31000-32000 localhost` and receive the following output:

    Starting Nmap 7.40 ( https://nmap.org ) at 2018-12-09 22:49 CET
    Nmap scan report for localhost (127.0.0.1)
    Host is up (0.00020s latency).
    Not shown: 999 closed ports
    PORT      STATE SERVICE     VERSION
    31518/tcp open  ssl/echo
    31790/tcp open  ssl/unknown
    1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
    SF-Port31790-TCP:V=7.40%T=SSL%I=7%D=12/9%Time=5C0D8DF6%P=x86_64-pc-linux-g
    SF:nu%r(GenericLines,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20cu
    SF:rrent\x20password\n")%r(GetRequest,31,"Wrong!\x20Please\x20enter\x20the
    SF:\x20correct\x20current\x20password\n")%r(HTTPOptions,31,"Wrong!\x20Plea
    SF:se\x20enter\x20the\x20correct\x20current\x20password\n")%r(RTSPRequest,
    SF:31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\
    SF:n")%r(Help,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x
    SF:20password\n")%r(SSLSessionReq,31,"Wrong!\x20Please\x20enter\x20the\x20
    SF:correct\x20current\x20password\n")%r(TLSSessionReq,31,"Wrong!\x20Please
    SF:\x20enter\x20the\x20correct\x20current\x20password\n")%r(Kerberos,31,"W
    SF:rong!\x20Please\x20enter\x20the\x20correct\x20current\x20password\n")%r
    SF:(FourOhFourRequest,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20c
    SF:urrent\x20password\n")%r(LPDString,31,"Wrong!\x20Please\x20enter\x20the
    SF:\x20correct\x20current\x20password\n")%r(LDAPSearchReq,31,"Wrong!\x20Pl
    SF:ease\x20enter\x20the\x20correct\x20current\x20password\n")%r(SIPOptions
    SF:,31,"Wrong!\x20Please\x20enter\x20the\x20correct\x20current\x20password
    SF:\n");

Seeing this, we know that port 31518 will only `echo` back the password we put in, while the other port's server first checks if we have sent the correct password most likely before sending us level 17's password.

We type `echo cluFn7wTiGryunymYOu4RcffSxQluehd | openssl s_client -connect localhost:31790 -quiet` to retrieve the RSA private key needed to get to level 17. Note: I read other people's walkthroughs of levels 1-15 to see if I missed anything and learned that the `-quiet` argument for `openssl s_client` prevented a lot of unnecessary output in the terminal.

We save a copy of the private key and follow the other steps already outlined in level 13's write-up. We then put it all together by typing in the standard `ssh` command to log into our Bandit servers but also add the `-i` argument with the key we just saved. Onto level 17!

Level 17:

There are two files in the home directory: `passwords.old` and `passwords.new`. The password for the next level is in `passwords.new` and is the only line that has been changed between the two files. The website recommends that we try and use the `diff` command, which outputs the lines in both files that are different from each other.

Typing `diff passwords.old passwords.new`, we receive the following output:

    42c42
    < hlbSBPAWJmL6WFDb06gpTx1pPButblOA
    ---
    > kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd

The ">" character denotes the line difference in the second file, `passwords.new`, which is the password for level 18!

Level 18:

If you noticed that we get logged out of the level 18 server after inputting the login password we acquired in the previous level with the output message, "Byebye !", then you did correctly solve the previous level's challenge the site states. The output and automatic logout is instead related to this level's challenge... D:

The password for level 19 is stored in the home directory's `readme` file like in level 0. The catch here is that the `.bashrc` file has bee modified to log you out when you login via SSH. [After searching for how to login without running `.bashrc`](https://serverfault.com/questions/94503/login-without-running-bash-profile-or-bashrc), I learned that the `-t` argument with `/bin/sh` appended to the command allows us to login without running the `.bashrc` file. This is because the `-t` argument forces a pseudo-terminal allocation that can be used to execute commands on a remote machine such as my laptop.

Once I added the `-t` argument and `\bin\sh` to the end of the command and entered the password to get into level 18, I was able to `cat` the `readme` file and retrieve the password for the next level!

Level 19:

Level 20's password is found in `/etc/bandit_pass`. The website recommends that we use the "setuid binary" in the home directory. After typing `ls`, we can see that there is a red-highlighted file in the home directory called `bandit20-do`. We could not find what the red highlight symbolizes, but we ran the `file` command on the file and discovered that the file is a "setuid executable".

The next step was to see if we could see the files in `/etc/bandit_pass`, and found that we did not have the permissions to access these password files, especially the one that contains the password for the next level `bandit20`. We go back to the home directory and see the permissions each file has using `ls -al` to see all the available files along with their [respective permissions](https://stackoverflow.com/questions/17578647/what-does-terminal-command-ls-l-show) and get the following output:

    total 28
    drwxr-xr-x  2 root     root     4096 Oct 16 14:00 .
    drwxr-xr-x 41 root     root     4096 Oct 16 14:00 ..
    -rwsr-x---  1 bandit20 bandit19 7296 Oct 16 14:00 bandit20-do
    -rw-r--r--  1 root     root      220 May 15  2017 .bash_logout
    -rw-r--r--  1 root     root     3526 May 15  2017 .bashrc
    -rw-r--r--  1 root     root      675 May 15  2017 .profile

Now we can see that the red-highlight means that `bandit20` owns the executable file. We try and run `bandit20` using `./bandit20`, and we get the following output:

    Run a command as another user.
    Example: ./bandit20-do id

We now understand that we can run the command `cat` as user `bandit20` with the `bandit20` setuid executable and open up the file `etc/bandit_pass/bandit20` to get our password! Putting this all together, we type `./bandit20-do cat /etc/bandit_pass/bandit20` in the home directory to get level 20's password!

Level 20:

There is a setuid binary, `suconnect` in the home directory that first makes a connection to `localhost` on the `port` you specify as a command line argument before reading a line of text from the connection and comparing it to the level 20's password. If the password is correct, then the binary file will provide us the password for level 21. It then hints that we should create our own network daemon and try connecting to it.

The website recommends we use the command `nc`, and, after [looking at some of its uses](https://www.computerhope.com/unix/nc.htm), we found that we can create a Netcat daemon that listens `-l` for connections on a certain port `-p` and forwards a message to any clients connecting on said port. After calling `echo "<prevLvlPassword> | nc -l -p 31337` (I hope you find this funny), we then call `suconnect` in the home directory on a separate terminal and use `31337` as the argument for the port we want to listen to, since that is the port our netcat daemon is listening and forwarding a message on. 

After typing `./suconnect 31337` on a separate terminal from the Netcat daemon, the executable connects to the daemon, receives the previous level's password, and outputs the following lines:

    Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
    Password matches, sending next password

Then `suconnect` sends the password over to the Netcat daemon, which outputs the password for level 21!

I hope you enjoyed part 3 of my Bandit walkthrough. Part 4 will include the walkthrough for levels 21-25 as these levels are now sometimes taking a while to figure out and complete.