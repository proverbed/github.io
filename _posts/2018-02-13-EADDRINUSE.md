---
layout: post
title: Find process listening on port
---

**EADDRINUSE** means that the port number which listen() tries to bind the server to is already in use.

This might have happened because a process killed might not have been killed gracefully, many times this error is a consequence of not exiting a process gracefully. That means, a lot of people exit a node command (or any other) using **CTRL+Z**. The correct way of stopping a running process is issuing the **CTRL+C** command which performs a clean exit.


If we want to find the process ID (PID) of the process running on the given port (say 8888)

`lsof -i tcp:8888`

##### lsof
`lsof` - list open files 

`-i` -> specifying the -i option with `tcp:8888` allows us to filter on the TCP port 8888. 

this will return something like: 

`
COMMAND     PID   USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
node      10958 dmitri   13u  IPv6 0x83649da50921b2ff      0t0  TCP *:http-alt (LISTEN)
`


You might be tempted to just do a `KILL -9` on the above process ID. 

> Generally, you should use kill -15 before kill -9 to give the target process a chance to clean up after itself. (Processes can't catch or ignore SIGKILL, but they can and often do catch SIGTERM.) If you don't give the process a chance to finish what it's doing and clean up, it may leave corrupted files (or other state) around that it won't be able to understand once restarted.

So, as stated above its better to kill the above process with `KILL -15 {PID}`, so in the above case: `KILL -15 10958`

In conclusion, exiting a process the right way will free up that port while shutting down. This will allow you to restart the process without going through the trouble of killing it yourself before being able to re-run it again.
