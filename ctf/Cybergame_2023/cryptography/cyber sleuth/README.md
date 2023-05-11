# Cyber Sleuth
## Description
The alleged boss of a ransomware gang had his computer seized in a recent raid. Your job is to help the analysts in their analysis.

## Task #1: Comm App
It turned out that together with other members of the gang, the attackers use an application for communication, which can be logged in by sending a secret password. Unfortunately, the password of the day was sent to the boss in encrypted form and can only be decrypted using his private key. However, we only know the public key and the encrypted content. Is it possible to get to the password of the day? https://drive.google.com/file/d/1x4ryaI2KKD6S6v3KcFDL1p76HQ6qjd3h/view?usp=share_link

Ok so we are dealing with RSA cryptography here.
I use https://github.com/RsaCtfTool/RsaCtfTool.

We have pub key and ciphertext. Pub key is small so it will crack easy. For some reason `--uncipherfile` does not work for me so I just paste the ciphertext.

```
$ ~/RsaCtfTool/RsaCtfTool.py --publickey Public.key --uncipher 13756672540135655601041982915088134976849597290712205192986024546245110557997926972456590345764037613691530654259468541003355626906
private argument is not set, the private key will not be displayed, even if recovered.

[*] Testing key Public.key.
attack initialized...
attack initialized...
[*] Performing smallq attack on Public.key.
[+] Time elapsed: 0.3959 sec.
[*] Performing factordb attack on Public.key.
[*] Attack success with factordb method !
[+] Total time elapsed min,max,avg: 0.3959/0.3959/0.3959 sec.

Results for Public.key:

Unciphered data :
HEX : 0x0000000000000000000000000000000000000000000000000000000000000000534b2d434552547b66346374307264625f72756c337a7d
INT (big endian) : 7977947601300536698481562263351925577708493987537713789
INT (little endian) : 1391628951108875290167398951623073915964910306034729726255023775708603815090875960474766558255445251477116099698774703831740940550144
utf-8 : SK-CERT{f4ct0rdb_rul3z}
STR : b'\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00SK-CERT{f4ct0rdb_rul3z}'
```

### Flag #1
**SK-CERT{f4ct0rdb_rul3z}**