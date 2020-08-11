# Young Blaze
You are provided with a single file called Damn. You can run 'file' command on it to figure out what it is.

```sh
$ file Damn
Damn: ELF 32-bit LSB relocatable, Intel 80386, version 1 (SYSV), not stripped
```

As you can see, this is not an executable file but a relocatable file. So, the source was compiled without linking and produced an object file.

In order to make it executable, you need to link it using some linker.

# Method #1
Using 'ld' (The GNU linker). Since this is a x32 object file you need to use 32-bit architecture while linking.

```sh
$ ld -m elf_i386 -s -o out Damn
$ cat out
Poseidon{L1nK_tH4T_$h17_im_0u7}
```

# Method #2
Using 'gcc' (The GNU C compiler). Since this is a x32 object file you need to use 32-bit architecture while linking. This object file already has essential startup functions defined so use '-nostartfiles' option. This method produces some linker warning but nonetheless produces the executable file.

```code
-nostartfiles

Do not use the standard system startup files when linking. The standard system libraries are used normally, unless -nostdlib or -nodefaultlibs is used. 
```

```sh
$ gcc -o out Damn -m32 -nostartfiles
$ cat out
Poseidon{L1nK_tH4T_$h17_im_0u7}
```

# Yolo Method #3
Running 'strings' command on the file reveals interesting string at the beggining.
```sh
$ strings Damn
Ezfp|qz{nY${^Ja]!AJ1}$"J|xJ%`"h
.text:
.bss
.data
.shstrtab
.symtab
.strtab
.rel.text:
tiwini3ada!?
_start
music
_exit
result
secret
secret_len
```

Maybe it's the flag :)
It's worth the shot to paste it in Magic decoder at [CyberChef](http://icyberchef.com/#recipe=Magic(3,true,false,''))

Analysis revealed that the falg is XOR encrypted with **HEX** key of value **15**

# Flag
**Poseidon{L1nK_tH4T_$h17_im_0u7}**

Easy challenge but you can learn about different ELF formats and about different stages of compilation :)
