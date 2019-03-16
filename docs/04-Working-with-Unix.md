# Working with Unix

> It is not the knowing that is difficult, but the doing. - Chinese proverb

## Self-Help

Each of the commands that we've discussed so far are thoroughly documented, and
you can view their documentation using the `man` command, where the first
argument to `man` is the command you're curious about. Let's take a look at the
documentation for `ls`:


```bash
man ls
```

```
LS(1)                     BSD General Commands Manual                    LS(1)

NAME
     ls -- list directory contents

SYNOPSIS
     ls [-ABCFGHLOPRSTUW@abcdefghiklmnopqrstuwx1] [file ...]

DESCRIPTION
     For each operand that names a file of a type other than directory, ls
     displays its name as well as any requested, associated information.  For
:
```

The controls for navigating `man` pages are the same as they are for `less`.
I often use `man` pages for quickly searching for an option that I've forgotten.
Let's say that I forgot how to get `ls` to print a long list. After typing
`man ls` to open the page, type `/` in order to start a search. Then type the
word or phrase that you're searching for, in this case type in `long list` and
then press `Enter`. The page jumps to this entry:

```
     -l      (The lowercase letter ``ell''.)  List in long format.  (See below.)
             If the output is to a terminal, a total sum for all the file sizes is
             output on a line before the long listing.
```

Press the `n` key in order to search for the next occurrence of the word, and if
you want to go to the previous occurrence type `Shift` + `n`. This method of
searching also works with `less`. When you're finished looking at a `man` page
type `q` to get back to the prompt.

The `man` command works wonderfully when you know which command you want to look
up, but what if you've forgotten the name of the command you're looking for? You
can use `apropos` to search all of the available commands and their
descriptions. For example let's pretend that I forgot the name of my favorite
command line text editor. You could type `apropos editor` into the command line
which will print a list of results:


```bash
apropos editor
```

```
## ed(1), red(1)            - text editor
## nano(1)                  - Nano's ANOther editor, an enhanced free Pico clone
## sed(1)                   - stream editor
## vim(1)                   - Vi IMproved, a programmers text editor
```

The second result is `nano` which was just on the tip of my tongue! Both `man`
and `apropos` are useful when a search is only a few keystrokes away, but if 
you're looking for detailed examples and explanations you're better off using 
a search engine if you have access to a web browser.

### Summary

- Use `man` to look up the documentation for a command.
- If you can't think of the name of a command use `apropos` to search for a word
associated with that command.
- If you have access to a web browser, using a search engine might be better
than `man` or `apropos`.

### Exercises

1. Use `man` to look up the flag for human-readable output from `ls`.
2. Get help with `man` by typing `man man` into the console.
3. Wouldn't it be nice if there was a calendar command? Use `apropos` to look
for such a command, then use `man` to read about how that command works.

## Get Wild

Let's go into my `Photos` folder in my home directory and take a look around:


```bash
pwd
```

```
## /Users/sean
```


```bash
ls
```

```
## Code
## Documents
## Photos
## Desktop
## Music
## todo-2017-01-24.txt
```


```bash
cd Photos
ls
```

```
## 2016-06-20-datasci01.png
## 2016-06-20-datasci02.png
## 2016-06-20-datasci03.png
## 2016-06-21-lab01.jpg
## 2016-06-21-lab02.jpg
## 2017-01-02-hiking01.jpg
## 2017-01-02-hiking02.jpg
## 2017-02-10-hiking01.jpg
## 2017-02-10-hiking02.jpg
```

I've just been dumping pictures and figures into this folder without organizing
them at all! Thankfully (in the words of Dr. Jenny Bryan) [I have an unwavering 
commitment to the ISO 8601 date
standard](https://twitter.com/JennyBryan/status/816143967695687684) so at least
I know when these photos were taken. Instead of using `mv` to move around each
individual photo I can select groups of photos using the `*` wildcard. A 
**wildcard** is a character that represents other characters, much like how
joker in a deck of cards can represent other cards in the deck. Wildcards are
a subset of metacharacters, a topic which we will discuss in detail later on in
this chapter. The `*` ("star") wildcard represents *zero or more of any
character*, and it can be used to match names of files and folders in the 
command line. For example if I wanted to list all of the files in my Photos 
directory which have a name that starts with "2017" I could do the following:


```bash
ls 2017*
```

```
## 2017-01-02-hiking01.jpg
## 2017-01-02-hiking02.jpg
## 2017-02-10-hiking01.jpg
## 2017-02-10-hiking02.jpg
```

Only the files starting with "2017" are listed! The command `ls 2017*` literally
means: list the files that start with "2017" followed by zero or more of any
character. As you can imagine using wildcards is a powerful tool for working 
with groups of files that are similarly named.

Let's walk through a few other examples of using the star wildcard. We could
only list the photos starting with "2016":


```bash
ls 2016*
```

```
## 2016-06-20-datasci01.png
## 2016-06-20-datasci02.png
## 2016-06-20-datasci03.png
## 2016-06-21-lab01.jpg
## 2016-06-21-lab02.jpg
```

We could list only the files with names ending in `.jpg`:


```bash
ls *.jpg
```

```
## 2016-06-21-lab01.jpg
## 2016-06-21-lab02.jpg
## 2017-01-02-hiking01.jpg
## 2017-01-02-hiking02.jpg
## 2017-02-10-hiking01.jpg
## 2017-02-10-hiking02.jpg
```

In the case above the file name can start with a sequence of zero or more of 
any character, but the file name must end in `.jpg`.
Or we could also list only the first photos from each set of photos:


```bash
ls *01.*
```

```
## 2016-06-20-datasci01.png
## 2016-06-21-lab01.jpg
## 2017-01-02-hiking01.jpg
## 2017-02-10-hiking01.jpg
```

All of the files above have names that are composed of a sequence of characters,
followed by the adjacent characters `01.`, followed by another sequence of
characters.
Notice that if I had entered `ls *01*` into the console every file would have
been listed since `01` is a part of all of the file names in my Photos
directory.

Let's organize these photos by year. First let's create one directory for
each year of photos:


```bash
mkdir 2016
mkdir 2017
```

Now we can move the photos using wildcards:


```bash
mv 2017-* 2017/
ls
```

```
## 2016
## 2016-06-20-datasci01.png
## 2016-06-20-datasci02.png
## 2016-06-20-datasci03.png
## 2016-06-21-lab01.jpg
## 2016-06-21-lab02.jpg
## 2017
```

Notice that I've moved all files that start with "2017-" into the 2017 folder!
Now let's do the same thing for files with names starting with "2016-":


```bash
mv 2016-* 2016/
ls
```

```
## 2016
## 2017
```

Finally my photos are somewhat organized! Let's list the files in each directory
just to make sure all was moved as planned:


```bash
ls 2016/
```

```
## 2016-06-20-datasci01.png
## 2016-06-20-datasci02.png
## 2016-06-20-datasci03.png
## 2016-06-21-lab01.jpg
## 2016-06-21-lab02.jpg
```


```bash
ls 2017/
```

```
## 2017-01-02-hiking01.jpg
## 2017-01-02-hiking02.jpg
## 2017-02-10-hiking01.jpg
## 2017-02-10-hiking02.jpg
```

Looks good! There are a few more wildcards beyond the star wildcard which we'll
discuss in the next section where searching file names gets a little more
advanced.

### Summary

- Wildcards can represent many kinds and numbers of characters.
- The star wildcard (`*`) represents zero or more of any character.
- You can use wildcards on the command line in order to work with multiple files
and folders.

### Exercises

1. Before I organized the photos by year, what command would have listed all of
the photos of type `.png`?
2. Before I organized the photos by year, what command would have deleted all of
my hiking photos?
3. What series of commands would you use in order to put my figures for a data
science course and the pictures I took in the lab into their own folders?

## Search

### Regular Expressions

The ability to search through files and folders can greatly improve your
productivity using Unix. First we'll cover searching through text files.
I recently downloaded a list of the names of the states in the US which you
can find [here](http://seankross.com/notes/states.txt). Let's take a look at 
this file:


```bash
cd ~/Documents
ls
```

```
## canada.txt
## states.txt
```


```bash
wc states.txt
```

```
## 50      60     472 states.txt
```

It makes sense that there are 50 lines, but it's interesting that there are 60
total words. Let's a take a peak at the beginning of the file:


```bash
head states.txt
```

```
## Alabama
## Alaska
## Arizona
## Arkansas
## California
## Colorado
## Connecticut
## Delaware
## Florida
## Georgia
```

This file looks basically how you would expect it to look! You may recall from
Chapter 3 that the kind of shell that we're using is the bash shell. Bash
treats different kinds of data differently, and we'll dive deeper into data
types in Chapter 5. For now all you need to know is that text data are
called **strings**. A string could be a word, a sentence, a book, or a file or 
folder name. One of the most effective ways to search through strings is to use
**regular expressions**. Regular expressions are strings that define patterns
in other strings. You can use regular expressions to search for a sub-string
contained within a larger string, or to replace one part of a string with
another string.

One of the most popular tools for searching through text files is `grep`. The
simplest use of `grep` requires two arguments: a regular expression and a text
file to search. Let's see a simple example of `grep` in action and then I'll
explain how it works:


```bash
grep "x" states.txt
```

```
## New Mexico
## Texas
```

In the command above, the first argument to `grep` is the regular expression
`"x"`. The `"x"` regular expression represents one instance of the letter "x".
Every line of the `states.txt` file that contains at least one instance of the
letter "x" is printed to the console. As you can see New Mexico and Texas are
the only two state names that contain the letter "x". Let's try searching for 
the letter "q" in all of the state names using `grep`:


```bash
grep "q" states.txt
```

Nothing is printed to the console because the letter "q" isn't in any of the
state names. We can search for more than individual characters though. For
example the following command will search for the state names that contain the
word "New":


```bash
grep "New" states.txt
```

```
## New Hampshire
## New Jersey
## New Mexico
## New York
```

In the previous case the regular expression we used was simply `"New"`, which
represents an occurrence of the string "New". Regular expressions are not
limited to just being individual characters or words, they can also represent
parts of words. For example I could search all of the state names that contain
the string "nia" with the following command:


```bash
grep "nia" states.txt
```

```
## California
## Pennsylvania
## Virginia
## West Virginia
```

All of the state names above happen to end with the string "nia".

### Metacharacters

Regular expressions aren't just limited to searching with characters and
strings, the real power of regular expressions come from using
**metacharacters**. Remember that metacharacters are characters that can be used
to represent other characters. To take full advantage of all of the metacharacters
we should use `grep`'s cousin `egrep`, which just extends `grep`'s capabilities.
If you're using Ubuntu you should use `grep -P` instead of `egrep` for results
that are consistent with this chapter.
The first metacharacter we should discuss is the `"."` (period) metacharacter, 
which represents *any* character. If for example I wanted to search `states.txt` 
for the character "i", followed by any character, followed by the character "g" 
I could do so with the following command:


```bash
egrep "i.g" states.txt
```

```
## Virginia
## Washington
## West Virginia
## Wyoming
```

The regular expression "i.g" matches the sub-string "irg" in V*irg*inia, and
West V*irg*inia, and it matches the sub-string "ing" in Wash*ing*ton and
Wyom*ing*. The period metacharacter is a stand-in for the "r" in "irg" and the
"n" in "ing" in the example above. The period metacharacter is extremely liberal,
for example the command `egrep "." states.txt` would return every line of
states.txt since the regular expression `"."` would match one occurrence of any
character on every line (there's at least one character on every line). 

Besides characters that can represent other
characters, there are also metacharacters called **quantifiers** which allow you
to specify the number of times a particular regular expression should appear in 
a string. One of the most basic quantifiers is `"+"` (plus) which represents one 
or more occurrences of the proceeding expression. For example the regular 
expression "s+as" means: one or more "s" followed by "as". Let's see if any of 
the state names match this expression:


```bash
egrep "s+as" states.txt
```

```
## Arkansas
## Kansas
```

Both Arkan*sas* and Kan*sas* match the regular expression `"s+as"`. Besides the
plus metacharacter there's also the `"*"` (star) metacharacter which represents
zero or more occurrences of the preceding expression. Let's see what happens if
we change `"s+as"` to `"s*as"`:


```bash
egrep "s*as" states.txt
```

```
## Alaska
## Arkansas
## Kansas
## Massachusetts
## Nebraska
## Texas
## Washington
```

As you can see the star metacharacter is much more liberal with respect to
matching since many more state names are matched by `"s*as"`. There are more
specific quantifies you can use beyond "zero or more" or "one or more"
occurrences of an expression. You can use curly brackets (`{ }`) to specify an
exact number of occurrences of an expression. For example the regular expression
`"s{2}"` specifies exactly two occurrences of the character "s". Let's try using
this regular expression:


```bash
egrep "s{2}" states.txt
```

```
## Massachusetts
## Mississippi
## Missouri
## Tennessee
```

Take note that the regular expression `"s{2}"` is equivalent to the regular
expression `"ss"`. We could also search for state names that have between two
and three adjacent occurrences of the letter "s" with the regular expression
`"s{2,3}"`:


```bash
egrep "s{2,3}" states.txt
```

```
## Massachusetts
## Mississippi
## Missouri
## Tennessee
```

Of course the results are the same because there aren't any states that have "s"
repeated three times.

You can use a **capturing group** in order to search for multiple occurrences of
a string. You can create capturing groups within regular expressions by using
parentheses (`"( )"`). For example if I wanted to search states.txt for the
string "iss" occurring twice in a state name I could use a capturing group and
a quantifier like so:


```bash
egrep "(iss){2}" states.txt
```

```
## Mississippi
```

We could combine more quantifiers and capturing groups to dream up even more
complicated regular expressions. For example, the following regular expression
describes three occurrences of an "i" followed by two of any character:


```bash
egrep "(i.{2}){3}" states.txt
```

```
## Mississippi
```

The complex regular expression above still only matches "Mississippi".

### Character Sets

For the next couple of examples we're going to need some text data beyond the
names of the states. Let's just create a short text file from the console:


```bash
touch small.txt
echo "abcdefghijklmnopqrstuvwxyz" >> small.txt
echo "ABCDEFGHIJKLMNOPQRSTUVWXYZ" >> small.txt
echo "0123456789" >> small.txt
echo "aa bb cc" >> small.txt
echo "rhythms" >> small.txt
echo "xyz" >> small.txt
echo "abc" >> small.txt
echo "tragedy + time = humor" >> small.txt
echo "http://www.jhsph.edu/" >> small.txt
echo "#%&-=***=-&%#" >> small.txt
```

In addition to quantifiers there are also regular expressions for describing
sets of characters. The `\w` metacharacter corresponds to all "word" characters,
the `\d` metacharacter corresponds to all "number" characters, and the `\s`
metacharacter corresponds to all "space" characters. Let's take a look at using
each of these metacharacters on small.txt:


```bash
egrep "\w" small.txt
```

```
## abcdefghijklmnopqrstuvwxyz
## ABCDEFGHIJKLMNOPQRSTUVWXYZ
## 0123456789
## aa bb cc
## rhythms
## xyz
## abc
## tragedy + time = humor
## http://www.jhsph.edu/
```


```bash
egrep "\d" small.txt
```

```
## 0123456789
```


```bash
egrep "\s" small.txt
```

```
## aa bb cc
## tragedy + time = humor
```

As you can see in the example above, the `\w` metacharacter matches all letters,
numbers, and even the underscore character (`_`). We can see the complement of
this grep by adding the `-v` flag to the command:


```bash
egrep -v "\w" small.txt
```

```
## #%&-=***=-&%#
```

The `-v` flag (which stands for in**v**ert match) makes `grep` return all of the
lines not matched by the regular expression. Note that the character sets for 
regular expressions also have their inverse sets: `\W` for non-words, `\D` for
non-digits, and `\S` for non-spaces. Let's take a look at using `\W`:


```bash
egrep "\W" small.txt
```

```
## aa bb cc
## tragedy + time = humor
## http://www.jhsph.edu/
## #%&-=***=-&%#
```

The returned strings all contain non-word characters. Note the difference between
the results of using the invert flag `-v` versus using an inverse set regular
expression.

In addition to general character sets we can also create specific character
sets using square brackets (`[ ]`) and then including the characters we wish to
match in the square brackets. For example the regular expression for the set
of vowels is `[aeiou]`. You can also create a regular expression for the
complement of a set by including a caret (`^`) in the beginning of a set. For
example the regular expression `[^aeiou]` matches all characters that are not
vowels. Let's test both on small.txt:


```bash
egrep "[aeiou]" small.txt
```

```
## abcdefghijklmnopqrstuvwxyz
## aa bb cc
## abc
## tragedy + time = humor
## http://www.jhsph.edu/
```

Notice that the word "rhythms" does not appear in the result (it's the longest
word without any vowels that I could think of).


```bash
egrep "[^aeiou]" small.txt
```

```
## abcdefghijklmnopqrstuvwxyz
## ABCDEFGHIJKLMNOPQRSTUVWXYZ
## 0123456789
## aa bb cc
## rhythms
## xyz
## abc
## tragedy + time = humor
## http://www.jhsph.edu/
## #%&-=***=-&%#
```

Every line in the file is printed, because every line contains at least one
non-vowel! If you want to specify a range of characters you can use a hyphen
(`-`) inside of the square brackets. For example the regular expression `[e-q]`
matches all of the lowercase letters between "e" and "q" in the alphabet
inclusively. Case matters when you're specifying character sets, so if you
wanted to only match uppercase characters you'd need to use `[E-Q]`. To ignore
the case of your match you could combine the character sets with the `[e-qE-Q]`
regex (short for regular expression), or you could use the `-i` flag with `grep`
to **i**gnore the case. Note that the `-i` flag will work for any provided regular
expression, not just character sets. Let's take a look at some examples using
the regular expressions that we just described:


```bash
egrep "[e-q]" small.txt
```

```
## abcdefghijklmnopqrstuvwxyz
## rhythms
## tragedy + time = humor
## http://www.jhsph.edu/
```


```bash
egrep "[E-Q]" small.txt
```

```
## ABCDEFGHIJKLMNOPQRSTUVWXYZ
```


```bash
egrep "[e-qE-Q]" small.txt
```

```
## abcdefghijklmnopqrstuvwxyz
## ABCDEFGHIJKLMNOPQRSTUVWXYZ
## rhythms
## tragedy + time = humor
## http://www.jhsph.edu/
```

### Escaping, Anchors, Odds, and Ends

One issue you may have thought about during our little exploration of regular
expressions is how to search for certain punctuation marks in text considering
that those same symbols are used as metacharacters! For example, how would you
find a plus sign (`+`) in a line of text since the plus sign is **also** a
metacharacter? The answer is simply using a backslash (`\`) before the plus sign
in a regex, in order to "escape" the metacharacter functionality. Here are a few
examples:


```bash
egrep "\+" small.txt
```

```
## tragedy + time = humor
```


```bash
egrep "\." small.txt
```

```
## http://www.jhsph.edu/
```

There are three more metacharacters that we should discuss, and two of them come
as a pair: the caret (`^`), which represents the start of a line, and the dollar
sign (`$`) which represents the end of line. These "anchor characters" only
match the beginning and ends of lines when coupled with other regular
expressions. For example, going back to looking at states.txt, I could search
for all of the state names that begin with "M" with the following command:


```bash
egrep "^M" states.txt
```

```
## Maine
## Maryland
## Massachusetts
## Michigan
## Minnesota
## Mississippi
## Missouri
## Montana
```

Or we could search for all of the states that end in "s":


```bash
egrep "s$" states.txt
```

```
## Arkansas
## Illinois
## Kansas
## Massachusetts
## Texas
```

There's a mnemonic that I love for remembering which metacharacter to use for
each anchor: "First you get the **power**, then you get the **money**." The
caret character is used for exponentiation in many programming languages, so
"power" (`^`) is used for the beginning of a line and "money" (`$`) is used for
the end of a line.

Finally, let's talk about the "or" metacharacter (`|`), which is also called the
"pipe" character. This metacharacter allows you to match either the regex on 
the right or on the left side of the pipe. Let's take a look at a small example:


```bash
egrep "North|South" states.txt
```

```
## North Carolina
## North Dakota
## South Carolina
## South Dakota
```

In the example above we're searching for lines of text that contain the words
"North" or "South". You can also use multiple pipe characters to, for example,
search for lines that contain the words for all of the cardinal directions:


```bash
egrep "North|South|East|West" states.txt
```

```
## North Carolina
## North Dakota
## South Carolina
## South Dakota
## West Virginia
```

Just two more notes on `grep`: you can display the line number that a match
occurs on using the `-n` flag:


```bash
egrep -n "t$" states.txt
```

```
## 7:Connecticut
## 45:Vermont
```

And you can also `grep` multiple files at once by providing multiple file
arguments:


```bash
egrep "New" states.txt canada.txt
```

```
## states.txt:New Hampshire
## states.txt:New Jersey
## states.txt:New Mexico
## states.txt:New York
## canada.txt:Newfoundland and Labrador
## canada.txt:New Brunswick
```

You now have the power to do some pretty complicated string searching using
regular expressions! Imagine you wanted to search for all of the state names 
that both begin and end with a vowel. Now you can:


```bash
egrep "^[AEIOU]{1}.+[aeiou]{1}$" states.txt
```

```
## Alabama
## Alaska
## Arizona
## Idaho
## Indiana
## Iowa
## Ohio
## Oklahoma
```

I know there a many metacharacters to keep track of here so below I've included
a table with several of the metacharacters we've discussed in this chapter:

| Metacharacter |               Meaning                |
|--------------:|:-------------------------------------|
|       .       |            Any Character             |
|      \\w      |                A Word                |
|      \\W      |              Not a Word              |
|      \\d      |               A Digit                |
|      \\D      |             Not a Digit              |
|      \\s      |              Whitespace              |
|      \\S      |            Not Whitespace            |
|     [def]     |         A Set of Characters          |
|    [^def]     |           Negation of Set            |
|     [e-q]     |        A Range of Characters         |
|       ^       |         Beginning of String          |
|       $       |            End of String             |
|      \\n      |               Newline                |
|       +       |       One or More of Previous        |
|       *       |       Zero or More of Previous       |
|       ?       |       Zero or One of Previous        |
|    &#124;     | Either the Previous or the Following |
|      {6}      |        Exactly 6 of Previous         |
|    {4, 6}     |     Between 4 and 6 of Previous      |
|     {4, }     |        4 or more of Previous         |

If you want to experiment with writing regular expressions before you use them
I highly recommend playing around with http://regexr.com/.

### `find`

If you want to find the location of a file or the location of a group of files
you can use the `find` command. This command has a specific structure where
the first argument is the directory where you want to begin the search, and all
directories contained within that directory will also be searched. The first
argument is then followed by a flag that describes the method you want to use to
search. In this case we'll only be searching for a file by its name, so we'll
use the `-name` flag. The `-name` flag itself then takes an argument, the name
of the file that you're looking for. Let's go back to the home directory and
look for some files from there:


```bash
cd
pwd
```

```
## /Users/sean
```

Let's start by looking for a file called states.txt:


```bash
find . -name "states.txt"
```

```
## ./Documents/states.txt
```

Right where we expected it to be! Now let's try searching for all `.jpg` files:


```bash
find . -name "*.jpg"
```

```
## ./Photos/2016-06-21-lab01.jpg
## ./Photos/2016-06-21-lab02.jpg
## ./Photos/2017/2017-01-02-hiking01.jpg
## ./Photos/2017/2017-01-02-hiking02.jpg
## ./Photos/2017/2017-02-10-hiking01.jpg
## ./Photos/2017/2017-02-10-hiking02.jpg
```

Good file hunting out there!

### Summary

- `grep` and `egrep` can be used along with regular expressions to search for
patterns of text in a file.
- Metacharacters are used in regular expressions to describe patterns of
characters.
- `find` can be used to search for the names of files in a directory.

### Exercises

1. Search `states.txt` and `canada.txt` for lines that contain the word "New".
2. Make five text files containing the names of states that don't contain one of
each of the five vowels.
3. Download the GitHub repository for this book and find out how many `.html` 
files it contains.

## Configure

### History

Near the start of this book we discussed how you can browse the commands
that you recently entered into the prompt using the `Up` and `Down` arrow keys.
Bash keeps track of all of your recent commands, and you can browse your command
history two different ways. The commands that we've used since opening our
terminal can be accessed via the `history` command. Let's try it out:


```bash
history
```

```
## ...
## 48 egrep "^M" states.txt
## 49 egrep "s$" states.txt
## 50 egrep "North|South" states.txt
## 51 egrep "North|South|East|West" states.txt
## 52 egrep -n "t$" states.txt
## 53 egrep "New" states.txt canada.txt
## 54 egrep "^[AEIOU]{1}.+[aeiou]{1}$" states.txt
## 55 cd
## 56 pwd
## 57 find . -name "states.txt"
## 58 find . -name "*.jpg"
## 59 history
```

We've had our terminal open for a while so there are tons of commands in our
history! Whenever we close a terminal our recent commands are written to the
`~/.bash_history` file. Let's a take a look at the beginning of this file:


```bash
head -n 5 ~/.bash_history
```

```
## echo "Hello World!"
## pwd
## cd
## pwd
## ls
```

Looks like the very first commands we entered into the terminal! Searching your
`~/.bash_history` file can be particularly useful if you're trying to recall
a command you've used in the past. The `~/.bash_history` file is just a regular
text file, so you can search it with `grep`. Here's a simple example:


```bash
grep "canada" ~/.bash_history
```

```
## egrep "New" states.txt canada.txt
```

### Customizing Bash

Besides `~/.bash_history`, another text file in our home directory that we
should be aware of is `~/.bash_profile`. The `~/.bash_profile` is a list of
Unix commands that are run every time we open our terminal, usually with a
different command on every line. One of the most common commands used in a
`~/.bash_profile` is the `alias` command, which creates a shorter name for a
command. Let's take a look at a `~/.bash_profile`:

```
alias docs='cd ~/Documents'
alias edbp='nano ~/.bash_profile'
```

The first `alias` creates a new command `docs`. Now entering `docs` into the
command line is the equivalent of entering `cd ~/Documents` into the comamnd
line. Let's edit our `~/.bash_profile` with `nano`. If there's anything
in your `~/.bash_profile` already then start adding lines at the end of the
file. Add the line `alias docs='cd ~/Documents'`, then save the file and quit
`nano`. In order to make the changes to our `~/.bash_profile` take effect we
need to run `source ~/.bash_profile` in the console:


```bash
source ~/.bash_profile
```

Now let's try using `docs`:


```bash
docs
pwd
```

```
## /Users/sean/Documents
```

It works! Setting different `alias`es allows you to save time if there are long
commands that you use often. In the example `~/.bash_profile` above, the second
line, `alias edbp='nano ~/.bash_profile'` creates the command `edbp` (**ed**it
**b**ash **p**rofile) so that you can quickly add `alias`es. Try adding it to
your `~/.bash_profile` and take your new command for a spin!

There are a few other details about the `~/.bash_profile` that are important
when you're writing software which we'll discuss in the Bash Programming
chapter.

### Summary

- `history` displays what commands we've entered into the console
since opening our current terminal.
- The `~/.bash_history` file lists commands we've used in the past.
- `alias` creates a command that can be used as a substitute for a longer
command that we use often.
- The `~/.bash_profile` is a text file that is run every time we start a shell,
and it's the best place to assign `alias`es.

## Differentiate

It's important to be able to examine differences between files. First let's
make two small simple text files in the Documents directory.


```bash
cd ~/Documents
head -n 4 states.txt > four.txt
head -n 6 states.txt > six.txt
```

If we want to look at which lines in these files are different we can use the
`diff` command:


```bash
diff four.txt six.txt
```

```
## 4a5,6
## > California
## > Colorado
```

Only the differing lines are printed to the console. We could also compare
differing lines in a side-by-side comparison using `sdiff`:


```bash
sdiff four.txt six.txt
```

```
## Alabama              Alabama
## Alaska               Alaska
## Arizona              Arizona
## Arkansas             Arkansas
##                    > California
##                    > Colorado
```

In a common situation you might be sent a file, or you might download a file from the 
internet that comes with code known as a **checksum** or a **hash**. Hashing
programs generate a unique code based on the contents of a file. People
distribute hashes with files so that we can be sure that the file we think
we've downloaded is the genuine file. One way we can prevent malicious
individuals from sending us harmful files is to check to make sure the computed
hash matches the provided hash. 
There are a few commonly used file hashes but we'll talk about two called MD5
and SHA-1.

Since hashes are generated based on file contents, then two identical files
should have the same hash. Let's test this my making a copy of `states.txt`.


```bash
cp states.txt states_copy.txt
```

To compute the MD5 hash of a file we can use the `md5` command:


```bash
md5 states.txt
```

```
## MD5 (states.txt) = 8d7dd71ff51614e69339b03bd1cb86ac
```


```bash
md5 states_copy.txt
```

```
## MD5 (states_copy.txt) = 8d7dd71ff51614e69339b03bd1cb86ac
```

As we expected they're the same! We can compute the SHA-1 hash using the
`shasum` command:


```bash
shasum states.txt
```

```
## 588e9de7ffa97268b2448927df41760abd3369a9  states.txt
```


```bash
shasum states_copy.txt
```

```
## 588e9de7ffa97268b2448927df41760abd3369a9  states_copy.txt
```

Once again, both copies produce the same hash. Let's make a change to one of the
files, just to illustrate the fact that the hash changes if file contents are
different:


```bash
head -n 5 states_copy.txt > states_copy.txt
shasum states_copy.txt
```

```
## b1c1c805f123f31795c77f78dd15c9f7ac5732d4  states_copy.txt
```

### Summary

- The `md5` and `shasum` commands use different algorithms to create codes
(called hashes or checksums) that are unique to the contents of a file.
- These hashes can be used to ensure that a file is genuine.

## Pipes

One of the most powerful features of the command line is skilled use of the
**pipe** (`|`) which you can usually find above the backslash (`\`) on your
keyboard. The pipe allows us to take the output of a command, which would
normally be printed to the console, and use it as the input to another command.
It's like fitting an actual pipe between the end of one program and connecting
it to the top of another program!
Let's take a look at a basic example. We know the `cat` command takes the
contents of a text file and prints it to the console:


```bash
cd ~/Documents
cat canada.txt
```

```
## Nunavut
## Quebec
## Northwest Territories
## Ontario
## British Columbia
## Alberta
## Saskatchewan
## Manitoba
## Yukon
## Newfoundland and Labrador
## New Brunswick
## Nova Scotia
## Prince Edward Island
```

This output from `cat canada.txt` will go into our pipe, and we'll attach the
dispensing end of the pipe to `head`, which we use to look at the first few
lines of a file:


```bash
cat canada.txt | head -n 5
```

```
Nunavut
Quebec
Northwest Territories
Ontario
British Columbia
```

Notice that this is the same result we would get from `head -n 5 canada.txt`,
we just used `cat` to illustrate how the pipe works. The general syntax of the
pipe is 
`[program that produces output] | [program that uses pipe output as input instead of a file]`.

A more common and useful example where we could use the pipe is answering the
question: "How many US states end in a vowel?" We could use `grep` and regular
expressions to list all of the state names that end with a vowel, then we could
use `wc` to count all of the matching state names:


```bash
grep "[aeiou]$" states.txt | wc -l
```

```
## 32
```

The pipe can also be used multiple times in one command in order to take
the output from one piped command and use it as the input to yet another program!
For example we could use three pipes with `ls`, `grep`, and `less` so that we
could scroll through the files in our current directory that were created in February:


```bash
ls -al | grep "Feb" | less
```

```
-rw-r--r--   1 sean  staff   472 Feb 22 13:47 states.txt
```

Remember you can use the `Q` key to quit `less` and return to the prompt.

### Summary

- The pipe (`|`) takes the output of the program on its left side and directs
the output to be the input for the program on its right side.

### Exercises

1. Use pipes to figure out how many US states contain the word "New."
2. Examine your `~/.bash_history` to try to figure out how many unique commands
you've ever used. (You may need to look up how to use the `uniq` and `sort`
commands).

## Make

Once upon a time there were no web browsers, file browsers, start menus, or
search bars. When somebody booted up a computer all they got a was a shell
prompt, and all of the work they did started from that prompt. Back then people
still loved to share software, but there was always the problem of how software
should be installed. The `make` program is the best attempt at solving this
problem, and `make`'s elegance has carried it so far that it is still in wide
use today. The guiding design goal of `make` is that in order to
install some new piece of software one would:

1. Download all of the files required for installation into a directory.
2. `cd` into that directory.
3. Run `make`.

This is accomplished by specifying a file called `makefile`, which describes the
relationships between different files and programs. In addition to installing
programs, `make` is also useful for creating documents automatically. Let's build
up a `makefile` that creates a `readme.txt` file which is automatically
populated with some information about our current directory.

Let's start by creating a very basic `makefile` with `nano`:


```bash
cd ~/Documents/Journal
nano makefile
```

```
draft_journal_entry.txt:
  touch draft_journal_entry.txt
```

The simple `makefile` above illustrates a **rule** which has the
following general format:

```
[target]: [dependencies...]
  [commands...]
```

In the simple example we created `draft_journal_entry.txt` is the **target**, a
file which is created as the result of the **command(s)**. It's very important
to note that any commands under a target **must be indented with a `Tab`**. If
we don't use `Tab`s to indent the commands then `make` will fail.
Let's save and close the `makefile`, then we can run the following in the
console:


```bash
ls
```

```
## makefile
```

Let's use the `make` command with the target we want to be "made" as the only
argument:


```bash
make draft_journal_entry.txt
```

```
## touch draft_journal_entry.txt
```


```bash
ls
```

```
## draft_journal_entry.txt
## makefile
```

The commands that are indented under our definition of the rule for the
`draft_journal_entry.txt` target were executed, so now `draft_journal_entry.txt`
exists! Let's try running the same `make` command again:


```bash
make draft_journal_entry.txt
```

```
## make: 'draft_journal_entry.txt' is up to date.
```

Since the target file already exists no action is taken, and instead we're
informed that the rule for `draft_journal_entry.txt` is "up to date" (there's
nothing to be done).

If we look at the general rule format we previously sketched out, we can see
that we didn't specify any dependencies for this rule. A **dependency** is a
file that the target depends on in order to be built. If a dependency has
been updated since the last time `make` was run for a target then the target is
not "up to date." This means that the commands for that target will be run the
next time `make` is run for that target. This way, the changes to the dependency
are incorporated into the target. The commands are only run when the dependencies
change, or when the target doesn't exist at all, in order to avoid running
commands unnecessarily.

Let's update our `makefile` to include a `readme.txt` that is built
automatically. First, let's add a table of contents for our journal:


```bash
echo "1. 2017-06-15-In-Boston" > toc.txt
```

Now let's update our `makefile` with `nano` to automatically generate a 
`readme.txt`:


```bash
nano makefile
```

```
draft_journal_entry.txt:
  touch draft_journal_entry.txt
  
readme.txt: toc.txt
  echo "This journal contains the following number of entries:" > readme.txt
  wc -l toc.txt | egrep -o "[0-9]+" >> readme.txt
```

Take note that the `-o` flag provided to `egrep` above extracts the regular
expression match from the matching line, so that only the number of lines is
appended to `readme.txt`. Now let's run `make` with `readme.txt` as the target:


```bash
make readme.txt
```

```
## echo "This journal contains the following number of entries:" > readme.txt
## wc -l toc.txt | egrep -o "[0-9]+" >> readme.txt
```

Now let's take a look at `readme.txt`:


```bash
cat readme.txt
```

```
## This journal contains the following number of entries:
## 1
```

Looks like it worked! What do you think will happen if we run `make readme.txt`
again?


```bash
make readme.txt
```

```
## make: 'readme.txt' is up to date.
```

You guessed it: nothing happened! Since the `readme.txt` file still exists and
no changes were made to any of the dependencies for `readme.txt` (`toc.txt` is
the only dependency) `make` doesn't run the commands for the `readme.txt` rule.
Now let's modify `toc.txt` then we'll try running `make` again.


```bash
echo "2. 2017-06-16-IQSS-Talk" >> toc.txt
make readme.txt
```

```
## echo "This journal contains the following number of entries:" > readme.txt
## wc -l toc.txt | egrep -o "[0-9]+" >> readme.txt
```

Looks like it ran! Let's check readme.txt to make sure.


```bash
cat readme.txt
```

```
## This journal contains the following number of entries:
## 2
```

It looks like `make` successfully updated `readme.txt`! With every change to
`toc.txt`, running `make readme.txt` will *programmatically* update `readme.txt`.

In order to simplify the `make` experience, we can create a rule at the top of
our `makefile` called `all` where we can list all of the files that are built
by the `makefile`. By adding the `all` target we can simply run `make` without
any arguments in order to build all of the targets in the `makefile`. Let's 
open up `nano` and add this rule:


```bash
nano makefile
```

```
all: draft_journal_entry.txt readme.txt

draft_journal_entry.txt:
  touch draft_journal_entry.txt
  
readme.txt: toc.txt
  echo "This journal contains the following number of entries:" > readme.txt
  wc -l toc.txt | egrep -o "[0-9]+" >> readme.txt
```

While we have `nano` open let's add another special rule at the end of our
`makefile` called `clean` which destroys the files created by our `makefile`:

```
all: draft_journal_entry.txt readme.txt

draft_journal_entry.txt:
  touch draft_journal_entry.txt
  
readme.txt: toc.txt
  echo "This journal contains the following number of entries:" > readme.txt
  wc -l toc.txt | egrep -o "[0-9]+" >> readme.txt
  
clean:
  rm draft_journal_entry.txt
  rm readme.txt
```

Let's save and close our `makefile` then let's test it out.  First let's clean up
our repository:


```bash
make clean
ls
```

```
## rm draft_journal_entry.txt
## rm readme.txt
## makefile
## toc.txt
```


```bash
make
ls
```

```
## touch draft_journal_entry.txt
## echo "This journal contains the following number of entries:" > readme.txt
## wc -l toc.txt | egrep -o "[0-9]+" >> readme.txt
## draft_journal_entry.txt
## readme.txt
## makefile
## toc.txt
```

Looks like our `makefile` works! The `make` command is extremely powerful, and
this section is meant to just be an introduction. For more in-depth reading
about `make` I recommend [Karl Broman](https://twitter.com/kwbroman)'s
[tutorial](http://kbroman.org/minimal_make/) or 
[Chase Lambert](http://chaselambda.com)'s
[makefiletutorial.com](http://makefiletutorial.com).

### Summary

- `make` is a tool for creating relationships between files and programs, so
that files that depend on other files can be automatically rebuilt.
- `makefiles` are text files that contain a list of rules.
- Rules are made up of targets (files to be built), commands (a list of bash
commands that build the target), and dependencies (files that the target depends
on to be built).
