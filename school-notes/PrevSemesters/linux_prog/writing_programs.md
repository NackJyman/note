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

wyls: wyls.c
	${CC} ${CFLAGS} wyls.c -o wyls

tidy:
	${RM} *.o a.out core.*
```

### wyls.h
```c
/*
* wyls.h
* Author: Jack Nyman
* Date: March 8, 2022
*
* COSC 3750, Homework 5
*
* This is a simple version of the ls utility.
*/
#ifndef WYLS_H_
#define WYLS_H_

#include <dirent.h>
#include <errno.h>
#include <grp.h>
#include <limits.h>
#include <math.h>
#include <pwd.h>
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/stat.h>
#include <sys/types.h>
#include <time.h>
#include <unistd.h>

string humanBytes(int bytes);
string time(int path);
string permissions(int octal);
string owner(string path);
string ownerId(string path);

string lsDir(string path);
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
#include "wycat.h"

int main(int argc, char **argv) {
  // Initialize things
  const BUFF_SIZE = 256;
  bool human = false;
  bool uid_gid = false;
  bool current_dir = false;

  if(argc == 1) { // No flags or arguments
    bool current_dir = true;
  }

  // Check the supplied options
  for(int i = 1; i < argc && argv[i][0] == '-'; i++) {
    for(int j = 1; argv[i][j]; j++) {
      char option = argv[i][j];
      switch(option) {
        case 'n':
          uid_gid = true;
          break;
        case 'h':
          human = true;
          break;
        default:
          perror("wyls: Unsupported option.");
          return 0;
      }
    }
    if(i == (argc-1)) { // If the last argument supplied is an option
      current_dir = true;
    }
  }
  // Check arguemnts
  char* userGroup, perms, size, timeMod;
  int bytes, octal;
  if(current_dir) { // NO ARGUMENTS, RUN IN CURRENT DIR
    char dir[BUFF_SIZE];
    getcwd(BUFF_SIZE, dir);
    DIR* dr = opendir(dir);
    if(!dr) {
      perror("wyls: Could not open direcory.");
    }
    else {
      struct dirent* entry = NULL;
      while((entry = readdir(dr))) {
        // Get Permissions, convert to human
        perms = permissions(entry);
        // Get username and groupname, or uid and gid
        if(uid_gid) {
          userGroup = ownerId(entry);
        }
        else {
          userGroup = owner(entry);
        }
        // Get size of item in bytes, or human bytes
        if(human) {
          size = humanBytes(bytes);
        }
        else {
          size = bytes;
        }
        // Get the time
        timeMod = getTime(entry);
        // Print permissions, owner/uid, group/gid, size, time, item
        printf("%s %s %s %s %s %s", perms, userGroup, size, timeMod, entry);
      }
    }
    closedir(dr);
    return 0;
  }
  else { // ITERATE THROUGH THE ARGUMENTS
    for(int i = 1; i < argc && argv[i][0] != '-'; i++) {
      char* directory = argv[i];
      DIR* dr = opendir(directory);
      if(!dr) {
        perror("wyls: Could not open direcory.");
      }
      else{
        struct dirent* entry = NULL;
        while((entry = readdir(dr))) {
          // Get Permissions, convert to human
          char* perms = permissions(octal);
          // Get username and groupname, or uid and gid
          if(uid_gid) {
            char* userGroup = ownerId(entry);
          }
          else {
            char* userGroup = owner(entry);
          }
          // Get size of item in bytes, or human bytes
          if(human) {
            char* size = humanBytes(bytes);
          }
          else {
            int size = bytes;
          }
          // Get the time
          char* timeMod = getTime(entry);
          // Print permissions, owner/uid, group/gid, size, time, item
          printf("%s %s %s %s %s %s", perms, userGroup, size, timeMod, entry);
        }
      }
      closedir(dr);
    }
  }
  return 0;
}

char* humanBytes(int bytes) {
  return "69K";
}

char* getTime(struct dirent* entry) {
  return "Sep 19 2000";
}

char* permissons(int octal) {
  return "-rw-rw-rw-";
}

char* owner(struct dirent* entry) {
  return "Jack Administrator";
}

char* ownerId(struct dirent* entry) {
    return "Jack Administrator";
}
```
