# Getting Started in UNIX Coding Environment

Welcome to COMSW 3157 Advanced Programming with Dr. Brian Borowski! Prior to this course, you may have used various coding environments like Codio, VSCode, etc. But this guide will introduce you to the one that we use for AP. It is very important that you read through this guide so that you will be able to do the assignments for this course.

You will be interacting with the UNIX coding environment via the command line.

## What is the "Command Line"?

The command line in a UNIX coding environment allows you to interact with the operating system and execute commands. From the command line, you can navigate file systems and run programs, among other things. Think of this as a very basic way to interact with an operating system and an alternative to a mouse/trackpad.

## Logging in to our Class Server

Students will receive an account on the AP server, which is a server instance running on Google Cloud Platform. Think of this as logging into a remote computer where you will do all your work instead of working on and saving files to your local computer. You will access the class server using SSH, which allows a secure terminal session with our class server.

For those of you using macOS, we recommend using its [Terminal](https://support.apple.com/guide/terminal/welcome/mac).

On Windows, we recommend using [Windows Terminal](https://learn.microsoft.com/en-us/windows/terminal/), [cygwin](https://www.cygwin.com/), [PuTTY](https://www.putty.org/), or [MobaX](https://mobaxterm.mobatek.net/), just to name a few.

Using one of these programs, type this into the command line to establish your connection to our server:

```yaml
ssh YOUR_UNI@bb.cs.columbia.edu
```

You should be prompted to input your password which you should have received in an email. Note, you will not see the characters you type when entering the password.

## Interacting with the Shell

The application you interact with in your terminal window – the program that prints the command prompt and carries out the commands that you type – is called a “shell”. There are many different shells. Your account is configured to use the Bash shell by default, so if you run `echo $SHELL`, it should tell you that you are using Bash:

```console
$ echo $SHELL
/bin/bash
```

When you type a command into your shell you're running a program, much like clicking on an icon to launch a program on a GUI based operating system. We just used the `echo` program. `echo` prints out the text that you input as command line arguments. For example:

```console
$ echo Hello AP!
Hello AP!
```

There are many other useful commands. In fact, we've already used one — the `ssh` command when logging into the AP server. A few more examples:

```console
$ date
Sat Jan 14 10:56:27 EST 2023

$ ping google.com
PING google.com (142.251.16.101) 56(84) bytes of data.
64 bytes from bl-in-f101.1e100.net (142.251.16.101): icmp_seq=1 ttl=115 time=1.54 ms

$ python3
Python 3.10.7 (main, Nov 24 2022, 19:45:47) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 

$ cowsay Hello AP!
 ___________
< Hello AP! >
 -----------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||
```

In AP, we will typically write command line usage in the above format. The shell prompt, which you type commands after, is represented by a `$`. Text that isn't prepended by a `$` is the output of a program.

When you're not sure what a command does or how to use it, you should consult the `man` pages. If you wanted to figure out what `ping` does and how to use it, you would type `man ping`. Google is also an option.

For a more detailed walkthrough of the command line, we recommend [this video](https://www.youtube.com/watch?v=AWDxfZkGW_w) by a former AP student.

## Writing Code with a Text Editor

In this course, you will be writing code using a command line text editor on the class server. In previous semesters, most students and professors recommend Vim. But you can do your own research and use any text-editor that you feel comfortable with (for example, Emacs). You may, however, be tested on the basics of Vim.

Spend the first few days going through the tutorials for your chosen text-editor. For those of you who choose Vim, you can use the built-in tutorial by running `vimtutor` in the command-line. Getting comfortable with your text-editor is very important for this course and will make it a lot easier when assignments become more complex, so practice early!


### Acknowledgments

This guide was originally developed by Jae Woo Lee.

Phillip Le and Jeremy Carin adapted it in Spring 23.
