# Writing Programs (like real ones)
###### Linux Programming
---

This module is the culmination of all the programs written for this course. So far we have implemented _cat_ and _ls_.

_As of 3/6/2022, Neither have been graded._

## wycat
###### _(HW 4)_

This program is a simplified version of the __cat__ function in all linux distros.

### Makefile
```bash
# Makefile
# Author: Jack Nyman
# Date: Feb 22, 2022
#
# COSC 3750
#
# Makefile for wycat.c

CC=gcc
CFLAGS= -Wall -ggdb -I .

# Command to remove files.
RM=/bin/rm -f

.PHONY: tidy clean

wycat: wycat.c
	${CC} ${CFLAGS} wycat.c -o wycat

tidy:
	${RM} *.o a.out core.*
```

### wycat.c

```c
/*
* wycat.c
* Author: Jack Nyman
* Date: Feb 22, 2022
*
* COSC 3750, Homework 4
*
* This is a simple version of the cat utility.
*/
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main(int argc, char **argv) {
  char buffer[4096];
  FILE *fp;
  if(argc == 1) { // No inputs, read from stdin
    size_t numBytesRead = fread(&buffer, sizeof(*buffer), 4096, stdin);
    while(numBytesRead > 0) {
      fwrite(&buffer, sizeof(*buffer), numBytesRead, stdout);
      numBytesRead = fread(&buffer, sizeof(*buffer), 4096, stdin);
    }
    return 0;
  }
  else {
    for(int i = 1; i < argc; i++) {
      char* file = argv[i];
      if (strcmp(file, "-") == 0) { // Read from standard input
        int numBytesRead = fread(&buffer, sizeof(*buffer), 4096, stdin);
        while(numBytesRead > 0) {
          fwrite(&buffer, sizeof(*buffer), numBytesRead, stdout);
          numBytesRead = fread(&buffer, sizeof(*buffer), 4096, stdin);
        }
      }
      else {
        fp = fopen(file, "r");
        if (!fp) {
          perror("Error: Could not open file.");
        }
        else {
          int numBytesRead = fread(&buffer, sizeof(*buffer), 4096, fp);
          while (numBytesRead > 0) {
            fwrite(&buffer, sizeof(*buffer), numBytesRead, stdout);
            numBytesRead = fread(&buffer, sizeof(*buffer), 4096, fp);
          }
        }
        fclose(fp);
      }
    }
  }
  return 0;
}
```

---

## wyls
###### _(HW 5)_
#### Overview
_wyls_ will utilize argv and argc.
The accepted inputs can be pathname's and/or option.
If a supplied argument (directory) is not accessible, the program will use perror() to print a message and continue normally. If a option is not supported then an error message will be printed and the program will end.

##### Options
Options will appear before the filenames. like so,
```bash
$> wyls -n dir1 -h
```

'-h' can treated as a pathname, but that will not be an acceptable arguement. So you will use perror to print something like:
```bash
"-h": file not found
```

You can also lump options together like so:
```bash
$> wyls -nh dir1
```

##### Arguments
If there are no arguments (like a directory), _wyls_ will default to the current directory.
Remember to ignore '.' and '..'.
See _getcwd()_

The output will be similar to "ls -l"

Example output:
```bash
total 72  
-rw-------  1  kbuckner faculty 156     Sep 14 10:25 class_list.txt
drwx------  4  kbuckner faculty 4096    Sep 14 20:46 hw01/  
drwx------  15 kbuckner faculty 4096    Sep 21 21:19 hw02/  
drwx------  14 kbuckner faculty 4096    Sep 28 08:44 hw03/  
drwx------  3  kbuckner faculty 4096    Sep 30 11:27 hw04/  
drwx------  2  kbuckner faculty 4096    Sep 30 11:28 hw05/  
drwx------  2  kbuckner faculty 4096    Aug 28 2014 module01/  
drwx------  2  kbuckner faculty 4096    Sep 18 13:24 module2/  
-rw-------  1  kbuckner faculty 854     Sep 18 13:29 notes  
drwx------  3  kbuckner faculty 4096    Jun 2 08:48 old_hw03/  
drwx------  4  kbuckner faculty 4096    Jun 2 08:50 old_hw04/  
-rw-r--r--. 1  root     root    4953680 May 2 2014 /usr/share/dict/linux.words
```
Format needs to be neat, see _printf(3)_ for more information.

This version will ignore the line (first one) that contains number of blocks on disk. We will also ignore the second column containing number of hard links.

Print permissions in "rwx" format. first character is a _d_ for directory, and _l_ for a link, _'-'_ for anything else.

size will be in bytes unless '_-h_' is used. (Floating pint number with at most 1 digit to the right of the decimal, ignore if 0).
Use **K** for kilobytes, __M__ for megabytes, __G__ for gigabytes.

Time will be __mtime__, time last modified.
Use _localtime()_ and _strftime()_.
If modified in the last 180 days, do not print the year.

If '-n' is used, the program will print using _uid_ and _gid_, instead of username and group name.
See _getpwnam()_ and _getgrnam()_ functions.

Tar the final files
- _wyls.c_
- _hw5_pcode.txt_
- _Makefile_

---

### Makefile

```bash
# Makefile
# Author: Jack Nyman
# Date: March 8, 2022
#
# COSC 3750
#
# Makefile for wyls.c

CC=gcc
CFLAGS= -Wall -ggdb -I .

# Command to remove files.
RM=/bin/rm -f

.PHONY: tidy clean

wycat: wyls.c
	${CC} ${CFLAGS} wyls.c -o wyls

tidy:
	${RM} *.o a.out core.*
```

### wyls.c

```c
/*
* wyls.c
* Author: Jack Nyman
* Date: March 8, 2022
*
* COSC 3750, Homework 5
*
* This is a simple version of the ls utility.
*/
#include <stdio.h>
#include <string.h>
#include <errno.h>

int main(int argc, char **argv) {
  return 0;
}
```
