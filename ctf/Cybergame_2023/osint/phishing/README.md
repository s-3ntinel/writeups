# Phishing
## Description
You have been the target of a phishing campaign. You have decided to analyze this campaign.

## Task #1: Phishing link
The link in the phishing campaign is http://spotify-web.rf.gd/ and aims to steal credit card information

Showing source code of the website reveals.

```
<!--
TODO:remove this debug comment aHR0cHM6Ly9pbmZvc2VjLmV4Y2hhbmdlL0ByaW5fZGlkZSNTSy1DRVJUezVwMDcxZnlfcGgxNTFuZ193M2J9ICAgIDw+Pj5UaGlzIHdlYiBpcyBwYXJ0IG9mIGN5YmVyLXNlY3VyaXR5IENURiAtIEN5YmVyIEdhbWUgMjAyMzw8PD4K
-->
```

Decode `base64`.

```
https://infosec.exchange/@rin_dide#SK-CERT{5p071fy_ph151ng_w3b}    <>>>This web is part of cyber-security CTF - Cyber Game 2023<<<>
```

### Flag #1
**SK-CERT{5p071fy_ph151ng_w3b}**

## Task #2: Network
Visit the website. Inspect element on first image.

```
<img class="letterbox" src="https://media.infosec.exchange/infosecmedia/media_attachments/files/109/938/041/090/403/543/small/6aa950be48567f2c.jpg" srcset="https://media.infosec.exchange/infosecmedia/media_attachments/files/109/938/041/090/403/543/original/6aa950be48567f2c.jpg 978w, https://media.infosec.exchange/infosecmedia/media_attachments/files/109/938/041/090/403/543/small/6aa950be48567f2c.jpg 636w" sizes="578px" alt="https://t.me/phishers_public#SK-CERT{m45t0d0n_ph15hing_4cc0unt}" title="https://t.me/phishers_public#SK-CERT{m45t0d0n_ph15hing_4cc0unt}" lang="en">
```

### Flag #2
**SK-CERT{m45t0d0n_ph15hing_4cc0unt}**

## Task #3: Collaboration
Join telegram from link. User Riny Diede has flag in his bio.

![solve](./flag_3.PNG)

### Flag #3
**SK-CERT{t3l3gr4m_c0ll4b0r4t10n}**

## Task #4: Docs
Download file phish_final.png. `Exiftool` the image.

```
ExifTool Version Number         : 12.57
File Name                       : phish_final.png
Directory                       : /home/kali/Pictures
File Size                       : 32 kB
File Modification Date/Time     : 2023:04:17 14:44:25-04:00
File Access Date/Time           : 2023:04:18 14:04:09-04:00
File Inode Change Date/Time     : 2023:04:18 14:03:54-04:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 592
Image Height                    : 489
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
SRGB Rendering                  : Perceptual
Gamma                           : 2.2
Pixels Per Unit X               : 3779
Pixels Per Unit Y               : 3779
Pixel Units                     : meters
Exif Byte Order                 : Big-endian (Motorola, MM)
Resolution Unit                 : inches
Y Cb Cr Positioning             : Centered
Exif Version                    : 0232
Components Configuration        : Y, Cb, Cr, -
User Comment                    : https://docs.google.com/document/d/1u80o2TRIcl2FKyP6BAWe0f0OnCD6pHlREfi3p_T8yAw
Flashpix Version                : 0100
Image Size                      : 592x489
Megapixels                      : 0.289
```

Visit the google doc and copy hyperlink from Update button.

```
https://gitlab.com/mist-schi-fhuu/my-web#SK-CERT{f0rg0773n_gi714b_link}
```

### Flag #4
**SK-CERT{f0rg0773n_gi714b_link}**

## Task #5: Lab
Download repo from Gitlab. Run `ls-tree`.

```
$ git ls-tree 7f0748ca95f27a94dbc14c30ce5261a2f6e9468b
100644 blob e35476685a18531e0f64002f8eb3a75f31c3e382    README.md
100644 blob 452e5af1a2fadac08483c6d427dfb18cb57dfcc8    email.txt
100644 blob 5e9085d70c2f9c46d1d365e034b9ae967a9b5199    favicon.png
100644 blob 028d7f561a316c65a6a22d141d67ab2e23660578    index.html
100644 blob 9b1400970b4d6dafe7443bd8c0862128a8a846d7    style.css
```

There is a file `email.txt` that was removed in later commits. Dump its content.

```
$ git show 452e5af1a2fadac08483c6d427dfb18cb57dfcc8
On 20 Dec 2022, at 9:08, I**** S**** <i****l@****> wrote:
>
> Hello,
>
> the url with endpoint is card-stealer-url.evil/info#SK-CERT{unw4n73d_c0mm17}
>
> I.
>
> > On 19 Dec 2022, at 16:29, A**** S**** <a****@****> wrote:
> >
> > Message body:
> > Hi,
> > 
> > can you send me the final URL where card info will be send ?
> >
> > thx, A.
> >
> > --
> > 
>
```

### Flag #5
**SK-CERT{unw4n73d_c0mm17}**