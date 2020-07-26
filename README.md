# Shell scripting cheat sheet

## Setting up script:

* Create a file: `touch file.sh`
* Write `!/bin/bash` in the top of the file
* Make it executable: `chmod 755 file.sh`

* Path for scripts (folders which the shell looks for scripts):
  * See existing paths: `echo $PATH`
  * Add a path: export `PATH=$PATH:directory`


## Shell Scripting:

### Comments

```bash
#This is a comment
```

### Alias

```bash
alias l='ls -l'
```

### Functions 

```bash
foo() {
	echo "This is a function"
}
```
Show the output of a function
```bash
echo $(foo)
```

### Heredoc

````
command <</<<- token
	content to be used as command's standard input
token

<< doesn't ignore tab.
<<- ignores tab.

````

`token` can be anything as long as it doesn't conflit with bash reserved word, `_EOF_` is usually used.

### Variables and constants

```bash
var="This is a variable"
var=1
CONST="This is a constant"

Empty variables are also possible
var=
```

To show the content of the variable
```bash
$var
```
**Constants** behave like a variable, however they must be written in uppercase to indicate to programmers their different nature

### Test

* Commands are true if they return 0
* Commands are false if they return 1-255
````
 [me@linuxbox ~]$ ls -d /usr/bin
/usr/bin
[me@linuxbox ~]$ echo $?
0
[me@linuxbox ~]$ ls -d /bin/usr
ls: cannot access /bin/usr: No such file or directory
[me@linuxbox ~]$ echo $?
2 

---------------------------------------------------------

[me@linuxbox~]$ true
[me@linuxbox~]$ echo $?
0
[me@linuxbox~]$ false
[me@linuxbox~]$ echo $?
1
````

The **test** command returns 0 if the command inside is true and return 1 if it isn't.
````
test command

or

[command]
````
Here is a partial list of the conditions that **test** can evaluate (use `help test` to see all). 
Expression | Description
-----------|------------
-d file|True if file is a directory.
-e file|True if file exists.
-f file|True if file exists and is a regular file.
-L file|True if file is a symbolic link.
-r file|True if file is a file readable by you.
-w file|True if file is a file writable by you.
-x file|True if file is a file executable by you.
file1 -nt file2|True if file1 is newer than (according to modification time) file2
file1 -ot file2|True if file1 is older than file2
-z string|True if string is empty.
-n string|True if string is not empty.
string1 = string2|True if string1 equals string2.
 string1 != string2|True if string1 does not equal string2.

### If

**If** uses test to execute
```bash
# Alternate form

if [ -f .bash_profile ]
then
    echo "You have a .bash_profile. Things are fine."
else
    echo "Yikes! You have no .bash_profile!"
fi

# Another alternate form

if [ -f .bash_profile ]
then echo "You have a .bash_profile. Things are fine."
else echo "Yikes! You have no .bash_profile!"
fi     
```

* **;** is used to use more than 1 command in a line
* **Elif** can also be used

### Exit
Used to exit the script

```bash
Exit 0 #True
Exit 1 #False
Exit 2-255 #Helps to identify errors
```

### Testing for root

```bash
if [ $(id -u) = "0" ]; then
    echo "superuser"
fi

or

if [ $(id -u) != "0" ]; then
    echo "You must be the superuser to run this script" >&2
    exit 1
fi
```
* **>&2** Redirects the message to standart error (doesnt go in a file in case the scripts is writing)


### Read/Echo

```bash
echo -n "Enter some text > "
read text
echo "You entered: $text"
```
If a variable isn't used for read, it will use the environment variable REPLY.

* Use `help echo` and `help read` for command options

### Arithmetic
Shell is only capable of integer operations

```bash
first_num=0
second_num=0

echo -n "Enter the first number --> "
read first_num
echo -n "Enter the second number -> "
read second_num

echo "first number + second number = $((first_num + second_num))"
echo "first number - second number = $((first_num - second_num))"
echo "first number * second number = $((first_num * second_num))"
echo "first number / second number = $((first_num / second_num))"
echo "first number % second number = $((first_num % second_num))"
echo "first number raised to the power of the second number   = $((first_num ** second_num))"
```

### Case
````
case word in
    patterns ) commands ;;
esac
````
**case** selectively executes statements if word matches a pattern. You can have any number of patterns and statements. Patterns can be literal text or wildcards. You can have multiple patterns separated by the "|" character. 

```bash
echo -n "Type a digit or a letter > "
read character
case $character in
                                # Check for letters
    [[:lower:]] | [[:upper:]] ) echo "You typed the letter $character"
                                ;;

                                # Check for digits
    [0-9] )                     echo "You typed the digit $character"
                                ;;

                                # Check for anything else
    * )                         echo "You did not type a letter or a digit"
esac
```
`*` will match anything

### While and until
* Works when test is true
```bash
number=0
while [ "$number" -lt 10 ]; do
    echo "Number = $number"
    number=$((number + 1))
done
```

* Works when test is false
```bash
number=0
until [ "$number" -ge 10 ]; do
    echo "Number = $number"
    number=$((number + 1))
done
```

### Option Arguments
* `$#` indicates the number of arguments, from 0 to 9,
0 is the program and 1-9 are the options.
* `shift` reduces the parameters value down by one

```bash
echo "You start with $# positional parameters"

# Loop until all parameters are used up
while [ "$1" != "" ]; do
    echo "Parameter 1 equals $1"
    echo "You now have $# positional parameters"

    # Shift all the parameters down by one
    shift

done
```

Ex: command -1 -2 -3
It has 3 positional parameters (-1, -2, -3)

###For
```bash
for variable in words; do
    commands
done

Example1:
for i in word1 word2 word3; do
    echo $i
done

Example2:
for i in "$@"; do
    echo $i
done
```
* `$@` contains all the positional parameters


## Debugging and errors
* See what the script is doing
```bash

#!/bin/bash -x
.
.
.

or 

code
set -x
possible faulty code
set +x
code
```

* Careful with empty variables

```bash
num=

Error:
if [ $num = '1' ]...

False:
if [ '$num' = '1']...

```
