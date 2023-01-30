# Git 101

Git is a source code version control system. Such a system is most
useful when you work in a team, but even when you’re working alone,
it’s a very useful tool to keep track of the changes you have made to
your code.

In this class, you are required to use Git for doing your homework
assignments. You will use Git not only for coding
your homeworks, but also for cloning the assignment repos and
submitting your code.

This tutorial covers not only the basic git operations that you need,
but also the workflow between you, the TAs, and your partner -- from our
preparation of an assignment all the way to the grading of your
submissions by the TAs. Even if you are already familiar with git,
you may find the description of the workflow helpful.

# Setting Up and Configuring Your Git Environment

Before we are able to begin, make sure you follow the following instructions. 

## Set `EDITOR` environment variable

Type `echo $EDITOR`. If the shell does not respond with the name of
your editor -- vim, emacs, or nano -- add the following line at the end of
the `.bashrc` file in your home directory:

```bash
export EDITOR=your_choice_of_editor
```

After saving your `.bashrc` file, run `source ~/.bashrc`
in the command line to make sure that your modification
to `.bashrc` has taken effect.

## Configure your git environment

Tell git your name and email:

```bash
git config --global user.name "Your Full Name"
git config --global user.email your_uni@columbia/barnard.edu
```

git stores this information in `~/.gitconfig`

# Git Workflow for AP

Now that you have configured your Git environment and done the neccessary setup, let's go through how to do the assignments using Git. Before we go on, let's talk about **commits** which are integral to Git's use of workflow management. 

### What is a Git Commit?
Git commit's are a powerful aspect of Git which makes it a great workflow manager. Think of commits as "snapshots" of your code that you (with the use of git commands) are capturing throughout the various stages of development. Most git commands are oriented around updating and interacting with git commits. Let's move on.

## Getting Skeleton Code 

Whether you are working as a team or individual, you will be invited to a Git repository, named hw#-team#. This is your repository where you will do all your work. Accessing the repository via github.com, you are accessing the remote repository, and in order for us to work, we have to create a local copy, what we refer to as your **Local Repository**. To do that: 

1. Click the green button "Code" and make sure you select SSH, now copy that link. 
2. In your command-line, run `git clone <paste-link>` 
 
Each group member should do this so that you each have a local copy to work on.At this point, you have cloned your remote repository and have created your local repository. Now, let's go over the local repository structure. 

### Local Repository Structure 
Once running the above command, you now have a local copy of your remote repository a.k.a a local repository. What now? Git is a workflow manager that is great for tracking changes that you are making in git tracked files. Git does by structuring your local repository with these three components: 

1. Working Directory
2. Staging Area 
3. HEAD

### Files in your Git Repository
We will go over what these mean soon, but we have to introduce another aspect to this whole scheme. That is, file status in a directory under git revision control (like your fresh local repository that you cloned). **Files** under git revision control directories can be described in four ways

1.  Untracked - A little bit decievingFiles that are not under git revision control, a little bit decievin
2.  Tracked, unmodified - The file is in the git repository, and it has not been modified since the last commit.
3.  Tracked, modified, but unstaged - This is a file that is under git revision, you have made changes but have not staged these changes
4.  Tracked, modified, and staged - This is a file under git revision, you made changes, and staged these changes. 

All together: 

1. Files in your **Working Directory** can be described as
    a. Untracked files 
    b. Tracked and unmodified files
    c. Tracked, modified, but unstaged files
2. Staging Area 
    a. Tracked, modified, and staged. 
3. HEAD


The local repository in Git is a representation of your project's code and its evolution over time. It's made up of three main components: the HEAD (reference to the most recent "commit" think, the most recent revision to your code), the working directory (where changes are made), and the staging area (where changes are prepared for commit). Moving changes through these components is like saving mini "versions" of your work, with Git tracking the differences between them. To make changes in the working directory the current revision, you stage them, move them to the staging area, and then commit them to the local repository.


## The tracked, the modified, and the staged

A file in a directory under git revision control is either tracked or
untracked. A tracked file can be unmodified, modified but unstaged,
or modified and staged. Confused? Let’s try again.



The staging area is also called the "index".

## Other useful git commands

Here are some more git commands that you will find useful.

To rename a tracked file:

```bash
git mv old-filename new-filename
```

To remove a tracked file from the repository:

```bash
git rm filename
```

The `mv` or `rm` actions are automatically staged for you, but you still
need to `git commit` your actions.

Sometimes you make some changes to a file, but regret it, and want to
go back to the version last committed. If the file has not been
staged yet, you can do:

```bash
git checkout -- filename
```

If the file has been staged, you must first unstage it:

```bash
git reset HEAD filename
```

There are two ways to display a manual page for a git command. For
example, for the `git status` command, you can type one of the
following two commands:

```bash
git help status
man git-status
```

Lastly, `git grep` searches for specified patterns in all files in the
repository. To see all places you called `printf()`:

```bash
git grep printf
```

## Cloning a project

You created a brand new project in the test1 directory, added a file,
and modified the file. But more often than not, a programmer starts
with an existing code base. When the code base is under git version
control, you can \*clone\* the whole repository. This is in fact what
you will do to start your homework assignments from my skeleton code.
Let’s move up one directory, clone test1 into test2, and `cd` into the
test2 directory:

```bash
cd ..
git clone test1 test2
cd test2
```

Type `ll` to see that your `hello.c` file is cloned here. Moreover, if
you run `git log`, you will see that the whole commit history is
replicated here. `git clone` not only copies the latest version of
the files, but also copies the entire repository, including the entire
commit history. After cloning, the two repositories are
indistinguishable.

Let’s make some changes -- and let’s be bad. Edit `hello.c` to replace
`printf` with `printf%^&`, save and commit:

```bash
vim hello.c
git add hello.c
git commit -m "hello world modification - work in progress"
```

Now run `git log` to see your recent commit carrying on the commit
history that was cloned. If you want to see only the commits after
cloning:

```bash
git log origin..
```

Of course you can add `-p` and `--color` to see the full diff in color:

```bash
git log -p --color origin..
```

Let’s make one more modification. Fix the `printf`, and perhaps change
the "bye world" to "rock my world" while we’re there.

```bash
vim hello.c
git add hello.c
git commit -m "fixed typo & now prints rock my world"
```

Run `git log -p --color origin..` again to see the two commits you
have made after cloning.

## Adding a directory into your repository

After the homework deadline, we will publish the solution by adding a subdirectory to the solution repository. Let’s simulate that
process. Go into the original test1 directory, and make the solution
subdirectory and create two files in it:

```bash
cd ../test1
mkdir solution
cd solution
cp ../hello.c .
echo 'hello:' > Makefile
```

Type `ll` to see that two files (`Makefile` and `hello.c`) have been created
in the `solution` directory. (`hello.c` was copied from the parent
directory, and Makefile was created directly on the command line using
the echo command. BTW, the Makefile contains a single line, `hello:`.
Can you see why this is a legitimate Makefile?)
Now, move out of the solution directory, and `git add` and `git commit` the
solution directory:

```bash
cd ..
git add solution
git commit -m "added solution"
```

Note that `git add solution` stages all files in the directory.

## Creating tags from the command line

There are two steps to go when submitting your homework in this class.
First, you have to "tag" the commit
you want to hand in before "pushing" it to the remote server.
When you use git tag <tagname>, Git will create a tag at the current revision
but will not prompt you for an annotation. It will be tagged without a message.
When you use git tag -a <tagname> -m <msg>
Git will tag the commit and annotate it with the provided message.
For example, I finish my hw0 and tend to submit

```bash
git tag -a handin -m "hw0 submission"
```

Git will tag the commit and annotate it with the provided message
with message "hw0 submission" and tag it as "handin".

To delete a tag on your local repository, you can use git tag -d <tagname>

```bash
git tag -d handin
```

It can remove the handin tag just made in your local machine.

## Pushing commits to a remote repository

Assuming you didn't delete the handin tag, you are ready to push
your "hw0 submission" to the remote server.
The `git push` command takes two arguments:
    A remote name, for example, origin
    A branch name, for example, main

```bash
git push origin main
```

will do the job, and now your submission is on the remote server.
If you want to re-submit after pushing, you have to delete the
old tag in the remote server first:

```bash
git push --delete origin handin
```

and then remove the local handin tag before tagging the new commit for another push.

## Pulling changes from a remote repository

Once you hear that the homework solution is available, you will want to
retrieve it and take a look. You do that by "pulling" the changes in
my repository into your repository. Let’s pull the changes we just
made in `test1` into `test2`:

```bash
cd ../test2/
git pull
```

The `git pull` command looks at the original repository that you
cloned from, fetches all the changes made since the cloning, and
merges the changes into the current repository. You now have the
solution right in your repository.

## Branches

With group assignments, we recommend that you push work to branches first, and then merge back into main once your group members have reviewed the code. As an example, suppose that you are working on a part of the assignment, you can create a branch separate from main by doing the following:

`$ git checkout main`
`$ git checkout -b <branch-name>`

You can then commit your changes, and push to the branch by doing the following:

`$ git push origin <branch-name>`

This will allow multiple members of the team to work on separate features in parallel. When the feature you are working on is complete, you may then create a pull request to allow your team members to review the code, and finally merge the changes back into master. You can read more about using branches and pull requests from [GitHub’s own documentation](https://help.github.com/articles/proposing-changes-to-your-work-with-pull-requests/).

## Learning more about git

This tutorial covers everything you need to do your homework assignments.
Git is an extremely powerful tool and a beautifully designed piece of
software. If you want to learn more about it, start with the official
git tutorial:

```bash
man gittutorial
```

There are pointers to further documentations at the end of the tutorial.

The documentation page of the Git web site has many links as well:
[http://git-scm.com/documentation](http://git-scm.com/documentation)

### Acknowledgments

This guide was originally developed by Jae Woo Lee.

Leslie Chang adapted it in Spring 23.
