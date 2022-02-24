# Utilities
##### Linux Programming
---

There are way more utilities than the ones mentioned here, but these are the ones listed throughout the slides of the course.

Four basic groups
-	Process Control
-	File Control and Management
-	Text Processing
-	Miscellaneous

###### _Man & Info_
_Man_ displays the "manual pages," _Info_ displays documentation as well.

##### Process Control Utils
###### _Kill_
Can only signal processes you control. If no signal is specified, the SIGTERM(15) signal is sent. Which can be ignored, so instead you could use SIGKILL(9) like 
```bash
kill -9 22345
```
_For more info on signals, examine "man signal.h" or "man 7 signal"_

###### _ps_
_ps_ simply lists the processes running in a system and provides some status about them. Gives running time, % of CPU usage, current memory size, and ownership information.

###### _pgrep_ (process grep)

This lists processes that match some regular expression. Only provides PID and optionally the name. _Similar to pkill._

###### _top_

Shows the top 20 processes. Good for when PC starts slowing down.

##### Text Utilities
###### _wc_ 
Simply a word count. Prints the number of newlines, number of words and numbers of characters on the input. If there are multiple files, then it will print a total over all the files.

###### _head_
Prints the __first__ lines of it's input, default is 10, however this could be changed with a flag

```bash
head -20 filename
```

###### _tail_
Prints the __last__ 10 lines of the file. Number of lines can be changed the same way with head.

###### _split_
Breaks the input into equal size files. Provide a prefix and it will add "aa" to the first, "ab" to the second and so on.

###### _sort_

Used to sort text files, line by line. Default is oeprate on entire line. Can specify fields, columns, field separator, sort order, etc. Helpful with lists of names.

###### _uniq_
Use on input to delete duplicate adjacent lines. Input does not have to be sorted, but only works on adjacent lines. Equivalent to "_sort -u_"
###### _cut_

Operate on fields, returns some subset of the fields on each line of inpute, can specify fields, field delimiters, more, Only useful if inpit is in some fixed format.

###### _tr_
Translate characters. Very useful to change all uppercase letters to lower or vice versa. Can Modify special characters like tabs. Give it two sets, characters to match and replacements. Always reads from standard input and writes to standard output.

##### File Control and Management

###### _ls_
List contents of some directory, many options ls -la shows every file and in long format.

###### _chmod_
Change mode (permissions), This is very important tool for administration. This changes or alters the file permissions, which are associated with three sets of people, Owner, group and others. Displayed as -rw-rw-rw-. Note that the left most position is a special indicator that could display 
- _d_ for directory
- _l_ for link
- _b_ for block special
- _c_ for character special

###### _rm_
"Remove" very important for general use of a PC. Remove or "unlink" a component. There is no 'undo.' Note that _rm_ does NOT remove directories unless using "rm -rf" which recursively removes all the components in a directory, very dangerous.

###### _cp_

Copies a file from one place to another. Use '-i' for interactive option, which prompts before overwriting an existing default. Works with a directory

```bash
cp source_file destination_file
```

###### _mv_
Moves a file, same exact format as _cp_, but does NOT create a copy.

###### _ln_
Create a link to an existing file. Two types of links, a soft link and a hard link. Soft links are safer than hard links because if you remove a hard link you remove the file as well.

###### _touch_
Creates a file wherever the user is.

###### _mkdir_
Creates a directory for the user whereever they are. '-p' will create a directory tree if needed.

###### _rmdir_

This simply removes a directory, it cannot remove a non-empty directory

###### _pwd_
Print working directory.

##### Miscellaneous Utilities

###### _uptime_
Prints how long the system has been running. Along with current time and number of users. And load averages of the last 1, 5, and 15 minutes.
See also _w_

###### _uname_
Has nothing to do with users, but software and hardware information

```bash
uname -a
```
Will provide the user with the kernel name, hostname, kernel version, hardware name, processor type, hardware platform, O/S.

###### _hostname_
Returns the name of the computer you are logged into, Can give you a fully qualified name and IP address. Used if a script needs to run on all machines in an organization.

###### _which_
This prints the full path of a shell command to standard out. Great for troubleshooting.

###### _df_
Displays filesystems.

###### _du_
This is disk usage, displays how much space you (or someone else) is using.

###### _file_
An odd utility that determines the type of a file. performs three checks.
- filesystem checks _(stat(2))_, determines if it exists and if it is a special device.
- Magic checks, looks at the beginning of the file for a magic number. 
- Language checks, look and see what is in hte file.

---