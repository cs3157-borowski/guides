# Getting Started in UNIX Coding Environment

Welcome to COMSW 3157 Advanced Programming with Dr. Brian Borowski! Prior to this course, you may have used various coding environments like Codio, VSCode, etc. But this guide will introduce you to the one that we use for AP. It is very important that you read through this guide so that you will be able to do the assignments for this course. 

You will be interacting with the UNIX coding environment via the command-line. 

## What is the "Command-Line"?
The command-line in a UNIX coding environment allows you to interact with the operating system and execute commands. From the command line, you can navigate file systems, run programs, amongst other things. Think of this as a very basic way to interact with an operating system and an alternative to a mouse/trackpad. 

## Logging in to our Class Server 
Sutdents will recieve an account on the server AP, which is a server instance running on Google Cloud Platform. Think of this as signing onto a remote server where you will do all your work instead of working on and saving files to your local machine. You will access the class server using SSH, which allows a secure terminal session with our class server.

For those of you using macOS, we recommend using its [Terminal](https://support.apple.com/guide/terminal/welcome/mac).

On Windows, we recommend using [cygwin](https://www.cygwin.com/), [PuTTY](https://www.putty.org/), [MobaX](https://mobaxterm.mobatek.net/), just to name a few.

Type this into the command-line to establish your connection to our server: 

```yaml
ssh YOUR_UNI@ap.cs.columbia.edu
```
You should be prompted to input your password which you should have recieved in an email. Note, it might seem like you are typing in nothing when typing in your password. 

## Writing Code with a Text Editor
In this course, you will be writing code using a command line text editor on the class server. In previous semesters, most students and professors recommend Vim. But you can do your own research and use any text-editor that you feel comfortable with (for example, Emacs). 

Spend the first few days going through the tutorials for your chosen text-editor. For those of you who choose Vim, you can use the built-in tutorial by running `vimtutor` in the command-line. Getting comfortable with your text-editor is very important for this course and will make it a lot easier when assignments become more complex, so practice early! 


### Acknowledgements 
This guide was originally developed by Jae Woo Lee. 

Phillip Le adapted it in Spring 23
