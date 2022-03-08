# Writing Shell Scripts
##### Linux Programming

---
#shell #script #utility #linux

Writing shell scripts lets you create your own utilities and functions for manipulating the stuff on your computer.

'#' is used as a comment character.

#### _pound-bang_

A pound bang should always be the at the left margin and the first line of the file.
```bash
#!/bin/sh
```

---
After the first line, you can type in commands, standard system utilites, or programs you have written, shell functions and shell syntax.

#### Variables
There are no data types, there are only strings. Declaration and assignment are done at the same time.

When refering to a named variable you need to place a '$' sign in front of it to call it correctly.

```bash
if [ $x == "bill" ]; then
```

##### Borne Shell Arguements
For the Bourne shell (_sh_), arguements are readily available inside the script.
- "$1" returns the first arguement, "$2" the second, and so on, through "$9."
- Entire command line is "$*"
- Number of arguments is "$#"
- "$0" is the name used to invoke the script.

##### _Shift_

Shift is a special command that manipulates the arguements provided. Shift will move $2 to $1 and $3 to $2 and so on. The original $1 is lost. You can _shift n_ where you can shift _n_ times.

#### Quotation Marks

There are often problems with strings with spaces or special characters. Using quotiation marks around a string that shell treat it as a single word.
- Double quotation (quote) marks allow variables to be expanded within the quotes.
- Single quotation (apostrophe or tick) marks remove all special meaning from characters like the dollar sign.

##### Back quotes (backtick)
Non interchaneable with other types of quotation marks. Everything within the back quotes is treated as a command. It is executed in a subshell and the standard output is substituted within the current script as a string.

---
### General Syntax

$\underline{Everything}$ must be delimited with whitespace.

```bash
if[-z "$x"]; then
```
will procude a syntax error.
```bash
if [ -z "$x" ]; then
```
This form is correct.
##### Flow Control Constructs
The most common are _if, for,_ and _case_. There are others such as _while_ and _until_ but are not super common.

###### _if_
Follows the genral syntax of:
```bash
if list; then
	list
fi
```


### Loop Syntax
This is taken from HW2. The following code iterates through each argument supplied when the function was called. Each if statement checks for something else, first if its a directory, then executable, then if its an unrecognizable filetype. In the hw there is else statement at the end to perform various operations on the file now that we made sure that it is a ASCII file.

```bash
for ARGUEMENT in $*; do
    IS_ASCII="$(file -b "$ARGUEMENT" | sed '/ASCII text/!d')"
    if [ -d "$ARGUEMENT" ]; then
        printf "modify: $ARGUEMENT, is a directory.\n"
        continue
    fi
    if [ -x "$ARGUEMENT" ]; then
        printf "modify: $ARGUEMENT, cannot edit executables.\n"
        continue
    fi
    if [ ! "$IS_ASCII" ]; then
        printf "modify: $ARGUEMENT, filetype not supported.\n"
        continue
    fi
done
```
