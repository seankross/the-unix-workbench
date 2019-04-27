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
a pound symbol is ignored (unless the pound symbol is between curly brackets
(`{ }`), but that's only in very specific situations). The pound symbol allows
you to make **comments** in
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
executes programs in order from the first line in your file to the last line.
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

- Every letter should be lowercase.
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
in `$@`, and we'll discuss how to handle arrays later on in this chapter. The
total number of arguments passed to your script is stored in `$#`. Now that you
know how to pass arguments to your scripts you can start writing your own command
line tools!

### Summary

- Variables can be assigned with the equal sign (`=`) operator.
- Strings or numbers can be assigned to variables.
- The value of a variable can be accessed with the dollar sign (`$`) before the
variable name.
- You can use the dollar sign and parentheses syntax (command substitution) to
execute a command and save the output in a variable.
- You can access command line arguments within your own scripts using the dollar
sign followed by the number of the argument.

### Exercises

1. Write a Bash program where you assign two numbers to different variables,
and then the program prints the sum of those variables.
2. Write another Bash program where you assign two strings to different
variables, and then the program prints both of those strings. Write a version
where the strings are printed on the same line, and a version where the strings
are printed on different lines.
3. Write a Bash program that prints the number of arguments provided to that
program multiplied by the first argument provided to the program.

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
going to need to look under the hood of Unix a little bit. Whenever you execute
a program on the command line, in general one of two things will happen: either
the command is executed successfully, or there's an error. In terms of errors
there are many ways that a program can go wrong, and Unix can take different
actions depending on what kind of error occurs. For example if I enter the name
of a command that does not exist into the terminal, then I'll see an error:


```bash
this_command_does_not_exist
```

```
## Error in running command bash
```

Since that command does not exist, it creates a specific kind of error which is
indicated by the program's **exit status**. The exit status of a program is an
integer which indicates whether the program was executed successfully or if
an error occurred.
The exit status of the last program run is stored in the question mark
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
1. Since these programs don't do much else, you could define `true` as a program
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
is not executed, so nothing is printed to the console for that command.
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

In a series of programs joined together by AND operators, any programs to the
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

As you can see the file `math.sh` exists! Most of the time when you're writing
bash scripts you won't be comparing two raw values or trying to find something
out about one raw value, instead you'll want to create a logical statement
about a value contained in a variable. Variables behave just like raw values in
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

As you can see 7 is greater than 3 though it is not greater than 10, and there
is not file in this directory called `7`. There are several other varieties of
logical flags, and you can find a table of several of these flags below.

| Logical Flag | Meaning | Usage |
|:-------------|:--------|:------|
| -gt | **G**reater **T**han | `[[ $planets -gt 8 ]]` |
| -ge | **G**reater Than or **E**qual To | `[[ $votes -ge 270 ]]` |
| -eq | **Eq**ual | `[[ $fingers -eq 10 ]]` |
| -ne | **N**ot **E**qual | `[[ $pages -ne 0 ]]` |
| -le | **L**ess Than or **E**qual To | `[[ $candles -le 9 ]]` |
| -lt | **L**ess **T**han | `[[ $wives -lt 2 ]]` |
| -e | A File **E**xists | `[[ -e $taxes_2016 ]]` |
| -d | A **D**irectory Exists | `[[ -d $photos ]]` |
| -z | Length of String is **Z**ero | `[[ -z $name ]]` |
| -n | Length of String is **N**on-Zero | `[[ -n $name ]]` |

Try using each of these flags on the command line before moving on to the next
section.

In addition to logical flags there are also logical operators. One of the most
useful logical operators is the regex match operator `=~`. The regex match
operator compares a string to a regular expression and if the string is a match
for the regex then the expression is equivalent to `true`, otherwise it's
equivalent to `false`. Let's test this operator a couple different ways:


```bash
[[ rhythms =~ [aeiou] ]] && echo t || echo f
my_name=sean
[[ $my_name =~ ^s.+n$ ]] && echo t || echo f
```

```
## f
## t
```

There's also the NOT operator `!`, which inverts the value of any conditional
expression. The NOT operator turns true expressions into false expressions and
vice-versa. Let's take a look at a few examples using the NOT operator:


```bash
[[ 7 -gt 2 ]] && echo t || echo f
[[ ! 7 -gt 2 ]] && echo t || echo f
[[ 6 -ne 3 ]] && echo t || echo f
[[ ! 6 -ne 3 ]] && echo t || echo f
```

```
## t
## f
## t
## f
```

Here's a table of some of the useful logical operators in case you need to
reference how they're used later:

| Logical Operator | Meaning | Usage |
|:-------------|:--------|:------|
| =~ | Matches Regular Expression | `[[ $consonants =~ [aeiou] ]]` |
| = | String Equal To | `[[ $password = "pegasus" ]]` |
| != | String Not Equal To | `[[ $fruit != "banana" ]]` |
| ! | Not | `[[ ! "apple" =~ ^b ]]` |

### If and Else

Conditional expressions are powerful because you can use them to control how a
Bash program that you're writing is executed. One of the fundamental constructs
in Bash programming is the **IF statement**. Code written inside of an IF
statement is only executed *if* a certain condition is true, otherwise the code
is skipped. Let's write a small program with an IF statement:

```
#!/usr/bin/env bash
# File: simpleif.sh

echo "Start program"

if [[ $1 -eq 4 ]]
then
  echo "You entered $1"
fi

echo "End program"
```

First this program will print "Start program", then the IF statement will check
if the conditional expression `[[ $1 -eq 4 ]]` is true. It will only be true if
you provide `4` as the first argument to the script. If the conditional
expression if true then it will execute the code in between `then` and `fi`,
otherwise it will skip over that code. Finally the program will print
"End program."

Let's try running this Bash program a few different ways. First we'll run this
program with no arguments:


```bash
bash simpleif.sh
```

```
## Start program
## End program
```

Since we didn't provide any arguments to `simpleif.sh` the code within the IF
statement was skipped! Now let's try providing an argument to this script:


```bash
bash simpleif.sh 77
```

```
## Start program
## End program
```

We provided the argument `77`, however 77 is not equal to 4, therefore the code
within the IF statement was once again skipped. Finally let's provide `4` as an
argument:


```bash
bash simpleif.sh 4
```

```
## Start program
## You entered 4
## End program
```

It worked! Since the first argument to this script was `4`, and 4 is equal to 4,
the code within the IF statement was executed. You can pair IF statements with
ELSE statements. An ELSE statement only runs if the conditional expression being
evaluated by the IF statement is false. Let's create a simple program that uses
an ELSE statement:

```
#!/usr/bin/env bash
# File: simpleifelse.sh

echo "Start program"

if [[ $1 -eq 4 ]]
then
  echo "Thanks for entering $1"
else
  echo "You entered: $1, not what I was looking for."
fi

echo "End program"
```

Now let's try running this program a few different ways:


```bash
bash simpleifelse.sh 4
```

```
## Start program
## Thanks for entering 4
## End program
```

The conditional expression `[[ $1 -eq 4 ]]` was true so code inside of the IF
statement was run and the code in the ELSE statement was not run. What do you
think will happened when we make the conditional expression false?


```bash
bash simpleifelse.sh 3
```

```
## Start program
## You entered: 3, not what I was looking for.
## End program
```

The conditional expression `[[ $1 -eq 4 ]]` was false so code inside of the ELSE
statement was run and the code in the IF statement was not run.

Between IF and ELSE statements you can also have ELIF statements. These
statements act like IF statements except they're only evaluated if preceding
IF and ELIF statements have all evaluated false conditional expressions. Let's
create a brief program using ELIF:

```
#!/usr/bin/env bash
# File: simpleelif.sh

if [[ $1 -eq 4 ]]
then
  echo "$1 is my favorite number"
elif [[ $1 -gt 3 ]]
then
  echo "$1 is a great number"
else
  echo "You entered: $1, not what I was looking for."
fi
```

First let's run the program with `4` as the first argument:


```bash
bash simpleelif.sh 4
```

```
## 4 is my favorite number
```

The condition in the IF statement was true, so only the first `echo` command was
executed. Now let's run the program with `5` as the first argument:


```bash
bash simpleelif.sh 5
```

```
## 5 is a great number
```

The first condition is false since 5 is not equal to 4, but then the next
condition in the ELIF statement is true since 5 is greater than 3, so that
echo command is executed and the rest of the statement is skipped. Try to guess
what will happen if we use `2` as an argument:


```bash
bash simpleelif.sh 2
```

```
## You entered: 2, not what I was looking for.
```

Since 2 is neither equal to 4 nor greater than 3, the code in the ELSE statement
is executed.

You should also know that you can combine conditional execution, conditional
expressions, and IF/ELIF/ELSE statements. The conditional execution operators
AND (`&&`) and OR (`||`) can be used in an IF or ELIF statement. Let's look at
an example using these operators in an IF statement:

```
#!/usr/bin/env bash
# File: condexif.sh

if [[ $1 -gt 3 ]] && [[ $1 -lt 7 ]]
then
  echo "$1 is between 3 and 7"
elif [[ $1 =~ "Jeff" ]] || [[ $1 =~ "Roger" ]] || [[ $1 =~ "Brian" ]]
then
  echo "$1 works in the Data Science Lab"
else
  echo "You entered: $1, not what I was looking for."
fi
```
Now let's test this script with a few different arguments:


```bash
bash condexif.sh 2
bash condexif.sh 4
bash condexif.sh 6
bash condexif.sh Jeff
bash condexif.sh Brian
bash condexif.sh Sean
```

```
## You entered: 2, not what I was looking for.
## 4 is between 3 and 7
## 6 is between 3 and 7
## Jeff works in the Data Science Lab
## Brian works in the Data Science Lab
## You entered: Sean, not what I was looking for.
```

The conditional execution operators work just like they would on the command
line. If the entire conditional expression evaluates to the equivalent of `true`
then the code within the IF statement is executed, otherwise it is skipped.

Finally we should note that IF/ELIF/ELSE statements can be nested inside of
other IF statements. Here's a small example of a program with nested statements:

```
#!/usr/bin/env bash
# File: nested.sh

if [[ $1 -gt 3 ]] && [[ $1 -lt 7 ]]
then
  if [[ $1 -eq 4 ]]
  then
  	echo "four"
  elif [[ $1 -eq 5 ]]
  then
  	echo "five"
  else
  	echo "six"
  fi
else
  echo "You entered: $1, not what I was looking for."
fi
```

Now let's run it a few times:


```bash
bash nested.sh 2
bash nested.sh 4
bash nested.sh 6
```

```
## You entered: 2, not what I was looking for.
## four
## six
```

In order to get to the inner IF statement, the conditions for the outer IF
statement must be met first (the first argument for the script must be between
3 and 7). As you can see combining variables, arguments, conditional
expressions, and IF statements allow you to write more powerful Bash programs.


### Summary

- All Bash programs have an exit status. `true` has an exit status of 0 and
`false` has an exit status of 1.
- Conditional execution uses two operators: AND (`&&`) and OR (`||`) which you
can use to control what command get executed based on their exit status.
- Conditional expressions are always in double brackets (`[[ ]]`). They have
an exit status of 0 if they contain a true assertion or 1 if they contain
a false assertion.
- IF statements evaluate conditional expressions. If an expression is true then
the code within an IF statement is executed, otherwise it is skipped.
- ELIF and ELSE statements also help control the flow of a Bash program, and IF
statements can be nested within other IF statements.

### Exercises

1. Write a Bash script that takes a string as an argument and prints
"how proper" if the string starts with a capital letter.
2. Write a Bash script that takes one argument and prints "even" if the first
argument is an even number or "odd" if the first argument is an odd number.
3. Write a Bash script that takes two arguments. If both arguments are numbers,
print their sum, otherwise just print both arguments.
4. Write a Bash script that prints "Thank Moses it's Friday" if today is Friday.
(Hint: take a look at the `date` program).

## Arrays

Arrays in Bash are ordered lists of values. You can create a list from scratch
by assigning it to a variable name. Lists are created with parentheses (`( )`)
with a space separating each element in the list. Let's make a list of the
plagues of Egypt:


```bash
plagues=(blood frogs lice flies sickness boils hail locusts darkness death)
```

To retrieve the array you need to use **parameter expansion**, which involves
the dollar sign and curly brackets (`${ }`). The positions of the elements in
the array are numbered starting from zero. To get the first element of this
array use `${plagues[0]}` like so:


```bash
echo ${plagues[0]}
```

```
## blood
```

Notice that the first element has an **index** of 0. You can get any of the
elements this way, for example the fourth element:


```bash
echo ${plagues[3]}
```

```
## flies
```

To get all of the elements of `plagues` use a star (`*`) between the square
brackets:


```bash
echo ${plagues[*]}
```

```
## blood frogs lice flies sickness boils hail locusts darkness death
```

You can also change an individual elements in the array by specifying their
index with square brackets:


```bash
echo ${plagues[*]}
plagues[4]=disease
echo ${plagues[*]}
```

```
## blood frogs lice flies sickness boils hail locusts darkness death
## blood frogs lice flies disease boils hail locusts darkness death
```

To get only part of an array you have to specify the index you would like to
start at, followed by the number of elements you would like to retrieve from the
array, separated by colons:


```bash
echo ${plagues[*]:5:3}
```

```
## boils hail locusts
```

The above query essentially says: get 3 array elements starting from the sixth
element of the array (remember, the sixth element has an index of 5).

You can find the length of an array using the pound sign (`#`):


```bash
echo ${#plagues[*]}
```

```
## 10
```

You can use the plus-equals operator (`+=`) to add an array onto the end of
an array array:


```bash
dwarfs=(grumpy sleepy sneezy doc)
echo ${dwarfs[*]}
dwarfs+=(bashful dopey happy)
echo ${dwarfs[*]}
```

```
## grumpy sleepy sneezy doc
## grumpy sleepy sneezy doc bashful dopey happy
```

### Summary

- Arrays are a linear data structure with ordered elements which can be stored
in variables.
- The each element of an array has an index and the first index is 0.
- Individual elements of an array can be accessed using their index.

### Exercises

1. Write a bash script where you define an array inside of the script, and the
first argument for the script indicates the index of the array element that is
printed to the console when the script is run.
2. Write a bash script where you define two arrays inside of the script, and the
sum of the lengths of the arrays are printed to the console when the script is
run.

## Braces

Bash has a very handy tool for creating strings out of sequences called
**brace expansion**. Brace expansion uses the curly brackets and two periods
(`{ .. }`) to create a sequence of letters or numbers. For example to create a
string with all of the numbers between zero and nine you could do the following:


```bash
echo {0..9}
```

```
## 0 1 2 3 4 5 6 7 8 9
```

In addition to numbers you can also create sequences of letters:


```bash
echo {a..e}
echo {W..Z}
```

```
## a b c d e
## W X Y Z
```

You can put strings on either side of the curly brackets and they'll be "pasted"
onto the corresponding end of the sequence:


```bash
echo a{0..4}
echo b{0..4}c
```

```
## a0 a1 a2 a3 a4
## b0c b1c b2c b3c b4c
```

You can also combine sequences so that two or more sequences are pasted
together:


```bash
echo {1..3}{A..C}
```

```
## 1A 1B 1C 2A 2B 2C 3A 3B 3C
```

If you want to use variables in order to define a sequence you need to use the
`eval` command in order to create the sequence:


```bash
start=4
end=9
echo {$start..$end}
eval echo {$start..$end}
```

```
## {4..9}
## 4 5 6 7 8 9
```

You can combine sequences with a comma between brackets (`{,}`):


```bash
echo {{1..3},{a..c}}
```

```
## 1 2 3 a b c
```

In fact you can do this with any number of strings:


```bash
echo {Who,What,Why,When,How}?
```

```
## Who? What? Why? When? How?
```

### Summary

- Braces allow you create string sequences and expansions.
- To use variables with braces you need to use the `eval` command.

### Exercises

1. Create 100 text files using brace expansion.

## Loops

### `for`

Loops are one of the most important programming structures in the Bash language.
All of the programs we've written so far are executed from the first line of the
script until the last line, but loops allow you to repeat lines of code based on
logical conditions or by following a sequence. The first kind of loop that we'll
discuss is a FOR loop. **FOR loops** iterate through every element of a sequence
that you specify. Let's take a look at a small example FOR loop:

```
#!/usr/bin/env bash
# File: forloop.sh

echo "Before Loop"

for i in {1..3}
do
	echo "i is equal to $i"
done

echo "After Loop"
```

Now let's execute this script:


```bash
bash forloop.sh
```

```
## Before Loop
## i is equal to 1
## i is equal to 2
## i is equal to 3
## After Loop
```

Let's walk through `forloop.sh` line-by-line. First `"Before Loop"` is printed
before the FOR loop, then the loop begins. FOR loops start with the syntax
`for [variable name] in [sequence]` followed by `do` on the next line. The
variable name that you define immediately after `for` will take on a value
inside of the loop that corresponds to an element in the sequence you provide
after `in`, starting with the first element of the sequence, followed by every
subsequent element. Valid sequences include brace expansions, explicit lists of strings,
arrays, and command substitutions. In this instance we're using the brace
expansion `{1..3}` which we know expands to the string `"1 2 3"`. The code
executed in each iteration of the loop is written
between `do` and `done`. In the first iteration of the loop, the variable `$i`
contains the value 1. The string `"i is equal to 1"` is printed to the console.
There are more elements in the brace expansion after 1, so after
reaching `done` the first time, the program starts executing back at the
`do` statement. The second time through the loop variable `$i` contains the
value 2. The string
`"i is equal to 2"` is printed to the console, then the loop goes back to the
`do` statement since there are still elements left in the sequence. The `$i`
variable is now equal to 3, so `"i is equal to 3"` is printed to the console.
There are no elements left in the sequence, so the program moves beyond the
FOR loop and finally prints `"After Loop"`. Stop for a moment and edit this loop
yourself. Try changing the brace expansion to include other sequences of
numbers, letters, or words, then execute the modified code. Before you execute
your modified program, write down what you think will be printed. How do the
results of executing your program compare with your expectations?

Once you've experimented a little take a look at this example with several
other kinds of sequence generating strategies:

```
#!/usr/bin/env bash
# File: manyloops.sh

echo "Explicit list:"

for picture in img001.jpg img002.jpg img451.jpg
do
	echo "picture is equal to $picture"
done

echo ""
echo "Array:"

stooges=(curly larry moe)

for stooge in ${stooges[*]}
do
	echo "Current stooge: $stooge"
done

echo ""
echo "Command substitution:"

for code in $(ls)
do
	echo "$code is a bash script"
done
```


```bash
bash manyloops.sh
```

```
## Explicit list:
## picture is equal to img001.jpg
## picture is equal to img002.jpg
## picture is equal to img451.jpg
##
## Array:
## Current stooge: curly
## Current stooge: larry
## Current stooge: moe
##
## Command substitution:
## bigmath.sh is a bash script
## condexif.sh is a bash script
## forloop.sh is a bash script
## letsread.sh is a bash script
## manyloops.sh is a bash script
## math.sh is a bash script
## nested.sh is a bash script
## simpleelif.sh is a bash script
## simpleif.sh is a bash script
## simpleifelse.sh is a bash script
## vars.sh is a bash script
```

The example above illustrates three other methods of creating sequences for FOR
loops: typing out an explicit list, using an array, and getting the result of a
command substitution. In each case a variable name is declared after the `for`,
and the value of tha variable changes through each iteration of the loop until
the corresponding sequence has been exhausted. Right now you should take a
moment to write a few FOR loops yourself, generating sequences in all of the
ways that we've gone over, just to reinforce your understanding of how a FOR
loop works. Loops and conditional statements are two of the most important
structures that we have at our disposal as programmers.

### `while`

Now that we've gotten a few FOR loops working let's move on to WHILE loops. The
**WHILE loop** is truly the [Reese's Peanut Butter Cup](https://youtu.be/O7oD_oX-Gio)
of programming structures, combining parts of the FOR loop and the IF statement.
Let's take a look at an example WHILE loop so you can see what I mean:

```
#!/usr/bin/env bash
# File: whileloop.sh

count=3

while [[ $count -gt 0 ]]
do
  echo "count is equal to $count"
  let count=$count-1
done
```

The WHILE loop begins first with the `while` keyword followed by a conditional
expression. As long as the conditional expression is equivalent to `true` when
an iteration of the loop begins, then
the code within the WHILE loop will continue to be executed. Based on the code
for `whileloop.sh` what do you think will be printed to the console when we run
this script? Let's find out:


```bash
bash whileloop.sh
```

```
## count is equal to 3
## count is equal to 2
## count is equal to 1
```

Before the WHILE the `count` variable is set to be 3, but then each
time the WHILE loop is executed 1 is subtracted from the value of `count`. The
loop then starts from the top again and the conditional expression is
re-checked to see if it's still equivalent to `true`. After three iterations
through the loop `count` is equal to 0 since 1 is subtracted from `count` in
every iteration. Therefore
the logical expression `[[ $count -gt 0 ]]` is no longer equal to `true` and
the loop ends. By changing the value of the variable in the logical expression
inside of the loop we're able to ensure that the logical expression will
eventually be equivalent to `false`, and therefore the loop will eventually end.

If the logical expression is never equivalent to `false` then we've created an
*infinite loop*, so the loop never ends and the program runs forever. Obviously we
would like for our programs to end eventually, and therefore creating infinite
loops is undesirable. However let's create an infinite loop so we know what to
do if we get into a situation where our program won't terminate. With a simple
"typo" we can change the program above so that it runs forever but substituting
the minus sign `-` with a plus sign `+` so that `count` is always greater than
zero (and growing) after every iteration.

```
#!/usr/bin/env bash
# File: foreverloop.sh

count=3

while [[ $count -gt 0 ]]
do
  echo "count is equal to $count"
  let count=$count+1              # We only changed this line!
done
```

```
## ...
## count is equal to 29026
## count is equal to 29027
## count is equal to 29028
## count is equal to 29029
## count is equal to 29030
## ...
```

If the program is working, then `count` is being incremented very rapidly and
you're watching numbers whiz by in your terminal! Don't fret, you can terminate
any program that's stuck in an infinite loop using `Control` + `C`. Use
`Control` + `C` to get the prompt back so that we can continue.

When constructing WHILE loops, make absolutely sure that you've structured the
program so that the loop will terminate! If the logical expression after `while`
never becomes `false` then the program will run forever, which is probably not
the kind of behavior you were planning for your program.

### Nesting

Just like IF statements `for` and `while` loops can be nested within each other.
In the example below a FOR loop is nested inside of another FOR loop.

```
#!/usr/bin/env bash
# File: nestedloops.sh

for number in {1..3}
do
  for letter in a b
  do
    echo "number is $number, letter is $letter"
  done
done
```

Based on what we know about FOR loops try to predict what this program will
print out before we run the program. Now that you've written down or typed out
your prediction let's run it.


```bash
bash nestedloops.sh
```

```
## number is 1, letter is a
## number is 1, letter is b
## number is 2, letter is a
## number is 2, letter is b
## number is 3, letter is a
## number is 3, letter is b
```

Let's closely examine what's going on here. The outer most FOR loop starts
iterating through the sequence generated by `{1..3}`. On the first pass through
the loop, the inner loop iterates through the sequence `a b` which first prints
`number is 1, letter is a` followed by `number is 1, letter is b`. The first
iteration of the outer loop is then finished and the whole process starts over
with `number` having a value of 2. This process continues going through the
inner loop until the sequence for the outer loop is exhausted. I again strongly
encourage you to pause for a moment and write some of your own nested loops
based on the code above. Try to predict what your nested loop program will print
before you run your program. If the printed result does not match your
prediction trace your way through the program and try to figure out why. Don't
just limit yourself to nested FOR loops, use nested WHILE loops, or FOR and
WHILE loops in nested combinations.

Besides nesting loops within each other you can also nest loops within IF
statements and IF statements within loops. Let's take a look at an example:

```
#!/usr/bin/env bash
# File: ifloop.sh

for number in {1..10}
do
  if [[ $number -lt 3 ]] || [[ $number -gt 8 ]]
  then
    echo $number
  fi
done
```

Before we run this example try once more to guess what the output will be.


```bash
bash ifloop.sh
```

```
## 1
## 2
## 9
## 10
```

For each iteration of the loop above, the value of `number` was checked in the
IF statement, and the `echo` command was only run if `number` was outside the
range from 3 to 8.

There are endless combinations for nesting IF statements and loops, but one good
rule of thumb you should remember is that your nesting should never go more than
two or possibly three layers deep. If you find yourself writing code with lots
of nesting, you should consider restructuring your program. Deeply nested code
is difficult to read and even more difficult to debug if your program contains
mistakes.

### Summary

- Loops allows you repeat sections of your program.
- FOR loops iterate through a sequence so that a variable that you assign
takes on the value of every element of the sequence in every iteration of the
loop.
- WHILE loops check a conditional statement at the beginning of every iteration.
If the condition is equivalent to `true` then one iteration of the loop is
executed and then the conditional statement is checked again. Otherwise the
loop ends.
- IF statements and loops can be nested in order to make more powerful
programming structures.

### Exercises

- Write several programs with three levels of nesting and include FOR loops,
WHILE loops, and IF statements. Before you run your program try to predict what
your program is going to print. If the result is different from your prediction
try to figure out why.
- Enter the `yes` command into the console, then stop the program from running.
Take a look at the `man` page for `yes` to learn more about the program.

## Functions

### Writing Functions

A function is a small piece of code that has a name. Writing functions allows
us to re-use the same code multiple times across programs. Functions have the
the following syntax:

```
function [name of function] {
  # code here
}
```

Pretty simple, right? Let's open up a new file called `hello.sh` so we can write
our first simple function.

```
#!/usr/bin/env bash
# File: hello.sh

function hello {
  echo "Hello"
}

hello
hello
hello
```

The entire structure of the function including the `function` keyword, the name
of the function, and the code for the function written inside of the brackets
serves as the **function definition**. The function definition assigns the code
within the function to the name of the function (`hello` in this case). After a
function is defined it can be used like any other command. Using our `hello`
command three times should be the equivalent of using `echo "Hello"` three
times. Let's run this script to find out:


```bash
bash hello.sh
```

```
## Hello
## Hello
## Hello
```

It looks like this function works exactly like we expected.

Functions share lots of their behavior with individual bash scripts including
how they handle arguments. The usual bash script arguments like `$1`, `$2`, and
`$@` all work within a function, which allows you to specify function arguments.
Let's create a slightly modified version of `hello.sh` which we'll call
`ntmy.sh`:

```
#!/usr/bin/env bash
# File: ntmy.sh

function ntmy {
  echo "Nice to meet you $1"
}
```

In the file above notice that we're not using the `ntmy` function after we've
defined it. That's because we're going to start using the functions that we
define as command line programs. So far in this chapter we've been using the
syntax of `bash [name of script]` in order to execute the contents of a script.
Now we're going to start using the `source` command, which allows us to use
function definitions in bash scripts as command line commands. Let's use
`source` with this file so that we can then use the `ntmy` command:


```bash
source ntmy.sh
ntmy Jeff
ntmy Philip
ntmy Jenny
```

```
## Nice to meet you Jeff
## Nice to meet you Philip
## Nice to meet you Jenny
```

And just like that you've created your very own command! Once you close your
current shell you'll lose access to the `ntmy` command, but in the next section
we'll discuss how to set up your own commands so that you always have access to
them.

Let's write a more complicated function. Imagine that we wanted to add up
a sequence of numbers from the command line, but we had no way of knowing how
many numbers would be in the sequence. What components would we need to write
this function? First we would need a way to capture a list of arguments which
can have variable length, second we would need a way to iterate through that
list so we could add up each element, and we would need a way to store the
cumulative sum of the sequence. These three requirements can be satisfied by
using the `$@` variable, a FOR loop, and variable where we can store the sum.
It's important to break down a larger goal into a series of individual
components before writing a program, that way we more easily can identify which
features and tools will be required. Let's write this program in a file called
`addseq.sh`.

```
#!/usr/bin/env bash
# File: addseq.sh

function addseq {
  sum=0

  for element in $@
  do
    let sum=sum+$element
  done

  echo $sum
}
```

In the program above we initialize the `sum` variable to be 0 so that we can
add other values in the sequence to `sum`. We then use a FOR loop to iterate
through every element of `$@`, which is an array of all the arguments we provide
to `addseq`. Finally we `echo` the value of `sum`. Let's `source` this program
and test it out:


```bash
source addseq.sh
addseq 12 90 3
addseq 0 1 1 2 3 5 8 13
addseq
addseq 4 6 6 6 4
```

```
## 105
## 33
## 0
## 26
```

By breaking down a large problem we were able to write a nice little function!

### Getting Values from Functions

Functions are used for two primary purposes: *computing values* and
*side effects*. In the `addseq` command in the previous section we provide the
command with a sequence of numbers and then the command provides us with the sum
of the sequence which is a value that we're interested in. In this case we can
see that `addseq` has computed a value based on a few input values.
Many other commands,
like `pwd` for example, return a value without affecting the state of the file
on our computer. There are however functions like `mv` or `cp` which move and
copy files on our computer. A side effect occurs whenever a function creates or
changes files on our computer. These commands don't print any value if they
succeed.

We'll often write functions in order to calculate some value, and it's important
to understand how to store the result of a function in a variable so that it can
be used later. Let's `source` `addseq.sh` and run it one more time:


```bash
source addseq.sh
addseq 3 0 0 7
```

```
## 10
```

If we look back at the code for `addseq.sh` we can see that we created a
variable in the function called `sum`. When you create variables in functions
those variables become **globally accessible**, meaning that even after the
program is finished that variable retains its value in your shell. We can easily
verify this by `echo`ing the value of `sum`:


```bash
echo $sum
```

```
## 10
```

This is an example of one strategy we can use to retrieve values that a function
has calculated. Unfortunately this approach is problematic because it changes
the values of variables that we might be using in our shell. For example if we
were storing some other important value in a variable called `sum` we would
destroy that value by accident by running `addseq`. In order to avoid this
problem it's important that we use the `local` keyword when assigning variables
within a function. The `local` keyword ensures that variables outside of our
function are not overwritten by our function. Let's create a new version of
`addseq` called `addseq2` which uses `local` when assigning variables.

```
#!/usr/bin/env bash
# File: addseq2.sh

function addseq2 {
  local sum=0

  for element in $@
  do
    let sum=sum+$element
  done

  echo $sum
}
```

Now let's `source` both files so we demonstrate how `local` helps us avoid
overwriting variables.


```bash
source addseq.sh
source addseq2.sh
sum=4444
addseq 5 10 15 20
echo $sum
```

```
## 50
## 50
```

Our original `addseq` overwrites the value we assigned to `sum`. Now let's try
`addseq2`.


```bash
sum=4444
addseq2 5 10 15 20
echo $sum
```

```
## 50
## 4444
```

By using `local` within our function the value of `sum` is preserved! In order
to correctly capture the value of the result of `addseq2` we can use command
substitution.


```bash
my_sum=$(addseq2 5 10 15 20)
echo $my_sum
```

```
## 50
```

### Summary

- Functions start with the `function` keyword followed by the name of the
function and curly brackets (`{}`).
- Functions are small, reusable pieces of code that behave just like commands.
- You can use variables like `$1`, `$2`, and `$@` in order to provide arguments
to functions, just like a Bash script.
- Use the `source` command in order to read in a Bash script with function
definitions so that you can use your functions in your shell.
- Use the `local` keyword to prevent your function from creating or modifying
global variables.
- Be sure to `echo` the results of your function (if there are any) so that
they can be captured with command substitution.

### Exercises

Below this list of exercises you can find examples of how these programs should
work when used on the command line.

1. Write a function called `plier` which multiplies together a sequence of
numbers.
2. Write a function called `isiteven` that prints `1` if a number is even or
`0` a number is not even.
3. Write a function called `nevens` which prints the number of even numbers when
provided with a sequence of numbers. Use `isiteven` when writing this function.
4. Write a function called `howodd` which prints the percentage of odd numbers
in a sequence of numbers. Use `nevens` when writing this function.
5. Write a function called `fib` which prints the number of
[fibonacci](https://en.wikipedia.org/wiki/Fibonacci_number) numbers specified.


```bash
plier 7 2 3
```

```
## 42
```


```bash
isiteven 42
```

```
## 1
```


```bash
nevens 42 6 7 9 33
```

```
## 2
```


```bash
howodd 42 6 7 9 33
```

```
## .60
```


```bash
fib 4
```

```
## 0 1 1 2
```


```bash
fib 10
```

```
## 0 1 1 2 3 5 8 13 21 34
```

## Writing Programs

### The Unix Philosophy

Perhaps there are some design patters that you've been noticing since we started
talking about Unix tools, and now we're going to discuss them explicitly. Unix
tools were designed along a set of guidelines which are best summarized by
[Ken Thompson](https://en.wikipedia.org/wiki/Ken_Thompson)'s idea that each Unix
program should **do one thing well**. Following this rule when writing functions
and programs accomplished several goals:

- Limiting a program to only doing one thing reduces the length of the program,
and the shorter a program is the easier it is to fix if it contains bugs or if
it needs to be revised.
- Writing short programs also helps the users of your code
understand what's going on in your code in the event that they need to read your
code. Reading a poem induces a different cognitive load compared to reading a
novel.
- Folks who don't read the source code of your program (most users won't -
they shouldn't have to) will be able to understand the inputs, outputs, and side
effects of your program more easily.
- Using small programs to write a new program will increase the likelihood that
the new program will also be small. **Composability** is the concept of
stringing small programs together to create a new program.

The concept of composability in Unix is best illustrated by the use of the pipe
operator (`|`) for creating pipelines of programs.
When you're considering what inputs your program is going to have and what your
program is going to print to the console you should consider whether or not your
program might be used in a pipeline, and you should organize your program
accordingly.

In the previous section we discussed the difference between functions that
compute values and functions that produce side effects. You should notice that
the side effect functions like `mv` and `cp` do not print any text to the
console if they are successful. The concept of *quietness* is another important
part of the Unix philosophy. Quietness in this case means that a function should
not print to the console unless it is necessary, either to inform the user of a
value (`pwd`), to display the result of a computation (`bc`), or to warn the
user that an error has occurred.

### Making Programs Executable

Let's take a detailed look at some of the code files in our current working
directory:


```bash
ls -l | head -n 3
```

```
## -rw-rw-r-- 1 sean sean 138 Jun 26 12:51 addseq.sh
## -rw-rw-r-- 1 sean sean 146 Jun 26 14:45 addseq2.sh
## -rw-rw-r-- 1 sean sean 140 Jan 29 10:06 bigmath.sh
```

The left column of this table contains a series of individual characters and
dashes. The first hyphen (`-`) signifies that each of the entries in this list
are files. If any of them were directories then instead of a hyphen there would
be a `d`. Excluding the first hyphen we have the following string: `rw-rw-r--`.
This string reflects the **permissions** that are set up for this file. There
are three permissions that we can grant: the ability to **read** the file (`r`),
**write** to or edit the file (`w`), or **execute** the file (`x`) as a program.
These three permissions can be granted on three different levels of access which
correspond to each of the three sets of `rwx` in the permissions string: the
owner of the file, the group that the file belongs to, and everyone other than
the owner and the members of a group. Since you
created the file you are the owner of the file, and you can set the permissions
for files that you own using the `chmod` command.

The `chmod` command takes two arguments. The first argument is a string which
specifies how we're going to change permissions for a file, and the second
argument is the path to the file. The first argument has to be composed in a
very specific way. First we can specify which set of users we're going to change
permissions for:

|Character  |Meaning |
|:----------|:-------|
|`u` |The owner of the file |
|`g` |The group that the file belongs to |
|`o` |Everyone else  |
|`a` |Everyone above |

We then need to specify whether we're going to add, remove, or set the
permission:

|Character  |Meaning |
|:----------|:-------|
|`+` |Add permission |
|`-` |Remove permission |
|`=` |Set permission  |

Finally we specify what permission we're changing:

|Character  |Meaning |
|:----------|:-------|
|`r` |Read a file |
|`w` |Write to or edit a file |
|`x` |Execute a file  |

Let's use `echo` to write a very short program which we'll call `short`.


```bash
echo 'echo "a small program"' > short
```

Normally if we wanted to run `short` we would enter `bash short` into the
console. If we make this file executable we would only need to enter `short`
into the command line to run the program, just like a command!
Let's take a look at the permissions for `short`.


```bash
ls -l short
```

```
## -rw-r--r--  1 sean  staff  23 Jun 28 09:47 short
```

We want to make this file executable and we're the owner of this file since we
created it. This means we can combine `u`, `+`, and `x` in order make `short`
executable. Let's try it:


```bash
chmod u+x short
ls -l short
```

```
## -rwxr--r--  1 sean  staff  23 Jun 28 09:47 short
```

We successfully added the `x`! To run an executable file we need to specify the
path to the file, even if the path is in the current directory, meaning we need
to prepend `./` to `short`. Now let's try running the program.


```bash
./short
```

```
## a small program
```

Looks like it works! There is one small detail we should add to this program
though. Even though we've made our file executable, if we give our program to
somebody else they might be using a shell that doesn't know how to execute our
program. We need to indicate how the program should be run by adding
a special line of text to the beginning of our program called a **shebang**.
The shebang always begins with `#!` followed by the path to the program which
will execute the code in our file. The shebang for indicating that we want to
use Bash is `#!/usr/bin/env bash`, which we've been adding to the start of our
scripts for a while now! Let's rewrite this program to include the Bash shebang
and then let's run the program.


```bash
echo '#!/usr/bin/env bash' > short
echo 'echo "a small program"' >> short
```

```
## a small program
```

Now our Bash script is ready to go!

### Environmental Variables

We're one step away from being able to use our scripts and functions as shell
commands, but first we need to learn about environmental variables.
An environmental variable is a variable that Bash creates where data about your
current computing environment is stored. Environmental variable names use all
capitalized letters. Let's look at the values for some of these variables. The
`HOME` variable contains the path to our home directory, and the `PWD` variable
contains the path to our current directory.


```bash
echo $HOME
echo $PWD
```

```
## /Users/sean
## /Users/sean/Code
```

If we want one of our functions to be available always as a command then we need
to change the `PATH` variable. Let's take a look at this variable first.


```bash
echo $PATH
```

```
## /usr/local/bin:/usr/bin:/bin:/usr/local/git/bin
```

The `PATH` variable contains a sequence of paths on our computer separated by
colons. When the shell starts it searches these paths for executable files, and
then makes those executable commands available in our shell. One approach to
making our scripts available is to add a directory to the `PATH`. Bash scripts
in the directory that are executable can be used as commands. We need to modify
`PATH` every time we start a shell, so we can ammend our `~/.bash_profile` so
that our directory for executable scripts is always in the `PATH`. To modify an
environmental variable we need to use the `export` keyword.

First let's create a new directory called `Commands` in our `Code` directory
where we can keep our executable scripts. Then we'll add a line to our
`~/.bash_profile` so that `Commands` is added to the `PATH`.


```bash
mkdir Commands
nano ~/.bash_profile
```

```
alias docs='cd ~/Documents'
alias edbp='nano ~/.bash_profile'

export PATH=~/Code/Commands:$PATH
```

Save `~/.bash_profile` and close `nano`. Now let's `source` our Bash profile
(we only need to do this once) and move `short` into the `Commands` directory.
Then we should be able to use `short` as a command!


```bash
source ~/.bash_profile
short
```

```
## a small program
```

Looks like it works!

Alternatively to making individual scripts executable we can add a `source`
command to our `~/.bash_profile` so that we can use a Bash function on the
command line. Let's use `nano` to open up our `~/.bash_profile` again.


```bash
nano ~/.bash_profile
```

```
alias docs='cd ~/Documents'
alias edbp='nano ~/.bash_profile'

export PATH=~/Code/Commands:$PATH
source ~/Code/addseq2.sh
```

Save the `~/.bash_profile`, quit `nano`, and now let's `source` our
`~/.bash_profile` so we can test if we can use `addseq2`.


```bash
source ~/.bash_profile
addseq2 9 8 7
```

```
## 24
```

Again it works! If you have multiple Bash functions that you'd like to be able
to use on the command line then it's a good idea to define these functions in
one of a few files so that you don't have to `source` every individual function
that you want to have available.

### Summary

- According to the Unix Philosophy you should keep your programs short, simple,
and quiet.
- Use `chmod` to make your programs executable.
- You can modify your `~/.bash_profile` in order to make scripts and functions
available to use on the command line.
- Use `export` to change an environmental variable.

### Exercises

Below this list of exercises you can find examples of how the programs described
here should work when used on the command line.

1. Make a script executable.
2. Put that script in a directory that you create and make that directory part
of your `PATH`.
3. Write a program called `range` that takes one number as an argument and
prints all of the numbers between that number and 0.
4. Write a program called `extremes` which prints the maximum and minimum 
values of a sequence of numbers.


```bash
range 6
```

```
## 0 1 2 3 4 5 6
```


```bash
range -3
```

```
## -3 -2 -1 0
```


```bash
extremes 8 2 9 4 0 3
```

```
## 0 9
```
