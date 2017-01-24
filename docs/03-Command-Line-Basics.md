# Command Line Basics

## Hello Terminal!

Once you have opened up Terminal or Git Bash then you should see a window that 
looks something like this:

![](img/shell1.png)

What you're looking at is the bash shell! Your shell will surely look different 
than mine, but all bash shells have the same essential parts. As you can see in
my shell it says `seans-air:~ sean$`. This string of characters is called the
**prompt**. You type command line commands after the prompt. The prompt is just
there to let you know that the shell is ready for you to type in a command.
Press `Enter` on your keyboard a few times to see what happens with the prompt.
Your shell should now look like this:

![](img/shell2.png)

If you don't type anything after the prompt and you press enter then nothing
happens and you get a new prompt under the old prompt. The white rectangle 
after the prompt is just a cursor that allows you to edit what you've typed
into the shell. Your cursor might look like a rectangle, a line, or an
underscore, but all cursors behave the same way. After typing something into
the command line you can move the cursor back and forth with the left and right
arrow keys.

Don't you think your shell looks messy with all of those old prompts? Don't
worry, you're about to learn your first shell command which will clear up your
shell! Type `clear` at the prompt and then hit enter. Voila! Your shell
is back to how you started.

Every command line command is actually a little computer program, even commands
as simple as `clear`. These commands all tend to have the following structure:

```
[command] [options] [arguments]
```

Some simple commands like `clear` don't require any options or arguments.
Options are usually preceded by a hyphen (`-`) and they tweak the behavior
of the command. Arguments can be names of files, raw data, or other options
that the command requires. A simple command that has an argument is `echo`.
The `echo` command prints a phrase to the console. Enter `echo "Hello World!"`
into the command line to see what happens:


```bash
echo "Hello World!"
```

```
## Hello World!
```

We'll be using the above syntax for the rest of the book, where on one line there
will be a command that I've entered into the command line, and then below that
command the console output of the command will appear (if there is any
console output). You can use `echo` to print any phase surrounded by double
quotes (`""`) to the console.

### Exercises

1. Print your name to the terminal.
2. Clear your terminal after completing #1.

## Navigating the Command Line

You've learned two command line commands (`clear` and `echo`) which is pretty
good! Before you learn more commands we need to discuss how files and folders 
are organized on your computer.

Computers are organized in a hierarchy of folders, where a folder can contain 
many folders and files. People who use Unix often refer to folders as 
directories and these terms are interchangeable. This directory hierarchy forms 
a tree, like the diagram below. You can use the command line to navigate these
trees on your computer.

![](img/musictree1.png)

As you can see in the image below, my Debussy directory is contained in my Music 
directory. This is the simplest case of how directories are structured.

![](img/musictree2.png)

The directory structure on most computers is much more complicated, but the 
structure on your computer probably looks something like this:

![](img/bigtree1.png)

There are a few special directories that you should be aware of on your 
computer. The directory at the top of this tree is called the root directory. 
The root directory contains all other directories, and is represented by a 
slash (`/`).

The home directory is another special directory that is represented by a tilde 
(`~`). Your home directory contains your personal files, like your photos, 
documents, and the contents of your desktop. When you first open up your shell
you usually start off in your home directory. Imagine tracing all of the 
directories from your root directory to the directory you're currently in. This
squence of directories is called a **path**. The diagram below illustrates the
path from a hypothetical root directory to the home directory.

![](img/redtree.png)

This path can be written as `/Users/sean`.

Open the command line if you closed it (either **Terminal** or **Git Bash**).
Your shell starts in your home directory. Whatever directory your shell is in
is called the **working directory**. Enter the `pwd` command into your shell
to **p**rint the **w**orking **d**irectory.


```bash
pwd
```

```
## /Users/sean
```

You can change your working directory using the `cd` command. If you use the
`cd` command without any arguments then your working directory is changed
to your home directory.

Enter `cd` into the command line and then enter `pwd`.


```bash
cd
pwd
```

```
## /Users/sean
```

You were in your working directory to start, and by entering `cd` into the
command line you did technically **c**hange **d**irectory, you just changed it
to your home directory (the directory you were in to begin with). To use `cd` to
change your working directory to a directory other than your home directory, you
need to provide `cd` with the path to another directory as an argument. You can
specify a path as either a path that is **relative** to your current directory,
or you can specify the **absolute** path to a directory starting from the root
of your computer. Let's say we simply want to change the working directory to
one of the folders that is inside our home directory. First we need to be able to
see which folders are in our working directory. You can list the files and
folders in a directory using the `ls` command. Let's use the `ls` command in our
home directory to list the files and folders contained within it.


```bash
ls
```

```
## Desktop
## Documents
## Photos
## Music
## todo.txt
```

It looks like I have four folders and one text file in my home directory. Now
let's switch into the Music directory:


```bash
cd Music
```

As you can see the path to the current working directory has changed:


```bash
pwd
```

```
## /Users/sean/Music
```

I specified a **relative** path when I entered `cd Music`. The path to the 
Music directory is just `Music/` relative to my previous working directory.
I can go back to `/Users/sean/` with the command `cd ..` which changes the
working directory to the folder above the current working directory:


```bash
cd ..
pwd
```

```
## /Users/sean
```

Notice that `..` is also a relative path, since it specifies the directory above
your current working directory. Similarly `.` is the path to your current
working directory. Therefore since my current working directory is `/Users/sean`
then `cd Music` is the same as `cd ./Music`.

I can `cd` to any folder as long as I know the **absolute** path to that folder.
For example I can `cd` to `/Users/sean/Music` by entering the following into 
the shell:


```bash
cd ~/Music
pwd
```

```
## /Users/sean/Music
```

It doesn't matter what directory I'm in since I'm using a absolue path, I can
jump straight to that directory (Remember that `~` is a shortcut for the path to
your home folder). Of course you shouldn't expect yourself to have every
absolute path on your computer memorized! You can use a terminal feature called
**tab completion** in order to speed up typing paths and other commands. Enter
the following into your shell, and then try pressing the `Tab` key (on some
machines you need to press it twice):


```bash
cd ~/
```

(press `Tab`)

```
## Desktop
## Documents
## Photos
## Music
## todo.txt
```

Pressing tab shows you a list of all files and folders inside of the `~/`
directory. Now I'm going to type `~/D` into my terminal and you can see what
happens when I press tab again:


```bash
cd ~/D
```

(press `Tab`)

```
## Desktop
## Documents
```

Since I added a "D" to the path, only folders with names that start with a "D"
are listed. If I type `cd ~/De` into the console and then press `Tab` then the
command will autocomplete to `cd ~/Desktop/`. If I press tab again, the console
will list all of the files and folders on my desktop.

Make sure to pause and try this yourself in your own terminal! You won't have
the same files or folders that I do, but you should try using `cd` and tab 
completion with directories and files that start with the same letters.

### Exercises

1. Set your working directory to the root directory.
2. Set your working directory to your home directory using three different 
commands.
3. Find a folder on your computer using your file and folder browser, and then
set your working dorectory to that folder using the terminal.
4. List all od the files and folders in the directory you navigated to in #3.

## Creation and Inspection

Now that you can fluidly use your terminal to bound between directories all
over your computer I'll show you some actions you can perform on folders and
files. One of the first actions you'll probably want to take when opening up a
fresh terminal is to create a new folder or file. You can **m**a**k**e a 
**dir**ectory with the `mkdir` command, followed by the path to the new
directory. First let's look at the contents of my home directory:


```bash
cd
ls
```

```
## Desktop
## Documents
## Photos
## Music
## todo.txt
```

I want to create a new directory to store some code files I'm going to write
later, so I'll use `mkdir` to create a new directory called `Code`:


```bash
mkdir Code
ls
```

```
## Desktop
## Documents
## Photos
## Music
## todo.txt
## Code
```

It worked! Notice that the argument `Code` to `mkdir` is a relative path, 
however I could have specified an absolute path. In general you should
expect Unix tools that take paths as arguments to accept both relative and
absolute paths.

There are a few different ways to create a new file on the command line. The
most simple way to create a blank file is to use the `touch` command, followed
by the path to the file you want to create. In this example I'm going to create
a new journal entry using touch:


```bash
touch journal-2017-01-24.txt
ls
```

```
## Desktop
## Documents
## Photos
## Music
## todo.txt
## Code
## journal-2017-01-24.txt
```

A new file has been created! I've been using `ls` to list the files and folders
in the current directory, but using `ls` alone doesn't differentiate between
which of the listed items are folders and which are files. Thankfully you can
use the `-l` option with `ls` in order to get a **l**ong listing of files in
a directory.


```bash
ls -l
```

```
## drwxr-xr-x  2 sean  staff  68 Jan 24 12:31 Code
## drwxr-xr-x  2 sean  staff  94 Jan 20 12:44 Desktop
## drwxr-xr-x  2 sean  staff  24 Jan 20 12:44 Documents
## drwxr-xr-x  2 sean  staff  68 Jan 20 12:36 Music
## drwxr-xr-x  2 sean  staff  68 Jan 20 12:35 Photos
## -rw-r--r--  1 sean  staff  90 Jan 24 11:33 journal-2017-01-24.txt
## -rw-r--r--  1 sean  staff  70 Jan 24 10:58 todo.txt
```

There is a row in the resulting table for each file or folder. If the entry in the
first column is a `d`, then the row in the table corresponds to a **d**irectory,
otherwise the information in the row corresponds to a file. As you can see in my
home directory there are five directories and two files. The string of
characters following the `d` in the case of a directory or following the first
`-` in the case of a file represent the permissions for that file or directory.
We'll cover permissions in a later section. The columns of this table also show
who created the file, the group that the creator of the file belongs to (we'll
cover grouos later when we cover permissions), the size of the file, the time
and date when the file was last modified, and then finally the name of the file.

Now that we've created a file there are a few different ways that we can inspect
and edit this file. First let's use the `wc` command to view the **w**ord
**c**ount and other information about the file:


```bash
wc todo.txt
```

```
##       3      14      70 todo.txt
```

The `wc` command displays the number of lines in a file followed by the number
of words and then the number of characters. Since this file looks pretty small
(only three lines) let's try printing it to the console using the `cat` command.


```bash
cat todo.txt
```

```
## - email Jeff
## - write letter to Aunt Marie
## - get groceries for Shabbat
```

The `cat` command is often used to print text files to the terminal, despite
the fact that it's really meant to con**cat**enate files. You can see this
concatination in action in the following example:


```bash
cat todo.txt todo.txt
```

```
## - email Jeff
## - write letter to Aunt Marie
## - get groceries for Shabbat
## - email Jeff
## - write letter to Aunt Marie
## - get groceries for Shabbat
```

Let's take a look at how we could view a larger file. Let's take a look inside
the Documents directory:


```bash
ls Documents
```

```
## a-tale-of-two-cities.txt
```

Let's examine this file to see if it's reasonable to read it with `cat`:


```bash
wc Documents/a-tale-of-two-cities.txt
```

```
##      17    1005    5799 Documents/a-tale-of-two-cities.txt
```

Wow, over 1000 words! If we use `cat` on this file it's liable to take up our
entire terminal. Instead of using `cat` for this large file we should use
`less`, which is a program designed for viewing multi-page files. Let's try
using `less`:


```bash
less Documents/a-tale-of-two-cities.txt
```

```
I. The Period

It was the best of times,
it was the worst of times,
it was the age of wisdom,
it was the age of foolishness,
it was the epoch of belief,
it was the epoch of incredulity,
it was the season of Light,
it was the season of Darkness,
it was the spring of hope,
it was the winter of despair,
we had everything before us, we had nothing before us, we were all going direct
Documents/a-tale-of-two-cities.txt
```

You can scroll up and down the file line-by-line using the up and down arrow 
keys, and if you want to scroll faster you can use the `spacebar` to go to the
next page and the `b` key to go to the previous page. In order to quit `less`
and go back to the prompt press the `q` key.

As you can see the `less` program is a kind of Unix tool with behavior that we
haven't seen before because it "takes over" your terminal. There are a few
programs like this that we'll discuss throughout this book.

We've now gone over a few tools for inspecting files, folders, and their
contents including `ls`, `wc`, `cat`, and `less`. Before the end of this section
we should discuss a few more techniques for creating and also editing files. One
easy way to create a file is using **output redirection**. Output redirection
stores text that would be normally printed to the command line in a text file.
You can use output redirection by typing the greater-than sign (`>`) at the end
of a command followed by the name of the new file that will contain the output
from the proceeding command. Let's try an example using `echo`:


```bash
echo "I'm in the terminal."
echo "I'm in the file." > echo-out.txt
```

```
## I'm in the terminal.
```

Only the first command printed output to the terminal. Let's see if the second
command worked:


```bash
ls
```

```
## Desktop
## Documents
## Photos
## Music
## todo.txt
## Code
## journal-2017-01-24.txt
## echo-out.txt
```


```bash
cat echo-out.txt
```

```
## I'm in the file.
```

Looks like it worked! You can also **append** text to the end of a file using
two greater-than signs (`>>`). Let's try this feature out:


```bash
echo "I have been appended." >> echo-out.txt
cat echo-out.txt
```

```
## I'm in the file.
## I have been appended.
```

Now for a **word of warning**. Imagine that I want to append another line to 
the end of `echo-out.txt`, so typed `echo "A third line." > echo-out.txt` into
the terminal when really I meant to type `echo "A third line." >> echo-out.txt`
(notice I used `>` when I meant to use `>>`). Let's see what happens:


```bash
echo "A third line." > echo-out.txt
cat echo-out.txt
```

```
## A third line.
```

Unfortunately I have unintentionally overwritten what was already contained in
`echo-out.txt`. There's no undo button in Unix so I'll have to live with this
mistake. This is the first of several lessons demonstrating the damage that you
should avoid inflicting with Unix.

- be careful before you execute a command
- we'll learn later how to protect yourself and how to go back/undo (with git)

### Exercises

1. Create and remove the same directory using three different path arguments.
2. Create, move, and remove a file.

## Migration and Destruction
