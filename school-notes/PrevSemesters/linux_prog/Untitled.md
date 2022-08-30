# Fork, Exec, and Parsing
##### Linux Programming
###### March 31, 2022
---

### Fork
```c
#include<unistd.h>

pid_t fork(void)
```

Creates an (almost) exact duplicate of the calling process. This includes any open files or directories (opendir).

The new process has its own pid.

#### Return Vals

On **Success** fork(2) returns the child's pid to the parent.
On **success** it returns 0 to the child.

If it returns -1 there was an **error**, sets errno.

#### Problems

In general, this is a simple and safe function. However when you do not chekcing the return value can lead to a "fork bomb." Account will be blocked if this happens.

##### How 
Remember to check the return value. If there is a error do not try again.
Use _getpid()_ to get process id. Get parent process id with _getppid()_.

### Exec Functions
There are seven six are front ends for the "real" one.
_execl, execlp, execle, execv, execvp, execvpe_
Are front ends.
The _real_ one is execve().

```c
int execve(const char *fname,char *const ...);
```

#### Operation
These functions overwrite the colling process with some new process. The text, data, bss, and stack of the callig process are overwritten.

...

#### Return Value

Only return value possible is -1.
_errno_ is set to indicate the problem.
Otherwise, the process calling __exec__ no longer exists.

###### _execl()_
Takes a list of arguements.
```c
execl("program", "program", "..", "..", NULL);
```

###### _execv()_
Takes a list of vectors.

```c
char** args;
execl("program", args);
```

###### _????p()_
Takes a "path" got executable like the shell.

###### _????e()_
last argument a vector for the environment cariables for the new process.

###### _execl()_

```c
int execl(const char *path, const char *arg, ..)
```
---

### Parsing
```c
prog a b | c | d >& e
```

#### Parsing Input

##### _strtok()_
Takes two arguments, a string that will be tokenized and another that supplies the delimiter(s) on which the token string is serpeated. Designed to be called several times.

```c
char names[] = {"John Smith; Jack; bill;"};

char* nptr;
nptr = strtok(names, ";");
printf(nptr); // Prints "John Smith"
while(nptr != NULL) {
	nptr = strtok(NULL, ";");
	printf(nptr);
}
```

Replaces the first semicolon with a '\0' Will fail if there is not a semicolon at the end of the names array.

#### Other Functions
There are other functions to name things within string:

- strchm();
- strpbrk();
- strstr();
- strsep();

slide 26

###### flex or lex
Lexical 

##### Reading the input.
Already seen two basic versions. _fgets()/getline()_ which reads a string from standard input up to a newline. The problem you may have with this.
```c
```

###### _fgetc()_
Oldie but a goodie, bought by IBM.

###### _scanf()_
...

### Homework 8
#### Wyshell

```c
int parse_line(const);

---

	
	
```

---