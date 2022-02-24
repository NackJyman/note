# Linux Programming
## misc stuff
---
##### Feburary 15, 2022
###### On Make FIles
"Cute will cost"

```ad-abstract
#### $*
###### The stem of the target in implicit...

```

### Pattern Rules
```
%.o : %.c
	${CC} -c ${CFLAGS} ${CPPFLAGS} $<
	
%.tab.c %tab.h: %.y
	bison -d $< 

% : %.c %.h
	${CC} -c ${CFLAGS} $< -o $@
```

### Rule Errors

You can test for errors by running "make -n" because it simulates running so you can see if it has errors in it

### Print out
- Requires stdio.h
	- ... ... ...
- Requres ...

#### -v

The "v" versions do not use individual arguements after the format string

Look here:
```sh
man printf(3)

printf("__", x, y, z ...);
```

"-Wall" will check the format against the arguments

You gotta read the man page.

### Numerical Values

Integer format is "d"
"uxXo" takes an unsigned int and print it as an unsigned int, ...

length modifiers for short (int), long, and long long.

Then there is width and precision. That is minimum field width

"c" for a single charcter

### Return Values

Negative output means error.

HW4 DOES NOT USE PRINTF
Slow when compared to "fwrite()" Do not use printf() for the *wycat* assignment.