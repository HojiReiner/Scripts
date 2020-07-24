# Shell scripting cheat sheet

## Setting up script:

* Create a file: `touch file.sh`
* Write `!/bin/bash` in the top of the file
* Make it executable: `chmod 755 file.sh`

* Path for scripts (folders which the shell looks for scripts):
  * See existing paths: `echo $PATH`
  * Add a path: export `PATH=$PATH:directory`


## Shell Scripting:

### **Comments**

```bash
#This is a comment
```

### **Alias**:

```bash
alias l='ls -l'
```

### **Functions**: 

```bash
foo() {
	echo "This is a function"
}
```
Show the output of a function
```bash
echo $(foo)
```

### **Heredoc**:

````
command <</<<- token
	content to be used as command's standard input
token

<< doesn't ignore tab.
<<- ignores tab.

````

`token` can be anything as long as it doesn't conflit with bash reserved word, `_EOF_` is usually used.

### **Variables and constants**

```bash
var="This is a variable"
var=1
CONST="This is a constant"
```

To show the content of the variable:
```bash
$var
```
**Constants** behave like a variable, however they must be written in uppercase to indicate to programmers their different nature

### **Test**

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









