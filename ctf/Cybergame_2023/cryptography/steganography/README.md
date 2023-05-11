# Steganography
## Description
You have come across some interesting images and files that hide secrets. Can you spot them all?

## Task #1: Bus
This image looked suspicious to us. Can you look at him? https://drive.google.com/file/d/1PnuqBg01bOYkoPMKW8Lip1DbHCvbSdNy/view?usp=share_link

`binwalk --dd='.*' bus.png`

There is another png inside.

```
$ file 14CD74 
14CD74: PNG image data, 343 x 34, 8-bit/color RGB, non-interlaced
```

![solve](./binwalk.PNG)

### Flag #1
**SK-CERT{57r4ng3_p1c7ur3}**