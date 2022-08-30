# Waiting 
###### Linux Programming
---

#### Process' Memory

If you create a child process within your program and terminate the parent process prior to the the terminiation of the child process, _init_ will inherit the child process.

If a child process does not become "waited," they will become zombies. (Defunct, terminated but not reaped by it's parent.)

#### _wait_ Functions
##### _wait()_

##### _waitid()_

##### _waitpid()_

###### Obsolete Versions:

##### _wait3()_

##### _wait4()_

---
Every process is tracked by the operating system. 


##### Bucker recommends
```c
waitpid(-1, &status, WNOHANG);
```

This function will return immediately if no child has exited.

###### He wrote this on the board
```c
if(nowait) {
	while (rtn = waitpid(-1, &status, WNOHANG)) > 0);
}
else {
	wait(chnle); // dont know what he wrote
} // read the man page 
```
---

### Signals

These are a form of interprocess communication. They are asynchronous. They are pretty bad and stuff.

---
## Exam 2 is comming lol ur fuked

Everything from before this week to the last exam. Will ask questions about the C library functions covered within lecture slides.

Questions about return values and error indications. Common questions on _gcc, cpp,_ and _make_. 1 or 2 sentences, maybe 3. 

- Tar

Lecture 13 maybe.

- Scheduling processes

Possible states, PCB, Short-term scheduler. Preemptive scheduling, compare throughput, turnaround time, waiting time, response time.

- Que

FIFO, SJF, priority. 

- Memory Management

Address binding, physical cersus logical.

Swapping, Paging, TLB.

- Communications

Pipes, TCP/UDP, RTT, Sockets (Know the general things inside a header).

$$Thursday, April \ 7^{th} \ 8am - 10am$$

---
