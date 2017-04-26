# Git and GitHub

> Proof rather than argument. - Japanese proverb

## What are Git and GitHub?

Git is a command line program which allows you to track versions of any code or
plain text documents that you create. Like the "track changes" feature of
a word processor Git keeps track of who made particular changes, the time and 
date of those changes, and where the changes were made. If a critical file gets
deleted by accident, or if you make a breaking change to your code and you want
to try to figure out where the breaking change was made, you can use Git to
restore the deleted file or find the new bug in your program. Git organizes
groups of files that you're tracking into a **repository**, which is just
directory where all of the changes to files in that directory are tracked. Git
can also help your collaborate with others when you're writing software. As
[Karl Broman](https://twitter.com/kwbroman) says
(paraphrasing [Mark Holder](https://twitter.com/mtholder)):
"Your closest collaborator is you six months ago, but you don’t reply to 
emails."

GitHub is a website that provides remote Git repositories. If you're working
on code together with a friend GitHub can help you sync changes to code files
between you and your friend. There's also a social and community aspect to 
GitHub, since you can watch other programmers develop their projects. GitHub 
also makes it easy to jump in and help somebody with their project. GitHub
offers many other useful features which will discuss at length.

## Setting Up Git and GitHub

Before setting up Git, go to [GitHub](https://github.com/) and create a free
account. Take note of which email address you use and which username you choose.

To see if you have Git installed open up your terminal and enter the following:


```bash
git --version
```

```
## git version 2.11.0 (Apple Git-81)
```

If you don't get a response back telling you the version of Git that you have
installed then you need to install Git. You can find instructions for installing
Git on your operating system 
[here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).

Open up your shell once you have git installed and run `git --version` again to
make sure that installation succeeded (you may need to restart your shell or
your computer). After Git is installed we need to set up two environmental
variables, but we only need to do this once. The first variable we need to set
up with Git is your GitHub user name, and the second variable is the email 
address that you used to create your GitHub account:


```bash
git config --global user.name "myUserName"
git config --global user.email myName@email.com
```


## Getting Started with Git

Let's create our first Git repository. First we need to create a directory:


```bash
cd
mkdir my-first-repo
cd my-first-repo
```

"Repo" in this case is just shorthand for "repository." To start tracking files
with Git in a directory enter `git init` into the command line:


```bash
git init
```

```
## Initialized empty Git repository in /Users/sean/my-first-repo/.git/
```

You've just created your first repository! Now let's create a file and start
tracking it.


```bash
echo "Welcome to My First Repo" > readme.txt
```

Now that we've created a file in this Git repository, let's use `git status` to
see what's going on in this repository. We'll be using `git status` continuously
throughout this chapter in order to get information about the status Git repository.


```bash
git status
```

```
## On branch master
## 
## Initial commit
## 
## Untracked files:
##   (use "git add <file>..." to include in what will be committed)
## 
## 	readme.txt
## 
## nothing added to commit but untracked files present (use "git add" to track)
```

As you can see `readme.txt` is listed as an untracked file. In order to let Git
know that you want to track this file we need to use `git add` with the name of
the file that we want to track. Let's start tracking `readme.txt`:


```bash
git add readme.txt
```

Git now knows to track any changes to `readme.txt` let's see how the status of
the repository has changed:


```bash
git status
```

```
## On branch master
## 
## Initial commit
## 
## Changes to be committed:
##   (use "git rm --cached <file>..." to unstage)
## 
## 	new file:   readme.txt
## 
```

Git is now tracking `readme.txt`, or in Git-specific language `readme.txt` is
now **staged**. Between the parentheses in the message above you can see that
`git status` is giving us a tip about how to unstage (or un-track) this file,
which we could do with `git rm --cached readme.txt`. Let's unstage this file
just to see what happens:


```bash
git rm --cached readme.txt
```

```
## rm 'readme.txt'
```


```bash
git status
```

```
## On branch master
## 
## Initial commit
## 
## Untracked files:
##   (use "git add <file>..." to include in what will be committed)
## 
## 	readme.txt
## 
```

Our repository is right back to the way it started with `readme.txt` as an
unstaged file. Let's start tracking `readme.txt` again so we can move on to
cooler Git features.


```bash
git add readme.txt
```

Now that Git is tracking `readme.txt` we need to create a milestone to indicate
the changes that we made to `readme.txt`. In this case, the changes that we made
were creating the file in the first place! This milestone is called a **commit**
in Git. A commit logs the content of all of the currently staged files. Right
now we only have `readme.txt` staged so let's commit the creation of this file.
When making a Git commit, we need to write a commit message which is specified
after the `-m` flag. The message should breifly describe what changes you've
made since the last commit.


```bash
git commit -m "added readme.txt"
```

```
## [master (root-commit) 73e53ca] added readme.txt
##  1 file changed, 1 insertion(+)
##  create mode 100644 readme.txt
```

The message above confirms that the commit succeeded and it summarizes the
changes that took place since the last commit. As you can see in the message we
only changed one file, and we only changed one line in that file. Let's run
`git status` again to see the state of our repository after we've made the first
commit:


```bash
git status
```

```
## On branch master
## nothing to commit, working tree clean
```

All of the changes to the files in this repository have been committed! Let's
add a few more files to this repository and commit them.


```bash
touch file1.txt
touch fil2.txt
ls
```

```
## file1.txt
## fil2.txt
## readme.txt
```

While we're at it let's also add a new line of text to `readme.txt`:


```bash
echo "Learning Git is going well so far." >> readme.txt
```

Now that's we've added two more files and we've made changes to one file let's
take a look at the state of this repository.


```bash
git status
```

```
## On branch master
## Changes not staged for commit:
##   (use "git add <file>..." to update what will be committed)
##   (use "git checkout -- <file>..." to discard changes in working directory)
## 
## 	modified:   readme.txt
## 
## Untracked files:
##   (use "git add <file>..." to include in what will be committed)
## 
## 	fil2.txt
## 	file1.txt
## 
## no changes added to commit (use "git add" and/or "git commit -a")
```

We can see that Git has detected that one file has been modified, and that there
are two files in this directory that it is not tracking. Now we needd to tell
Git to track the changes to these files. We could tell Git to track changes to
each file using `git add`, or since all of the files in this repository are
`.txt` files we could use a wildcard and enter `git add *.txt` into the
console. However if we want to track all of the changes to all of the files in
our directory we shoudl use the command `git add -A`.


```bash
git add -A
git status
```

```
## On branch master
## Changes to be committed:
##   (use "git reset HEAD <file>..." to unstage)
## 
## 	new file:   fil2.txt
## 	new file:   file1.txt
## 	modified:   readme.txt
## 
```

Now the changes to all of the files in this repository are being tracked.
Finally let's commit these changes:


```bash
git commit -m "added two files"
```

```
## [master 53a1983] added two files
##  3 files changed, 1 insertion(+)
##  create mode 100644 fil2.txt
##  create mode 100644 file1.txt
```

Darn it, now looking at this commit summary I realize that I have a typo in one
of the names of my files! Thankfully we can undo the most recent commit with 
the command `git reset --soft HEAD~`:


```bash
git reset --soft HEAD~
git status
```

```
## On branch master
## Changes to be committed:
##   (use "git reset HEAD <file>..." to unstage)
## 
## 	new file:   fil2.txt
## 	new file:   file1.txt
## 	modified:   readme.txt
## 
```

This repo is now in that exact same state it was right before we made the 
commit. Now we can rename `fil2.txt` to `file2.txt`, then let's look at the
status of the repository again.


```bash
mv fil2.txt file2.txt
git status
```

```
## On branch master
## Changes to be committed:
##   (use "git reset HEAD <file>..." to unstage)
## 
## 	new file:   fil2.txt
## 	new file:   file1.txt
## 	modified:   readme.txt
## 
## Changes not staged for commit:
##   (use "git add/rm <file>..." to update what will be committed)
##   (use "git checkout -- <file>..." to discard changes in working directory)
## 
## 	deleted:    fil2.txt
## 
## Untracked files:
##   (use "git add <file>..." to include in what will be committed)
## 
## 	file2.txt
## 
```

We previously told Git to track `fil2.txt`, and we can see that Git acknowledges
that the file has been deleted. We can bring Git up to speed with what files it
should be tracking with `git add -A`:


```bash
git add -A
git status
```

```
## On branch master
## Changes to be committed:
##   (use "git reset HEAD <file>..." to unstage)
## 
## 	new file:   file1.txt
## 	new file:   file2.txt
## 	modified:   readme.txt
## 
```

Finally we got the file names right! Now let's make the correct commit:


```bash
git commit -m "added two files"
```

```
## [master 12bb9f5] added two files
##  3 files changed, 1 insertion(+)
##  create mode 100644 file1.txt
##  create mode 100644 file2.txt
```

That looks much better.

### Summary

- Git tracks changes to plain text files (code files and text documents).
- A directory where chanegs to files are tracked by Git is called a Git
repository.
- You can track changes to a file using `git add [names of files]`.
- You can create a milestone about the state of your files using `git commit -m "messahe about changes since the last commit"`.
- To examine the state of files in your repository use `git status`.

### Exercises

1. Start a repository in a new directory.
2. Create a new file in your new Git repository. Make sure Git is tracking the
file and then create a new commit.
3. Make changes to the file, and then commit these changes.
4. Add two new files to your repository, but only commit one of them. What is
the status of your repository after the commit?
5. Undo the last commit, add the untracked file, and redo the commit.

## Important Git Features

### Gitting Help, Logs, and Diffs

Git commands have their own `man` pages. You can access them with
`git help [name of command]`. For example here's the start of the help page
for `git status`:


```bash
git help status
```

```
GIT-STATUS(1)                                Git Manual                               GIT-STATUS(1)

NAME
       git-status - Show the working tree status

SYNOPSIS
       git status [<options>...] [--] [<pathspec>...]

DESCRIPTION
       Displays paths that have differences between the index file and the current HEAD commit,
       paths that have differences between the working tree and the index file, and paths in the
       working tree that are not tracked by Git (and are not ignored by gitignore(5)). The first
       are what you would commit by running git commit; the second and third are what you could
       commit by running git add before running git commit.
```

Just like any other help page that uses `less`, you can return to the prompt
with the `Q` key.

If you want to see a list of your Git commits, enter `git log` into the console:


```bash
git log
```

```
## commit 12bb9f53b10c9b720dac8441e8624370e4e071b6
## Author: seankross <sean@seankross.com>
## Date:   Fri Apr 21 15:23:59 2017 -0400
## 
##     added two files
## 
## commit 73e53cae75301ce9b2802107b1956447241bb17a
## Author: seankross <sean@seankross.com>
## Date:   Thu Apr 20 14:15:26 2017 -0400
## 
##     added readme.txt
```

If you've made many commits to a repository you might need to press the `Q` key
in order to get back to the prompt. Each commit has its time, date, and commit
message recorded, along with a SHA-1 hash that uniquely identifies the commit.

Git can also help show the differences between unstaged changes to your files
compared to the last commit. Let's add a new line of text to `readme.txt`:


```bash
echo "The third line." >> readme.txt
git diff readme.txt
```

```
## diff --git a/readme.txt b/readme.txt
## index b965f6a..a3db358 100644
## --- a/readme.txt
## +++ b/readme.txt
## @@ -1,2 +1,3 @@
##  Welcome to My First Repo
##  Learning Git is going well so far.
## +I added a line.
```

As you can see a plus sign shows up next to the added line. Now let's open up
this file in a text editor so we can delete the second line.


```bash
nano readme.txt
# Delete the second line
git diff readme.txt
```

```
## diff --git a/readme.txt b/readme.txt
## index b965f6a..e173fdf 100644
## --- a/readme.txt
## +++ b/readme.txt
## @@ -1,2 +1,2 @@
##  Welcome to My First Repo
## -Learning Git is going well so far.
## +I added a line.
```

A minus sign appears next to the line we deleted. Let's take a look at the
status of our directory at this point.


```bash
git status
```

```
## On branch master
## Changes not staged for commit:
##   (use "git add <file>..." to update what will be committed)
##   (use "git checkout -- <file>..." to discard changes in working directory)
## 
## 	modified:   readme.txt
## 
## no changes added to commit (use "git add" and/or "git commit -a")
```

If you read the results from `git status` carefully you can see that we can take
this repository in one of two directions at this point. We can either `git add`
the files we've made changes to in order to track those changes, or we can
use `git checkout` in order to remove all of the changes we've made to a file
to restore its content to what was present in the last commit. Let's remove
our changes to see how this works.


```bash
cat readme.txt
```

```
## Welcome to My First Repo
## I added a line.
```


```bash
git checkout readme.txt
cat readme.txt
```

```
## Welcome to My First Repo
## Learning Git is going well so far.
```

And as you can see the changes we made to `readme.txt` have been undone.

Sometimes you might have files that you never want Git to track, for example
binary files that are generated as by-products of running code (PDFs or images),
or secrets like passwords or API keys. A file in your Git repository called 
`.gitignore` can list names of files and sub-folders, or simple regular
expressions (whatever you can use with `ls`) in order to specify files which
should be never tracked. Each line of a `.gitignore` file should specify a file
or group of files that should not be tracked by Git. Let's make a `.gitignore`
file to make sure that we never track image files in this repository:


```bash
touch toby.jpg
git status
```

```
## On branch master
## Untracked files:
##   (use "git add <file>..." to include in what will be committed)
## 
## 	toby.jpg
## 
## nothing added to commit but untracked files present (use "git add" to track)
```

Now that we're added an image to our repository, let's add a `.gitignore` file
to make sure Git doesn't track these kinds of files.


```bash
echo "*.jpg" > .gitignore
git status
```

```
## On branch master
## Untracked files:
##   (use "git add <file>..." to include in what will be committed)
## 
## 	.gitignore
## 
## nothing added to commit but untracked files present (use "git add" to track)
```

Now we can see that Git has detected the new `.gitignore` file, but it doesn't
see `toby.jpg`. Let's add and commit our `.gitignore` file:


```bash
git add -A
git commit -m "added gitignore"
```

```
## [master adef548] added gitignore
##  1 file changed, 1 insertion(+)
##  create mode 100644 .gitignore
```

Now if we add another `.jpg` file, Git will not see the file:


```bash
touch bernie.jpg
git status
```

```
## On branch master
## nothing to commit, working tree clean
```


```bash
ls
```

```
## bernie.jpg
## toby.jpg
## file1.txt
## file2.txt
## readme.txt	
```

### Summary

- `git help` allows you to read the `man` pages for specific Git commands.
- `git log` will show you your commit history.
- `git diff` displays what has changed between the last commit and your current
untracked changes.
- You can specify a `.gitignore` file in order to tell Git to not track certain
files.

### Exercises

1. Look at the help pages for `git log` and `git diff`.
2. Add to the `.gitignore` you already started to inlude a specific file name,
then add that file to your repository.
3. Create a file that contains the Git log for this repository. Use `grep` to
see which day of the week most of the commits occurred on.

## Branching

Branching is one of the most powerful features that Git offers. Creating
different Git branches allows you to work on a particular feature or set of
files independently from other "copies" of a repository. That way you and a
friend can work on different parts of the same file on different braches, and
then Git can help you elegantly merge your branches and changes together.

You can list all of the available branches with the command `git branch`:


```bash
git branch
```

```
## * master
```

The star (`*`) indicates which branch you're currently on. The default branch
that is created is always called *master*. Usually people use this branch as the
working version of the software that their writing, while they develop new and
potentially unstable features on other branches.

To add a branch we'll also use the `git branch` command, followed the name of
the branch we want to create:


```bash
git branch my-new-feature
```

Now let's enter `git branch` again to confirm that we've created the branch:


```bash
git branch
```

```
## * master
## my-new-feature
```

We can make `my-new-feature` the current branch using `git checkout` with the
name of the branch:


```bash
git checkout my-new-feature
```

```
## Switched to branch 'my-new-feature'
```


```bash
git branch
```

```
##   master
## * my-new-feature
```

If we look at `git status` we can also see that it will tell us which branch
we're on:


```bash
git status
```

```
On branch my-new-feature
nothing to commit, working tree clean
```

We can switch back to the `master` branch using `git checkout`:


```bash
git checkout master
```

```
## Switched to branch 'master'
```


```bash
git branch
```

```
## * master
##   my-new-feature
```

Now we can delete a branch by using the `-d` flag with `git branch` and the name
of the branch we want to delete:


```bash
git branch -d my-new-feature
```

```
## Deleted branch my-new-feature (was adef548).
```


```bash
git branch
```

```
## * master
```

Let's create a new branch for adding a section to the `readme.txt` in our
repository. We can create a new branch and switch to that branch at the same
time using the command `git checkout -b` and the name of the new branch we want
to create:


```bash
git checkout -b update-readme
```

```
## Switched to a new branch 'update-readme'
```



## Markdown

## GitHub
