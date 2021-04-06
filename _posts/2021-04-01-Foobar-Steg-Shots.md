---
title: Foobar CTF 2021 - Shots  | Steg
published: true
---

## [](#header-2)Shots

> Lesly accidently deleted her photos from camera. She gave you the camera to figure it out. BTW, Lesly is Marvel fan.

> Downloadable file: sdcard

### [](#header-3)Solution
After changing the sdcard extension to sdcard.dmg

I used a tool called [Disk Drill](https://www.cleverfiles.com/disk-drill-win.html) to recover the deleted files from her sdcard and I retrieved 9 photos in jpeg and one photo in png. They are all related to Marvel.

![image](https://user-images.githubusercontent.com/81070073/113358986-aeadbb80-92fb-11eb-84a8-3490ca40cad8.png)

Using the strings command, I was able to retrieve the following digital footprint:
```
$3br
456789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
56789:CDEFGHIJSTUVWXYZcdefghijstuvwxyz
```
This is a sign that Steghide was perhaps used.

I retrieved 8 x _notsecret.xtx_ with the same message: **Maybe this is what you want: GLUG{#####wrong_file#####}** and

a _notsosecret.txt_ with a different message: **Maybe "this" is what you want: GLUG{####################}** from Black Panther's picture.

Now there are three things on my mind:
1. It's odd that Captain America's picture is in .png compared to the rest of the crew.
2. The word "this" is emphasized so that could be a passphrase or something.
3. The creator of this challenge gave us a new hint that points toward LSB Steg (since no one has solved it within 12 hours).

> **L**esly accidently deleted her photos from camera. 
> **S**he gave you the camera to figure it out. 
> **B**TW, Lesly is Marvel fan.

Using this [password based LSB Steg tool](https://github.com/ra1nb0rn/lsb_image_stego) on Captain America's picture:
```Linux
\\Linux command
$./lsb_image_stego.py -R -c Captain_America.png -o output.txt -p this
-----------------------------------------------------------
|===== LSB Image Steganography Tool (by Dustin Born) =====|
-----------------------------------------------------------
[+] Parsing carrier image
[+] Recomputing embedding positions
    Progress: [········································] (0/488 positions)
    Progress: [========================================] (488/488 positions)
[+] Extracting secret file from carrier image
    Progress: [········································] (0/61 bytes)
    Progress: [========================================] (61/61 bytes)
[+] Decompressing secret file
[+] Restoring secret file as 'output.txt'
[+] Done

$cat output.txt
Maybe this is what you want: GLUG{y0u_f0und_4_d34d_m4n}
```
