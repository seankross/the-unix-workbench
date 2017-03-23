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
I recently downloaded a list of the names of the states in the US. Let's take a
look at this file:


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
numbers, and even the underscore character (`_`). We can see the compliment of
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
compliment of a set by including a caret (`^`) in the beginning of a set. For
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
|    {4, 6}     |     Between 4 and 6 or Previous      |
|     {4, }     |       More than 4 of Previous        |

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

## Make

## Configure

- .bash_profile, .bash_history, alias, PATH and other env vars

## Differentiate

Sometimes you need to be able examine differences between files. First let's
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
## Alabama							Alabama
## Alaska								Alaska
## Arizona							Arizona
## Arkansas							Arkansas
## 							      >	California
## 							      >	Colorado
```

Sometimes you might be sent a file, or you might download a file from the 
internet that comes with code known as a **checksum** or a **hash**. Hashing
programs generate a unique code based on the contents of a file. People
distribute hashes with files so that you can be sure that the file you think
you're downloading is the genuine file????????????????????????


There are a few commonly used file hashes but we'll talk about two called MD5
and SHA-1.


```bash
cp states.txt states_copy.txt
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

## Connect

- pipes, input redirection
