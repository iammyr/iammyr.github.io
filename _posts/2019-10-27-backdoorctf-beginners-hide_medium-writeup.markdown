---
layout: post
title:  "Backdoor CTF - Beginners - Hidden Flag Medium"
date:   2019-10-27 16:10:14 +0100
categories: ctf
author: iammyr
tags: ctf, reverse-engineering
---

I completed all challenges of the [Backdoor CTF - Beginners section](https://backdoor.sdslabs.co/beginner). <br /><br />
One thing I truly did not know, was that while debugging with gdb, if part of the code is not reached via normal app workflow, then you can force its execution. I learned this thanks to the "hidden flag medium" challenge.
Thanks to the same challenge I also learned how to run 32-bit ELF on a 64-bit architecture.

1. First I checked what type of file I was dealing with:
```
$ file hide_medium 
hide_medium: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.24, BuildID[sha1]=fe9ddc13d0659e1badb3fd04934d02b4aa60893a, not stripped
```
2. Since I have a 64-bit arch, the file would not run. So I did the following:
```
sudo dpkg --add-architecture i386
sudo apt-get update
sudo apt-get install libc6:i386 libncurses5:i386 libstdc++6:i386
```

Then I was able to run the program.

3. So I opened the program with gdb, breakpoint on the main and then run
```
gdb hide_medium
b main
```
Nothing out of this. `info functions` showed the function named `print_flag`. Since the normal program execution does not enter that part of code, we can enforce its execution with the following:
```
b main
r
compile code -- print_flag()
```
This resulted in the flag being printed out.
