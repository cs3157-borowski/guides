# Submission Clarifications

The teaching staff noticed that there were a lot of discrepancies between the work that students thought they submitted and what the teaching staff received to grade. The TAs will only grade the code in your main branch, and **unfortunately we can't make any exceptions to this rule**; however, we want to ensure that you get credited for all the work you do in the future, and you have necessary skillset to obtain a good score in the homework assignments which goes beyond just coding skill.

Let's review the tools you already know about, that will help you ensure that:

1. All your code is in the main branch.
2. Your directory structure is correct.
3. You have the necessary amount of commits.
4. Your code will compile when TAs are grading.

## The main branch

After you have pushed to GitHub, an easy way to ensure that all your work has properly been committed is to go to the Github website an open your repo. Please double check that the files listed reflect what you want the TA's to see.

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

If you recall, the tree command will actually produce a very similar output, so after you are satisfied with your work and have pushed it, you should run the following sequence of commands to verify your directory structure is as expected.

First, get a clean copy of your repo, by using the clone command with the correct homework number and team number:

```
git clone git@github.com:cs3157-borowski-s23/hw1-0 ~/cs3157/hw1-submission
```

Then go into the clean copy, and use the tree command:

```
cd ~/cs3157/hw1-submission
tree
```

The output should show the same directory structure as the prompt. Note that extra source files (.h and .c) and/or any executables/.o files/anything else will be considered extraneous and automatically removed. Furthermore, if the assignment is in C, the file names should show up in white if you are using the default color scheme and .bashrc meaning that they do not have the execute permission bit on.

Note that manually checking on GitHub by inspecting each directory and the files within it is also a reliable way to test this.

## Commits

If you cloned a clean version of the repo, you can just use the `git log` command (or `git log --oneline` if it is easier for you to read) to verify your commit history and commit messages. For all submissions you must have at least 5 meaningful commits with relevant commit messages.

Note that manually checking the commits history section (rightmost header above your code) on GitHub is also a reliable way to test this.

## Compilation

The TAs will make no effort to compile your code besides running `make`. So if you cloned a clean version of the repo, you should go into the src directories for each part and run nothing but make, which should produce an executable identical to the one the TAs will use for grading. You should run a few tests after generating the executable even if you have tested your code prior to pushing the changes.

## Very Important Reminder

As a general reminder, submitting and testing your code is as important as the development of the homework code itself, so it is critical that you apply the skills above for every submission.

### Acknowledgements

This guide was developed by Xurxo Riesco and edited by Phillip Le for Spring 2023. It was modified by Palash Sharma for Summer 2024.
