# Submission Clarifications

The teaching staff noticed that there were a lot of discrepancies between the work that students thought they submitted and what the teaching staff received to grade. The TAs will only grade the code in your handin tag, and **unfortunately we can't make any exceptions to this rule**; however, we want to ensure that you get credited for all the work you do in the future, and you have necessary skillset to obtain a good score in the homework assignments which goes beyond just coding skill.

Let's review the tools you already know about, that will help you ensure that:
1. All your code is in the handin tag.
2. Your directory structure is correct.
3. You have the necessary amount of commits.
4. Your code will compile when TAs are grading.

## The handin tag

After you have tagged your work, and pushed to GitHub, an easy way to ensure that all your work has properly been tagged is to switch the repository view to the handin tag using the leftmost dropdown menu, once the button shows "handin" any files you see are exactly what the TAs will see when grading. Please double check that the files listed reflect what you want the TA's to see. 

![image](https://user-images.githubusercontent.com/101436499/219896712-a43c3016-5b0b-4f8b-875a-0641d5dc7547.png)

## Directory structure

The README for all homeworks will provide the expected directory structure of the submission. For example in HW2, the structure was as follows:

```
part1
    \_ src
        \_ recursive.h
        \_ recursive.c
        \_ iterative.h
        \_ iterative.c
        \_ gcd.c
        \_ Makefile
    \_ instructions.md
part2
    \_ src
        \_ convert.c
        \_ Makefile
    \_ instructions.md
README.md
```

If you recall, the tree command will actually produce a very similar output, so after you are satisfied with your work and have tagged and pushed it, you should run the following sequence of commands to verify your directory structure is as expected.

First, get a clean copy of your repo, by using the clone command with the correct homework number and team number:

```
git clone git@github.com:cs3157-borowski-s23/hw1-team0 ~/cs3157/hw1-submission
```

Then go into the clean copy, and checkout to the handin tag and use the tree command:

```
cd ~/cs3157/hw1-submission
git checkout handin
tree
```

The output should show the same directory structure as the prompt. Note that you are allowed to have extra source files (.h and .c) but not any executables/.o files/anything else extraneous. Furthermore, if the assignment is in C, the file names should show up in white if you are using the default color scheme and .bashrc meaning that they do not have the execute permission bit on.

Note that checking manually on GitHub via inspecting each directory and the files within it after selecting the handin tag as shown above, is also a reliable way to test this.

## Commits

If you cloned a clean version of the repo and checked out to the `handin` tag, you can just use the `git log` command (or `git log --oneline` if it is easier for you to read) to verify your commit history and commit messages. For all submissions you must have at least 5 meaningful commits with relevant commit messages.

Note that checking manually on GitHub on the commits history section (rightmost header above your code) after selecting the `handin` tag as shown above, is also a reliable way to test this.

## Compilation

The TAs will make no effort to compile your code besides running make 

So if you cloned a clean version of the repo and checked out to the handin tag, you should go into the src directories for each part and run nothing but make , which should produce an executable identical to the one the TAs will use for grading. You should run a few tests after generating the executable even if you have tested your code prior to adding the tag and pushing the changes.

## Very Important Reminder

As a general reminder, submitting and testing your code is as important as the development of the homework code itself, so it is critical that you apply the skills above for every submission.

### Acknowledgements

This guide was developed by Xurxo Riesco and edited by Phillip Le for Spring 2023. 
