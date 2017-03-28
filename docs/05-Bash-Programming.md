# Bash Programming

> Communities begin by building their kitchen. - French proverb

The last two chapters have discussed how to use the bash shell. Bash itself is a
little programming language, and this chapter we're going to discuss how you can
write your own computer programs in Bash. Programming in Bash is useful to know
because of how seamlessly it integrates with all of the command line programs
you've already learned. By the end of this chapter you should be able to write 
your own command line tools!

You can use `nano` to write all of the programs we're going to discuss in this
chapter, however I recommend using the [Atom](https://atom.io/) text editor 
because it's more user friendly.

Now let's create a new file called `math.sh` in the `~/Code/` directory
and let's open that file with either `nano` or Atom.


```bash
cd ~/Code/
nano math.sh
```

You should now have a new, clean text file open. Any code block in the following
chapters that starts with the code (or similar) below should indicate to you
that we're working on a particular text file.

```
#!/usr/bin/env bash
# File: math.sh
```

You do not need to add these lines to your file, though you should type exactly
what I have typed below these lines. **Note**: please type all of the lines out
for every program that we're going to write, do not copy and paste. Typing code
is a little different from typing an email, and you should practice typing the
code out yourself as much as possible. Both of these lines start with the pound
symbol (`#`) and in the Bash programming language anything that is typed after
a pound symbol is ignored. The pound symbol allows you to make **comments** in
your code which you can use to annotate code so that another human being who is
reading your code can understand how your program is designed to function.

If you're using `nano` or another shell-based text editor you should perhaps
open up two terminals, one where you can edit and save the programs you're
working on, and one where you can run your programs. One advantage of using Atom
is that you can keep Atom open in a separate window and then run your programs
in your terminal window.

## Math

The Bash programming language can do very basic arithmetic, which we'll
demonstrate in this section.
Now that you have `math.sh` open in your preferred text editor type the following
into your text editor:

```
#!/usr/bin/env bash
# File: math.sh

expr 5 + 2
expr 5 - 2
expr 5 \* 2
expr 5 / 2
```

Save `math.sh` and then run this script in your shell:


```bash
bash math.sh
```

```
## 7
## 3
## 10
## 2
```

Let's break down what's going on in the Bash script you just created. Bash
executes programs in order from the fist line in your file to the last line.
The `expr` command can be used to **evaluate** Bash **expressions**.
An expression is just
a valid string of Bash code that, when run, produces a result. The arithmetic
operators that you're already familiar with for addition (`+`), subtraction
(`-`), and multiplication (`*`) work like you would expect them to. Notice that
when doing multiplication you need to escape the star character, otherwise
Bash thinks you're trying to create a regular expression! The division operator
(`/`) does not work as you might expect it to since 5 / 2 = 2.5. Bash does
**integer division**, which means that the result of dividing one number by
another is always rounded down to the nearest integer. Let's take a look at a
few examples on the command line:


```bash
expr 1 / 3
expr 10 / 3
expr 40 / 21
expr 40 / 20
```

```
## 0
## 3
## 1
## 2
```

The other numerical operator you should be aware of that you might not be
familiar with is the modulus operator (`%`). The modulus operator returns the
**remainder** after integer division. In integer division if A / B = C, and
A % B = D, then B * C + D = A. Let's take a look at some examples on the command
line:


```bash
expr 1 % 3
expr 10 % 3
expr 40 % 21
expr 40 % 20
```

```
## 1
## 1
## 19
## 0
```

Notice that when one number is completely divisible by another number then the
result of the modulus is zero.

If you want to do more complex math, for example math with fractions and numbers
with decimals then I highly suggest combining `echo` and the **b**ench **c**alculator
program called `bc`. Open up a new file called `bigmath.sh` and type in the
following:

```
#!/usr/bin/env bash
# File: bigmath.sh

echo "22 / 7" | bc -l
echo "4.2 * 9.15" | bc -l
echo "(6.5 / 0.5) + (6 * 2.2)" | bc -l
```

Save `bigmath.sh` and then run this script in your shell:


```bash
bash bigmath.sh
```

```
## 3.14285714285714285714
## 38.430
## 26.2
```

You can pipe any mathematical string to `bc` with the `-l` flag in order to
use decimal numbers in your calculations.

### Summary

- Bash programs are executed in order from the first line in a file until the
last line.
- Anything written after a pound sign (`#`) is a comment and is not executed by
Bash.
- You can do simple arithmetic with the `expr` command.
- Perform more complicated arithmetic by piping a string expression into `bc`
using `echo`.

### Exercises

1. Look at the `man` pages for `bc`.
2. Try doing some math in `bc` interactively.
3. Try writing some equations in a file and then provide that file as an
argument to `bc`.

## Variables

In Bash you can store data in variables. In chapter 4 we discussed environmental
variables that are set by your operating system. You can also create your own
variables. Make sure you follow these rules when you're naming variables:

- Every character should be lowercase.
- The variable name should start with a letter.
- The name should only contain alphanumeric characters and underscores (`_`).
- Words in the name should be separated by underscores.

If you follow those rules then you can avoid accidentally overwriting data
stored in environmental variables.

You can assign data to a variable using the equals sign (`=`). The data you
store in a variable can either be a string or a number. Let's create a variable
now on the command line:


```bash
chapter_number=5
```

The variable name is on the left hand side of the equals sign, and the data
which will be stored in that variable is on the right hand side of the equals
sign. Notice that there are no spaces on either side of the equals sign, this
is not allowed when assigning variables:


```bash
chapter_number = 5
```

```
## Error in running command bash
```

In order to print the data in a variable, also called the value of a variable,
we can use `echo`. When you want to retrieve the value of a variable you must
use the dollar sign (`$`) before the name of the variable. Let's try this out:


```bash
echo $chapter_number
```

```
## 5
```

You can modify the value of a variable using arithmetic operators by using the
`let` command:


```bash
let chapter_number=$chapter_number+1
echo $chapter_number
```

```
## 6
```

You can also store strings in variables:


```bash
the_empire_state="New York"
echo $the_empire_state
```

```
## New York
```

Occasionally you might want to run a command like you would on the command line
and store the result of that command in a variable. We can do this by wrapping
the command in a dollar sign and parentheses (`$( )`) around a command.
This syntax is called **command substitution**. The command is executed and then
gets replaced by the string that resulted from running the command. For
example if we wanted to store the number of lines in `math.sh`:


```bash
math_lines=$(cat math.sh | wc -l)
echo $math_lines
```

```
## 7
```

Variable names with a dollar sign can also be used inside other strings in
order to insert the value of the variable into the string:


```bash
echo "I went to school in $the_empire_state."
```

```
## I went to school in New York.
```

When writing a Bash script, the script gives you a few variables for free. Let's
create a new file called `vars.sh` with the following code:

```
#!/usr/bin/env bash
# File: vars.sh

echo "Script arguments: $@"
echo "First arg: $1. Second arg: $2."
echo "Number of arguments: $#"
```

Now let's try running the script a few times in a few different ways:


```bash
bash vars.sh
```

```
## Script arguments:
## First arg: . Second arg: .
## Number of arguments: 0
```


```bash
bash vars.sh red
```

```
## Script arguments: red
## First arg: red. Second arg: .
## Number of arguments: 1
```


```bash
bash vars.sh red blue
```

```
## Script arguments: red blue
## First arg: red. Second arg: blue.
## Number of arguments: 2
```


```bash
bash vars.sh red blue green
```

```
## Script arguments: red blue green
## First arg: red. Second arg: blue.
## Number of arguments: 3
```

Your script can accept arguments just like a command line program! The first
argument to your script is stored in `$1`, the second argument is stored in
`$2`, etc, etc. An array of all of the arguments passed to your script is stored
`$@`, and we'll discuss how to handle arrays later on in this chapter. The total
number of arguments passed to your script is stored in `$#`. Now that you know
how to pass arguments to your scripts you can start writing your own commnad
line tools!

### Summary

- Variables can be assigned with the equal sign (`=`) operator.
- Strings or numbers can be assigned to variables.
- The value of a variable can be accessed with the dollar sign (`$`) before the
variable name.
- You can use the dollar sign and parentheses syntax (command substitution) to
execute a command and save the output in a variable.
- You can access command line arguments within your own scripts using the dollar
sign followed by the number of the argumnet.

### Exercises

1. Write a Bash program where you assign two numbers to different variables,
and then the program prints the sum of those variables.
2. Write another Bash program where you assign two strings to different
variables, and then the program prints both of those strings. Write a version
where the strings are printed on the same line, and a version where the strings
are printed on different lines.
3. Write a Bash program that prints the number of arguments provided to that
program multiplied by the first argument providied to the program.

## User Input

If you're making Bash programs for you or for others to use one way you can get
user input is to specify arguments for users to provide to your program, as we
discussed in the previous section. You could also ask users to type in a string
on the command line by temporarily stopping the execution of your program using
the `read` command. Let's a write a small script where you can see how the read
command works:

```
#!/usr/bin/env bash
# File: letsread.sh

echo "Type in a string and then press Enter:"
read response
echo "You entered: $response"
```

Now let's run this script:


```bash
bash letsread.sh
```

```
## Type in a string and then press Enter:
## 
```

Let's type `Hello!` into the console, then press enter:

```
## Type in a string and then press Enter:
## Hello!
## You entered: Hello!
```

The `read` command prompts the user to type in a string, and the string that the
user provides is stored in the variable that is given to the `read` command in
the script.

### Summary

- `read` stores a string that the user provides in a variable.

### Exercises

1. Write a script that asks the user for an adjective, a noun, and a verb, and
then use those words in a sentence (like [Mad Libs](https://en.wikipedia.org/wiki/Mad_Libs)).

## Logic and If/Else

### Conditional Execution

When writing computer programs it is often useful for your program to be able to
make decisions based on inputs like arguments, files, and environmental
variables. Bash provides mechanisms for creating **logical expressions** which
resemble mathematical equations. These logical expressions can be evaluated
until they are either true or false. In fact, `true` and `false` are both simple
Bash commands! Let's try them both out now:


```bash
true
false
```

At first it doesn't look like they do much. In order to see how they work, we're
going to need to look under the hood of Unix a little bit. Whenever you exceute
a program on the command line, in general one of two things will happen: either
the command is executed successfully, or there's an error. In terms of errors
there are many ways that a program can go wrong, and Unix can take different
action depending on what kind of error occurs. For example if I enter the name
of a command that does not exist into the keyboard, then I'll see an error:


```bash
this_command_does_not_exist
```

```
## Error in running command bash
```

Since that command does not exist, it creates a specific kind of error which is
indicated by the program's **exit status**. The exit status of a program is an
integer, the exit status of the last program run is stored in the question mark
variable (`$?`). We can take a look at the exit status of the last program with
`echo`:


```bash
echo $?
```

```
## 127
```

This particular exit status made an indication to the shell that it should
print an error message to the console. What's the exit status of a program that
runs successfully? Let's take a look:


```bash
echo I will succeed.
echo $?
```

```
## I will succeed.
## 0 
```

So the exit status of a successful program is 0. Now let's take a look at the
exit statuses of `true` and `false`:


```bash
true
echo $?
false
echo $?
```

```
## 0
## 1
```

As you can see `true` has an exit status of 0 and `false` has an exit status of
1. Since there programs don't do much else, you could define `true` as a program
that always has an exit status of 0 and `false` as a program that always has an
exit status of 1.

Knowing the exit status of these programs is important when discussing the
**logical operators**: the AND operator (`&&`) and the OR operator (`||`). The
AND and OR operators can be used for conditional execution of programs on the
command line. Conditional execution occurs when the execution of one program
depends on the exit status of another program. For example in the case of the
AND operator, the program on the right hand side of `&&` will only be executed
if the program on the left hand side of `&&` has an exit status of 0. Let's
take a look at some small examples:


```bash
true && echo "Program 1 was executed."
false && echo "Program 2 was executed."
```

```
## Program 1 was executed.
```

Since `false` has an exit status of 1, the program `echo "Program 2 was executed."`
is not executed, so nothing is printed to the console for that line command.
Several AND operators can be chained together like so:


```bash
false && true && echo Hello
echo 1 && false && echo 3
echo Athos && echo Porthos && echo Aramis
```

```
## 1
## Athos
## Porthos
## Aramis
```

In a series of programs joined toegther by AND operators, any programs to the
right of a program that has a non-zero exit status is not executed.

The OR operator (`||`) follows a similar set of principles. Commands on the right
hand side of `||` are only executed if the command on the left hand side *fails*
and therefore has an exit status other than 0. Let's take a look at how this
works:


```bash
true || echo "Program 1 was executed."
false || echo "Program 2 was executed."
```

```
## Program 2 was executed.
```

Only `echo "Program 2 was executed."` runs because `false` has a non-zero exit
status. You can combine multiple OR operators so that only the first program
with an exit status of 0 is executed:


```bash
false || echo 1 || echo 2
echo 3 || false || echo 4
echo Athos || echo Porthos || echo Aramis
```

```
## 1
## 3
## Athos
```

You can combine AND and OR operators in commands, which are evaluated from left
to right:


```bash
echo Athos || echo Porthos && echo Aramis
echo Gaspar && echo Balthasar || echo Melchior
```

```
## Athos
## Aramis
## Gaspar
## Balthasar
```

By combining AND and OR operators you can precisely control the conditions for
when certain commands should be executed.

### Conditional Expressions

Enabling your Bash script to make decisions is extremely useful. Conditional
execution allows you to control the circumstances where certain programs are
executed based on whether those programs succeed or fail, but you can also
construct **conditional expressions** which are logical statements that are
either equivalent to `true` or `false`. Conditional expressions either compare
two values, or they ask a question about one value. Conditional expressions are
always between double brackets (`[[ ]]`), and they either use **logical flags**
or **logical operators**. For example, there are several logical flags you could
use for comparing two integers. If we wanted to see if one integer was greater
than another we could use `-gt`, the **g**reater **t**han flag. Enter this
simple conditional expression into the command line:


```bash
[[ 4 -gt 3 ]]
```

The logical expression above is asking: Is 4 greater than 3? No result is
printed to the console so let's check the exit status of that expression.


```bash
echo $?
```

```
## 0
```

It looks like the exit status of this program is 0, the same exit status as
`true`. This conditional expression is saying that `[[ 4 -gt 3 ]]` is equivalent
to `true`, which of course we know is logically consistent, 4 is in fact greater
than 3! Let's see what happens if we flip the expression around so we're asking
if 3 is greater than 4:


```bash
[[ 3 -gt 4 ]]
```

Again, nothing is printed to the console so we'll look at the exit status:


```bash
echo $?
```

```
## 1
```

Ah-ha! Obviously 3 is not greater than 4, so this false logical expression 
resulted in an exit status of 1, which is the same exit status as `false`! 
Because they have the same exit status `[[ 3 -gt 4 ]]` and `false` are
essentially equivalent. To quickly test the logical value of a conditional
expression, we can use the AND and OR operators so that an expression will
print "t" if it's true and "f" if its false:


```bash
[[ 4 -gt 3 ]] && echo t || echo f
[[ 3 -gt 4 ]] && echo t || echo f
```

```
## t
## f
```

This is a little trick you can use to quickly look at the resulting value of a
logical expression.

These **binary** logical expressions compare two values, but there are also
**unary** logical expressions that only look at one value. For example, you can
test whether or not a file exists using the `-e` logical flag. Let's take a look
at this flag in action:


```bash
cd ~/Code
[[ -e math.sh ]] && echo t || echo f
```

```
## t
```

As you can see the file `math.sh` exists! Most of the time when you'r writing
bash scripts you won't be comparing two raw values or trying to find something
out about one raw value, instead you'll want to create a logical statememnt
about a value contained ina variable. Variables behave just like raw values in
logical expressions. Let's take a look at a few examples:


```bash
number=7
[[ $number -gt 3 ]] && echo t || echo f
[[ $number -gt 10 ]] && echo t || echo f
[[ -e $number ]] && echo t || echo f
```

```
## t
## f
## f
```


| Logical Flag | Meaning | Usage |
|:-------------|:--------|:------|
| -gt | **G**reater **T**han | `[[ $planets -gt 8 ]]` |
| -ge | **G**reater Than or **E**qual To | `[[ $votes -ge 270 ]]` |
| -eq | **Eq**ual | `[[ $fingers -eq 10 ]]` |
| -ne | **N**ot **E**qual | `[[ $pages -ne 0 ]]` |
| -le | **L**ess Than or **E**qual To | `[[ $candles -le 8 ]]` |
| -lt | **L**ess **T**han | `[[ $wives -lt 2 ]]` |
| -e | A File **E**xists | `[[ -e $taxes_2016 ]]` |
| -d | A **D**irectory Exists | `[[ -d $photos ]]` |
| -z | Length of String is **Z**ero | `[[ -z $name ]]` |
| -n | Length of String is **N**on-Zero | `[[ -n $name ]]` |

| Logical Operator | Meaning | Usage |
|:-------------|:--------|:------|
| =~ | Matches Regular Expression | `[[ $consonants =~ [aeiou] ]]` |
| = | Equal To | `[[ $fingers = 10 ]]` |
| != | Not Equal To | `[[ $pages != 0 ]]` |
| ! | Not | `[[ ! "apple" =~ ^b ]]` |



## Arrays

## Braces

## Loops

## Functions

## Writing Programs

### Exercises

Write a program that takes one number as an argument and prints
"the number is even" if the number is even, "the number is odd" if the number
is odd, and "the number is less than 1" if the number is less than 1.


```bash
evenodd 6
```

```
## the number is even
```


```bash
evenodd 19
```

```
## the number is odd
```


```bash
evenodd 0
```

```
## the number is less than 1
```

Write a program that takes many numbers as arguments and prints the largest
number. Add a flag to specify whether it prints the minimum or maximum number.


```bash
findn -max 8 2 9 4 0 3
```

```
## 9
```

Write a program that takes one number as an argument and prints all of the
numbers
