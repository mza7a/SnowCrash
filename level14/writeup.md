# SnowCrash Level14
After asking some friends, I found out that the only way to beat this level(until now) is by reversing getflag itself.
So shall we ?
```
(gdb) info functions
All defined functions:

Non-debugging symbols:
0x08048444  _init
0x08048490  strdup
0x08048490  strdup@plt
0x080484a0  __stack_chk_fail
0x080484a0  __stack_chk_fail@plt
0x080484b0  getuid
0x080484b0  getuid@plt
0x080484c0  fwrite
0x080484c0  fwrite@plt
0x080484d0  getenv
0x080484d0  getenv@plt
0x080484e0  puts
0x080484e0  puts@plt
0x080484f0  __gmon_start__
0x080484f0  __gmon_start__@plt
0x08048500  open
0x08048500  open@plt
0x08048510  __libc_start_main
0x08048510  __libc_start_main@plt
0x08048520  fputc
0x08048520  fputc@plt
0x08048530  fputs
0x08048530  fputs@plt
0x08048540  ptrace
0x08048540  ptrace@plt
0x08048550  _start
0x08048580  __do_global_dtors_aux
0x080485e0  frame_dummy
0x08048604  ft_des
0x0804871c  syscall_open
0x0804874c  syscall_gets
0x080487be  afterSubstr
0x08048843  isLib
0x08048946  main
0x08048ed0  __libc_csu_init
0x08048f40  __libc_csu_fini
0x08048f42  __i686.get_pc_thunk.bx
0x08048f50  __do_global_ctors_aux
0x08048f7c  _fini
```

Note that : ft_des
Lets disass the main.
```
	[...]
		0x08048dd2 <+1164>:	call   0x8048604 <ft_des>
   		0x08048dd7 <+1169>:	mov    DWORD PTR [esp+0x4],ebx
		0x08048ddb <+1173>:	mov    DWORD PTR [esp],eax
		0x08048dde <+1176>:	call   0x8048530 <fputs@plt>
		0x08048de3 <+1181>:	jmp    0x8048e2f <main+1257>
		0x08048de5 <+1183>:	mov    eax,ds:0x804b060
		0x08048dea <+1188>:	mov    ebx,eax
		0x08048dec <+1190>:	mov    DWORD PTR [esp],0x8049220
		0x08048df3 <+1197>:	call   0x8048604 <ft_des>
		0x08048df8 <+1202>:	mov    DWORD PTR [esp+0x4],ebx
		0x08048dfc <+1206>:	mov    DWORD PTR [esp],eax
		0x08048dff <+1209>:	call   0x8048530 <fputs@plt>
	[...]
```

It's like calling getflag for every level.
Lets disass the ft_des and make a breakpoint at the end to see the flag.
```
(gdb) disass main
blablabla
(gdb) b * 0x0804871b
Breakpoint 1 at 0x804871b
(gdb) run
Starting program: /bin/getflag
You should not reverse this
[Inferior 1 (process 2812) exited with code 01]
```

Okey smart, but not enough. Let's try to bypass a ptrace located at the beginning of the code.
What we going to do is simple :
Set a breakpoint before the ptrace call, and change eip to another address so he can jump.
The address will be the encrypted flag of the last ft_des(level14 the last one.)
I ran the program but I got a segfault.
I disass the ft_des function and then set a breakpoint on the ret.
and then I ran the program again and it worked like a charm.
The following is the segfault.
```
(gdb) b * 0x08048982
Breakpoint 1 at 0x8048982
(gdb) x/s 0x8049220
0x8049220:	 "g <t61:|4_|!@IF.-62FH&G~DCK/Ekrvvdwz?v|"
(gdb) run
Starting program: /bin/getflag

Breakpoint 1, 0x08048982 in main ()
(gdb) info registers
eax            0x0	0
ecx            0xbffff7a4	-1073743964
edx            0xbffff734	-1073744076
ebx            0xb7fd0ff4	-1208152076
esp            0xbffff5e0	0xbffff5e0
ebp            0xbffff708	0xbffff708
esi            0x0	0
edi            0x0	0
eip            0x8048982	0x8048982 <main+60>
eflags         0x200246	[ PF ZF IF ID ]
cs             0x73	115
ss             0x7b	123
ds             0x7b	123
es             0x7b	123
fs             0x0	0
gs             0x33	51
(gdb) set $eip=0x08048dec
(gdb) si
0x08048df3 in main ()
(gdb) si
0x08048604 in ft_des ()
(gdb) c
Continuing.

Program received signal SIGSEGV, Segmentation fault.
0xb7e911eb in fputs () from /lib/i386-linux-gnu/libc.so.6
```

Now for the actual working part :
```
(gdb) disass ft_des
Dump of assembler code for function ft_des:
[...]
End of assembler dump.
(gdb) b * 0x0804871b
Breakpoint 2 at 0x804871b
(gdb) run
The program being debugged has been started already.
Start it from the beginning? (y or n) y
Starting program: /bin/getflag

Breakpoint 1, 0x08048982 in main ()
(gdb) set $eip=0x08048dec
(gdb) si
0x08048df3 in main ()
(gdb) si
0x08048604 in ft_des ()
(gdb) si
Breakpoint 2, 0x0804871b in ft_des ()
(gdb) info register
eax            0x804c008	134529032
ecx            0xffffffd7	-41
edx            0x804c008	134529032
ebx            0xb7fd0ff4	-1208152076
esp            0xbffff5dc	0xbffff5dc
ebp            0xbffff708	0xbffff708
esi            0x0	0
edi            0x0	0
eip            0x804871b	0x804871b <ft_des+279>
eflags         0x200286	[ PF SF IF ID ]
cs             0x73	115
ss             0x7b	123
ds             0x7b	123
es             0x7b	123
fs             0x0	0
gs             0x33	51
(gdb) x/s 0x804c008
0x804c008:	 "7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ"
(gdb) quit
```