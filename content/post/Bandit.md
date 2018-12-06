---
title: "Bandit Part 1"
date: 2018-12-05T16:14:48-05:00
draft: false
toc: false
comments: false
categories:
- Pentesting
tags:
- OverTheWire
---

My attempt at solving all 34 levels of the wargame [Bandit](http://overthewire.org/wargames/bandit/).
<!--more-->
Bandit is a wargame hosted by overthewire.org and is aimed at absolute beginners, which I am. Below are the write-ups for each level I solved.

Beginning:

We start at level 0, which is simply logging onto the game using SSH to connect to `bandit.labs.overthewire.org` on port `2220` with the username `bandit0` and the password `bandit0`.

Typing `ssh` into the command line showed us the optional flags and the required argument, `destination`:

    ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]
        [-b bind_address] [-c cipher_spec] [-D [bind_address:]port]
        [-E log_file] [-e escape_char] [-F configfile] [-I pkcs11]
        [-i identity_file] [-J [user@]host[:port]] [-L address]
        [-l login_name] [-m mac_spec] [-O ctl_cmd] [-o option] [-p port]
        [-Q query_option] [-R address] [-S ctl_path] [-W host:port]
        [-w local_tun[:remote_tun]] destination [command]`

We know that we have the `user`name, the `host` address and the `port` number, so we type the following line in my terminal: 
`ssh -p 2220 bandit0@bandit.labs.overthewire.org`. 

It asks us for the password, and I'm in: 

        ,----..            ,----,          .---. 
       /   /   \         ,/   .`|         /. ./|
      /   .     :      ,`   .'  :     .--'.  ' ;
     .   /   ;.  \   ;    ;     /    /__./ \ : |
    .   ;   /  ` ; .'___,/    ,' .--'.  '   \' .
    ;   |  ; \ ; | |    :     | /___/ \ |    ' ' 
    |   :  | ; | ' ;    |.';  ; ;   \  \;      : 
    .   |  ' ' ' : `----'  |  |  \   ;  `      |
    '   ;  \; /  |     '   :  ;   .   \    .\  ; 
     \   \  ',  /      |   |  '    \   \   ' \ |
      ;   :    /       '   :  |     :   '  |--"  
       \   \ .'        ;   |.'       \   \ ;     
    www. `---` ver     '---' he       '---" ire.org     
               
    Welcome to OverTheWire!

Level 0:

We need to find a file called `readme` in the home directory of level 0 in order to find the password to get to level 1. Simply typing `ls` shows us all the files and directories in the current directory, showing us there is indeed a `readme` file in the home directory. We type `cat readme` to read the file which only contained the password to get to level 1!

We type `ssh -p 2220 bandit1@bandit.labs.overthewire.org`, enter the password we found in the `readme` file, and...we're in!

For future reference, unless otherwise specified, we will be typing `ssh -p 2220 banditX@bandit.labs.overthewire.org`, where `X` is the next level, in order to connect to the next level and enter the password we found in the current level.

Level 1:

The password to get to the next level is stored in the file `-` located in the home directory. Simply typing `cat -` does not work, as hyphens designate that an flag/option/argument is going to be entered.  

For example, `-p 2220` is a flag for the SSH commands I used to log into each level. Note that each command's flags are different: `-p` does not always denote the user is entering a port number when used in other commands.

Therefore, we need to precede the hyphen with a `./` to denote that the hyphen is being read as the name for a file in the home directory, denoted as `.` in UNIX operating systems,  and not as the beginning of a flag for the `cat` command:

We type `cat ./-` and receive the password for level 2!

Level 2:

The password for the next level is stored in a file called `spaces in this filename` located in the home directory. Unforunately, typing the file name verbatim will make the `cat` command believe each word in the file name is its own argument. Typing a space as `\ ` will tell the terminal that you want the space ASCII character to be used so the terminal does not think you are entering another argument.

Following what we know above, typing `cat spaces\ in\ this\ filename` allows us to view the password stored in the file. Onto level 3!

Level 3:

The password for the next level is in the `inhere` directory, but when we change to the `inhere` directory by typing `cd inhere` and then type `ls` to list the contents of the directory no files are present.

Looking at the `man` page of the `ls` command, we can see that there is a flag `-a, --all` that allows us to see entries in the directory that start with `.`, otherwise known as hidden entries. 

After typing `ls -a`, we see that there is a `.hidden` file. We `cat` the file and find the password to log onto level 4!

Level 4:

The password for the next level is stored in the only human-readable file in this level's `inhere` directory which is also found in the home directory. The `inhere` directory contains ten files labeled `-file00` through  `-file09`.

Human-readable most likely means the file has only ASCII characters as its contents. The `file` command determines the file type of the file selected, and there is also a wildcard character `*` that runs a command through all of the files that match the naming scheme of the letters prepended to `*`. For example, `file ./hello*` will run the `file` command on all files that begin with `hello` such as `hello1.jpg` and `helloworld.txt`.

Knowing this, we run the command `file ./*` and are given the output:

    ./-file00: data
    ./-file01: data
    ./-file02: data
    ./-file03: data
    ./-file04: data
    ./-file05: data
    ./-file06: data
    ./-file07: ASCII text
    ./-file08: data
    ./-file09: data

We `cat ./-file07` and get the password for level 5!

Level 5:

The file we want to find is also in a `inhere` directory and is human-readable, 1033 bytes in size and is not executable. The website suggests that we use the `find` command to find the correct file. Look at the `find` command's `man` page by typing `man find`, we can find files that are `-readable`, files of a certain `-size` in bytes (`-size Xc`, where `X` is a number), and files that are `-executable`. We should also note that adding an exclamation mark before any flag is adding a `not` to the logic statement, i.e. `! -executable` means that we are searching for a file that is not executable.

Putting this all together, we can use the following command to find the file we are looking for: `find -readable -size 1033c ! -executable` and we get only one file that matches these criteria `./maybehere07/.file2`. We open up that file to get the password for level 6!

That's all I've done for now, but I will be posting more level write-ups later.