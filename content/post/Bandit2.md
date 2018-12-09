---
title: "Bandit Part 2"
date: 2018-12-06T10:06:53-05:00
draft: false
toc: false
comments: false
categories:
- Pentesting
tags:
- OverTheWire
---

Part 2 of me solving the wargame [Bandit](http://overthewire.org/wargames/bandit/).
<!--more-->

We ended last time finding the password for level 6, so that's where we'll start.

Level 6:

The password file is stored _somewhere on the server_ and is owned by `user` bandit7, owned by `group` bandit6 and is 33 bytes in `size`. After looking at the `find` command's `man` page again, it appears there are parameters to search by `-user` and `-group` as well as the `-size` flag that we made use of in the previous challenge.

Putting what we know together, I tried typing in the command `find -user bandit7 -group bandit6 -size 33c` while in the directory we log into and did not find any results. Rereading the hint's _somewhere on the server_, I realized I would probably need to go to the root directory, or the direcotry on a UNIX system that is the parent directory to all directories and files in a UNIX file system.

Typing in the command generated a lot of 'permission denied' and 'no such file or directory' errors. We recall from our undergraduate Systems Programming class that there are a number of errors a Linux system can call, with the two above errors being number error codes 13 and 2 respectively. We also know that each error then generates its own output stream and said output streams can be redirected to `/dev/null` where they are essentially discarded and removed from the output we see generated.

Therefore, after running the command `find  -user bandit7 -group bandit6 -size 33c 2>/dev/null 13>/dev/null` we get a single result file, `./var/lib/dpkg/info/bandit7.password`. After running the `cat` command on it, we receive the password to level 7!

Level 7:

The password for level 8 is stored in the file `data.txt` next to the word `millionth`. This level suggests that we use the command `grep` which searches files for lines that have the given pattern in them. We also know that we can output a file with `cat`. But to get the output as input for `grep` we need to do what's called *piping*. Piping is feeding the output of one command to the input of another. Piping was initially used to run multiple programs in sequence due to the small amount of memory available in early computer models and is performed by typing the `|` symbol between two command calls.

Putting all this together, we call `cat data.txt | grep millionth` to allow grep to search through data.txt without exhausting the server's memory and we find the line with the word `millionth` right next to the password for level 8!

Level 8:

The password for level 9 is stored in the file `data.txt` and is the only line of text that occurs only once. The website suggests we use the command `uniq` which either reports or omits all lines that are repeated throughout a file. We find a flag `-u` that only prints unique lines in the file. But, the `man` page for the `uniq` command notes that `uniq` does not detect repeated lines unless they are adjacent, so sorting the file first with the `sort` command is key.

With what we know, I try `sort data.txt | uniq -u` and get the password for level 9!

Level 9

The password for level 10 is stored in the file `data.txt` and is one of the few human-readable strings beginning with several `=` characters. We try `cat`ing the file, but learn that you cannot because the file is not human-readable. The level recommends using the `strings` command, which prints the strings of printable characters in files. With this we can look at the human-readable strings within the file and then search them using `grep`.

Putting it all together, we type `strings data.txt | grep "=="` and get the following output:

    2========== the
    ========== password
    ========== isa
    ========== <password>

Sweet, onto level 10.

Level 10:

The password for level 11 is stored in the file `data.txt`, which contains `base64` encoded data. The website suggests we use the `base64` command which has a decode `-d` flag to decode any data with `base64` encoding.

Knowing these, we type `base64 -d data.txt` and receive the output:

    The password is <password>

Onto level 11!

Level 11:

The password for level 12 is stored in the file `data.txt` where all lower- and uppercase letters have been rotated by 13 positions. The website suggests using the `tr` command. The `tr` command allows us to translate or delete characters from an input source and then write the results to the standard output.

We read the Rot13 reading material and looked up how to use the command `tr` to solve Rot13-encrypted files and came up with `cat data.txt | tr '[a-zA-Z]' '[n-za-mN-ZA-M]'` to account for both upper- and lowercase characters and receive:

    The password is <password>

Onto level 12!

Level 12:

The password for level 13 is stored in the file `data.txt`, which is a hexdump of a file that has been repeatedly compressed. We must create a directory in `/tmp` if we want to store files we decompress, as we are not allowed to in the home directory. This one looks like it's going to be a long one...challenged accepted.

We start by making a directory in `/tmp` with the command `mkdir /tmp/goo` and copy the `data.txt` file over to it with `cp data.txt /tmp/goo`. We then change to the directory. We need to reverse the hexdump with the `xxd` command the website suggests we use. We see that the `xxd` command can reverse the hexdump by using its `-r` flag.

After typing `xxd -r data.txt > dumped` to output the reversed hexdump into the file `dumped` we then call `file dumped` to assess the file type of `dumped` and see that it holds data compressed by the `gzip` command.

To decompress a `gzip`-compressed file, the file must first have the `gzip` ending `.gz`. So we change `dumped`'s file name to `dumped.gz` with the command `mv` that simply moves the file to the new name: `mv dumped dumped.gz`. Next we use the `gunzip` command to decompress the file: `gunzip dumped.gz` and receive `dumped`. Then we check `dumped`'s file type and find out that the decompressed file is a compressed bzip2 archive with a block size of 900k.

We can decompress a `bzip2`-compressed file by calling the `bzip2` command with the decompress `-d` flag. Typing `bzip2 dumped` gives us the file `dumped.out` that is a `gzip`-compressed archive. We change the name again to `dumped.gz` and decompress the file the same way we did before. The file decompresses to `dumped`, a `tar`-compressed archive....argh!

The `tar` command has a lot of decompression features, and the standard ones to use for decompression are the flags `-x`(untars/extract the files compressed in a tar archive), `-v` (verbosely shows the progress of untarring all the files by listing all of the decompressed file names in the output) and `-f` (tells `tar` that the next argument is the name of the `tar` archive). Using these altogether, `tar -xvf dumped`, we get the archive `data5.bin` which is another `tar` archive! We repeat the decompression process for `tar` files and get `data6.bin`. We repeat the process again to get `data8.bin` and use the `file` command once again to learn `data8.bin` is another `gzip` archive...

Not seeing an end in sight, I change the name of `data8.bin` to `data8.gz` with the `mv` command and decompress the `gzip` file as discussed above and we get `data8`, a ASCII text file!!!!!! 

We `cat` the file and get the tear-inducing line:

    The password is <password>

Finally, onto level 13.

Level 13:

The password for the next level is stored in `/etc/bandit_pass/bandit14` and can only be read by `user bandit14`. We do not get the password for level 14 but instead get a private SSH key that can be used to log onto the next level.

In order to use the SSH key, we need to first `cat` the file and copy its contents to a file on our computer. From what I've found [online](https://unix.stackexchange.com/questions/257590/ssh-key-permissions-chmod-settings), we're supposed to use `chmod` to give any private SSH key you create the `600` privacy permissions, which means only the owner may read and write to the file, so that we can only see what our privacy key is (although it didn't seem to matter for this challenge).

Looking at the `ssh` command some more, we learn that we can use our copied private SSH key as the password for SSH in the command's line rather than waiting for it to ask us what the password is using the `-i` flag. Knowing this, we type the command 'ssh -i sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org' and log into bandit14 successfully! From there we can simply cat the file `/etc/bandit_pass/bandit14' and retrieve the password to log onto level 14 without an SSH key!

Level 14:

The password for level 15 can be retrieved by submitting the password of the current level to `port 30000` on `localhost`. One of the commands the website recommends is using `nc`, which is used to create arbitrary TCP and UDP connections and listens. Judging from examples of its use online, we can use it to connect to a port and a destination IP address, and we can also type `localhost` to connect to our localhost rather than figuring out its IP address! To submit information, the `nc` command requires the port and destination IP address like so: `nc [destination] [port]`.

Besides the `cat` command that prints the content of its input to the standard output stream, there is the `echo` command which simply prints the argument you give it to the standard output stream, such as a password string.

Putting this together, we can use this level's password, the `nc` command, and the `echo` command to submit the password to port 30000 on our localhost: `echo <password> | nc -30000 localhost`.

The password is ours! Onto level 15.

Level 15:

The password for the next level can be retrieved by submitting the password of the current level to `port 30001` on `localhost` using SSL encryption. The website recommends using both the `openssl` and `s_client` commands to connect to the localhost on port 30001. Looking at `openssl`'s `man` page, we see that this command allows us to use SSL/TLS encyption on the following command we call, while `s_client` allows us to create a generic SSL/TLS client that connects to a server using SSL/TLS. THe `s_client` command also allows us to `-connect` to a `host:port`. 

By using `echo` and piping its output into an `openssl s_client`, we can get the password for level 16: `echo <password> | openssl s_client -connect localhost:30001`. This gives us our password, but the website also gives us a hint to use the `-ign_eof` flag, which stops the connection from shutting down when an EOF character is reached in the input...typing `echo <password> | openssl s_client -connect localhost:30001 -ign_eof` gives us the same password. [Researching this](https://www.reddit.com/r/HowToHack/comments/2jixkl/going_through_bandit_wargames_at_overthewire_and/), apparently some people needed to use the `-ign_eof` flag, or the `quiet` flag (since it implies the use of the `-ign_eof` flag), to get the password.

We will walk through levels 16+ in a later post, I hope you enjoyed this post! 