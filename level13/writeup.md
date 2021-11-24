# SnowCrash Leve13

Let's launch that binary:
```
level13@SnowCrash:~$ ./level13
UID 2013 started us but we we expect 4242
```

At first I wanted to look for a way to change the UID. Lukcily I watched a video of liveoverflow before.
Its like my first cracked program.
Let's open gdb :
```
(gdb) info functions
All defined functions:

Non-debugging symbols:
0x08048314  _init
0x08048360  printf
0x08048360  printf@plt
0x08048370  strdup
0x08048370  strdup@plt
0x08048380  getuid
0x08048380  getuid@plt
0x08048390  __gmon_start__
0x08048390  __gmon_start__@plt
0x080483a0  exit
0x080483a0  exit@plt
0x080483b0  __libc_start_main
0x080483b0  __libc_start_main@plt
0x080483c0  _start
0x080483f0  __do_global_dtors_aux
0x08048450  frame_dummy
0x08048474  ft_des
0x0804858c  main
0x080485f0  __libc_csu_init
0x08048660  __libc_csu_fini
0x08048662  __i686.get_pc_thunk.bx
0x08048670  __do_global_ctors_aux
0x0804869c  _fini
```

Nothing really important here.
Let's disassemble the main :
```
(gdb) disass main
Dump of assembler code for function main:
   0x0804858c <+0>:	push   ebp
   0x0804858d <+1>:	mov    ebp,esp
   0x0804858f <+3>:	and    esp,0xfffffff0
   0x08048592 <+6>:	sub    esp,0x10
   0x08048595 <+9>:	call   0x8048380 <getuid@plt>
   0x0804859a <+14>:	cmp    eax,0x1092
   0x0804859f <+19>:	je     0x80485cb <main+63>
   0x080485a1 <+21>:	call   0x8048380 <getuid@plt>
   0x080485a6 <+26>:	mov    edx,0x80486c8
   0x080485ab <+31>:	mov    DWORD PTR [esp+0x8],0x1092
   0x080485b3 <+39>:	mov    DWORD PTR [esp+0x4],eax
   0x080485b7 <+43>:	mov    DWORD PTR [esp],edx
   0x080485ba <+46>:	call   0x8048360 <printf@plt>
   0x080485bf <+51>:	mov    DWORD PTR [esp],0x1
   0x080485c6 <+58>:	call   0x80483a0 <exit@plt>
   0x080485cb <+63>:	mov    DWORD PTR [esp],0x80486ef
   0x080485d2 <+70>:	call   0x8048474 <ft_des>
   0x080485d7 <+75>:	mov    edx,0x8048709
   0x080485dc <+80>:	mov    DWORD PTR [esp+0x4],eax
   0x080485e0 <+84>:	mov    DWORD PTR [esp],edx
   0x080485e3 <+87>:	call   0x8048360 <printf@plt>
   0x080485e8 <+92>:	leave
   0x080485e9 <+93>:	ret
End of assembler dump.
```

Now on this line :
```
	0x08048595 <+9>:	call   0x8048380 <getuid@plt>
	0x0804859a <+14>:	cmp    eax,0x1092
	0x0804859f <+19>:	je     0x80485cb <main+63>
```

It calls getuid to get the current uid, then it compare it with `0x1092` printing it in decimal is : 4242
All we have to do is set a breakpoint on that line, change the $eax in registers and then we'll have our results.
```
(gdb) b * 0x0804859a
Breakpoint 1 at 0x804859a
(gdb) run
Starting program: /home/user/level13/level13

Breakpoint 1, 0x0804859a in main ()
(gdb) info registers
eax            0x7dd	2013
ecx            0xbffff784	-1073743996
edx            0xbffff714	-1073744108
ebx            0xb7fd0ff4	-1208152076
esp            0xbffff6d0	0xbffff6d0
ebp            0xbffff6e8	0xbffff6e8
esi            0x0	0
edi            0x0	0
eip            0x804859a	0x804859a <main+14>
eflags         0x200246	[ PF ZF IF ID ]
cs             0x73	115
ss             0x7b	123
ds             0x7b	123
es             0x7b	123
fs             0x0	0
gs             0x33	51
(gdb) set $eax=4242
(gdb) c
Continuing.
your token is 
[Inferior 1 (process 2726) exited with code 050]
```
ez.