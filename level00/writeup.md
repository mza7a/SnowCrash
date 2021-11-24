# Snowcrash Level00

After booting the machine. The first thing I tried was a simple listing in the home:

```
ls -la
```

Nothing there. This will be harder than I thought xd.

I tried to read /etc/shadow after that but still no luck.
After that I tried to read /etc/passwd but nothing except a hash for level01.

Now lets go enumate the rest of the system.
As basic enumaration I tried looking for the O.S type, kernel, version...
Basic infos that might help not just in this level but the upcoming levels as well.
In the home directory of this repo you will find these basic infos.

Nothing really interesting.
Lets go ahead and try to find files that are under the flag00 user.

```
level00@SnowCrash:~$ find / -user flag00 2> /dev/null
/usr/sbin/john
/rofs/usr/sbin/john
```

At first I was like this is just another john the ripper tool.
But why it was created by flag00. If you already worked with johntheripper.
You will now that it requires sudo to run. Which means its made by the root.
Instead here we have the flag00 as the creator.
Cat that file :

```
level00@SnowCrash:~$ cat /usr/sbin/john
cdiiddwpgswtgt
```

Okey sneeky sneeky... Its obviously a rot.

```
cdiiddwpgswtgt
nottoohardhere
```

Easy I guess unlike what I said above.
```
level00@SnowCrash:~$ su flag00
Password:
Don't forget to launch getflag !
flag00@SnowCrash:~$ getflag
Check flag.Here is your token :
```

On to the next level.