# Writing Make Files
##### Linux Programming
---

You gotta say what to compile, which compiler (prolly gcc). 

```bash
CC=gcc 
CFLAGS=-Wall -ggdb -I . 

# Make command for removing a file simpler. 
RM=/bin/rm -f 
OBJS = prompt.o compute.o display.o 
# DEPS = prompt.h compute.h display.h 

.PHONY: tidy clean 

approxe: approxe.c ${OBJS}
	${CC} ${CFLAGS} ${OBJS} approxe.c -o approxe
	
prompt.o: prompt.c prompt.h 
	${CC} -c ${CFLAGS} prompt.c -o prompt.o 
	
compute.o: compute.c compute.h 
	${CC} -c ${CFLAGS} compute.c -o compute.o 
	
display.o: display.c display.h 
	${CC} -c ${CFLAGS} display.c -o display.o 

tidy: 
	${RM} *.o a.out core.* 
	
clean: 
	tidy ${RM} approxe
```