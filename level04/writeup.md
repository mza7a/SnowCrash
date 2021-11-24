# SnowCrash Level04

In this level, we have a perl script that open port 4747 in localhost,
when sending a get request to that port with an 'x' attribute, it displays
the command echo 'x'. we also know that in order to run a command in echo
instead of writing it as text is by using echo '$(x)'. Then we only have to
send the x attribute equals to getflag, the command gets executed in the server
and it prints the flag.