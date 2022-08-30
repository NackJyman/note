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

---

## Communicate Between Processes

### Interprocess Communication

Using pipes to shoot outputs to other processes. However we can create our own pipes. We will be doing the network version od interprocess communication. Using sockets, woo! 

#### Sockets

Transport layer artifact, a socket is bounf to a port. Options are the domain, the type, and the port numbers

- Domain could be **AF_INET** or **AF_UNIX**.
- THe type is normally one of **SOCK_STREAM** (TCP) or **SOCK_DGRAM** (UDP)

#### Ports

The port is an artifact at the network layer. They are the way that the single I/O connection to the network is multiplexed.

This way a large number of processes can communicate at (almost the same time).

There are three sets of ports:
- Well-known or System Ports, 0-1023
- User Ports, 1024-49151
- Dynamic and/or Private Ports, 49152-65535;

Basically do not use well-known (O/S stops this usually) or User ports (poor choice). 

#### Program Examples
Instant messaging called talk.
##### talkd

```c

```

Reading from the keyboard is not simple, the input is buffered. Data is not immediately transfered to the process.

Utilize the function **fgets()**, reads the input.

##### talkc

```c

```

Just needs a hostname to connect, (like web browser).
HTTP uses port 80.

When the port is "busy"/assigned-to-process, the program exits. Whenever this program does exit, it must close the socket. 

Use **send(2)** and **recv(2)**, do not use sendTo() these will block your processes until completion of task. When reading from a socket, read 1 character at a time.

###### Hostname

Accessing your current hostname.

Use function **gethostname(char *n, size_t l)**.
*l* is the length of the buffer, n
n must have space allocated.
...

##### _space of buff = 256 gang brr _

###### Creating a Socket
Type of stream defines semantics. The _Socket()_ function returns a file descriptor.

We have to bind/name the socket. Using:

```c
int bind(int sfd, const struct sockaddr addr, socklen_t addrlen)
```

_your ip needs to be in here correctly cause if not it sends return signal to wrong place_

This is what the socket looks like

```c 
struct sockaddr_in {
	sa_family_t  sin_family;
		/* address family: AF_INET */
	in_port_t    sin_port;
		/* port in network byte order*/
	...
}
```

###### Name Resultion
Use **gethostbyname()** and **gethostbyaddr()**.

###### Accepting Connections
**listen()** then **accept()**, which will return a new socket descriptor. Original socket is not affected.

The Client

We need to create another socket. **connect()** function. This takes 

network byte order

see **gai_strerror()** manual

---

