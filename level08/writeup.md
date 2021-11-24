# SnowCrash Level08

Another binary, this time we have a token with it.
Trying to read the token gave us permission denied.

GDB TIME...
Well this one was so long but... hold on.
We can use strings in order to simplify it.
We get that the binary tries to read a file. We give it token but it can't due to permissions.
So why not a symbolic link to trick the program.

```
level08@SnowCrash:~$ ln -s /home/user/level08/token /tmp/temp_tok
level08@SnowCrash:~$ ./level08 /tmp/temp_tok
quif5eloekouj29ke0vouxean
```

and now lets get out flag.
```
level08@SnowCrash:~$ su flag08
Password:
Don't forget to launch getflag !
flag08@SnowCrash:~$ getflag
Check flag.Here is your token : 
```