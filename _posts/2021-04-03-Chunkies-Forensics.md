---
title: ShaktiCon CTF 2021 - Chunkies  | Forensics
published: true
---

## [](#header-2)Chunkies

> We could only retrieve only this file from the machine, but looks like this is corrupted. Can you recover the file?
> Downloadable file: _file.png_

### [](#header-3)Solution
For this challenge, I used two tools to recover the file: [HexEd.it](https://hexed.it/) and the command [pngcheck](https://installlion.com/kali/kali/main/p/pngcheck/install/index.html)

I also used this [website](https://hackmd.io/@FlsYpINbRKixPQQVbh98kw/Sk_lVRCBr) to help me learn the critical chunks of a png file.

There will be a lot of back and forth between HexEd and pngcheck.

![image](https://user-images.githubusercontent.com/81070073/113530308-88498380-957a-11eb-8fff-b5b691c31ec3.png)

Right away, I saw that the png header is wrong. There was a missing "89" byte, so I added that in front.

![image](https://user-images.githubusercontent.com/81070073/113530366-b5963180-957a-11eb-8ed4-36c1b9a7e343.png)

```Linux
//Linux
$pngcheck file.png
file.png  CRC error in chunk IHDR (computed 5a7b8bdc, expected 5a9b8bdc)
ERROR: file.png
```

**pngcheck** told me that there was an error related to the IHDR chunk, specifically its CRC portion. The correct bytes should have been 5a 7b 8b dc instead of

5a 9b 8b dc (I know, it didn't really make any sense for pngcheck to use the word "expected" here). Anyway, I fixed that.

![image](https://user-images.githubusercontent.com/81070073/113530755-a663b380-957b-11eb-8035-bd996268818f.png)

```Linux
//Linux
$pngcheck file.png
file.png  illegal (unless recently approved) unknown, public chunk IADT
ERROR: file.png
```

This time, **pngcheck** told me that the chunk IADT was wrong. After googling, I found out that IADT should have been IDAT instead. I found all of the IADT strings and changed them into IDAT. (There were like 7 or 8 occurances I think)

```Linux
//Linux
$pngcheck file.png
file.png  illegal (unless recently approved) unknown, public chunk INED
ERROR: file.png
```

Finally, **pngcheck** stated that INED was wrong. Google told me that it should have been IEND instead. (There was only one mistake)

```Linux
//Linux
$pngcheck file.png
OK: file.png (512x308, 32-bit RGB+alpha, non-interlaced, 92.8%).
```

After opening the image successfully, the flag was retrieved!

![image](https://user-images.githubusercontent.com/81070073/113531082-a912d880-957c-11eb-93cb-bdc92e557aa6.png)
