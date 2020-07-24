# Shell scripting cheat sheet

## Setting up script:

* Create a file: touch `file.sh`
* Make it executable: chmod 755 `file.sh`

* Path for scripts (folders which the shell looks for scripts):
  * See existing paths: echo $PATH
  * Add a path:export PATH=$PATH:`directory`


## Shell Scripting:

* Alias:

```shell
alias l='ls -l'
```

* Functions: 

```shell
today() {
    	echo -n "Today's date is: "
    	date +"%A, %B %-d, %Y"
}
```
