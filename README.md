# Shell scripting cheat sheet

## Setting up script:

* Create a file: touch `file.sh`
* Make it executable: chmod 755 `file.sh`

* Path for scripts (folders which the shell looks for scripts):
  * See existing paths: echo $PATH
  * Add a path: export PATH=$PATH:`directory`


## Shell Scripting:

* **Alias**:

```bash
alias l='ls -l'
```

* **Functions**: 

```bash
today() {
	echo -n "Today's date is: "date +"%A, %B %-d, %Y"
}
```

* **Here Script**:

````
command <</<<- token
	content to be used as command's standard input
token

<< doesn't ignore tab.
<<- ignores tab.

````

`token` can be anything as long as it doesn't conflit with bash reserved word, `_EOF_` is usually used.

* **Variables**

```bash
var="This is a variable in string"
var=1
```

To show the content of the variable:
```bash
$var
```













