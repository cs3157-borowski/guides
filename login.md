# Getting Started in the UNIX Coding Environment

Welcome to COMSW 3157 Advanced Programming with Dr. Brian Borowski! Prior to this course, you may have used various coding environments like Codio, VSCode, etc. But this guide will introduce you to the one that we use for AP, the UNIX Coding Environment on our remote class server. It is very important that you read through this guide so that you will be able to complete the assignments for this course. 

You will be interacting with the UNIX coding environment via the command-line. 

## What is the "Command-Line"?
The command-line in a UNIX coding environment allows you to interact with the operating system and execute commands. From the command line, you can navigate file systems, run programs, amongst other things. Think of this as a very simplified way of interacting with an operating system or an alternative to a mouse/trackpad. 

## Logging in to our Class Server 
Students will receive an account on the AP class server, which is an Ubuntu Linux server instance running on Google Cloud Platform. This remote server is where you will do all your work and save all your files. 

For those of you using macOS, we recommend using its [Terminal](https://support.apple.com/guide/terminal/welcome/mac). If you are using a Windows machine , we recommend using [cygwin](https://www.cygwin.com/), [PuTTY](https://www.putty.org/), [MobaX](https://mobaxterm.mobatek.net/), just to name a few. 

Now, to log onto our remote class server, you will need to establish use SSH (Secure SHell).Type this into the command-line to establish your connection to our server: 

```yaml
ssh YOUR_UNI@ap.cs.columbia.edu
```
You should be prompted to input your password which you should have received via email. Don't worry, it will seem like you are not typing anything, but continue entering your password and press ENTER when done. If you followed these steps closely, you should see this message appear: 
```yaml
                  __                             __      
   _      _____  / /________  ____ ___  ___     / /_____ 
  | | /| / / _ \/ / ___/ __ \/ __ `__ \/ _ \   / __/ __ \
  | |/ |/ /  __/ / /__/ /_/ / / / / / /  __/  / /_/ /_/ /
  |__/|__/\___/_/\___/\____/_/ /_/ /_/\___/   \__/\____/ 
                                                       
                     :::     :::::::::  
                   :+: :+:   :+:    :+: 
                  +:+   +:+  +:+    +:+ 
                 +#++:++#++: +#++:++#+  
                 +#+     +#+ +#+        
                 #+#     #+# #+#        
                 ###     ### ###        

```

## Text Editors
In this course, you will be writing code using a command line text editor on the class server. In previous semesters, most students and professors recommend Vim. But you can do your own research and use any text-editor that you feel comfortable with (for example, Emacs). 

Spend the first few days going through the tutorials for your chosen text-editor. For those of you who choose Vim, you can use the built-in tutorial by running `vimtutor` in the command-line. Getting comfortable with your text-editor is very important for this course and will make it a lot easier when assignments become more complex, so practice early! 


### Acknowledgements 
This guide was originally developed by Jae Woo Lee. 

Adaptations by Phillip Le for Spring 23

_Last Edited 1/12/23_
