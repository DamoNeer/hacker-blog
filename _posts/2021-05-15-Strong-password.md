---
title: dCTF 2021 - Strong Password | Cracking
published: true
---

## [](#header-2)Strong Password

> Zip file with a password. I wonder what the password could be?

> Downloadable zip file

### [](#header-3)Solution

Given a zip file, I need to find the password to it. 

On my Linux machine, I ran the following command to obtain the hash of the zip file.

Then, I deleted the front and end non-hash text and selected just the $zip2$ hash to crack using Hashcat.

Please note the hash is too long so I just used "..." to represent the content between the front and end signature.

```
\\Linux
$zip2john strong_password.zip > hash.txt
$cat hash.txt

$zip2$.......$zip2$
```

After that, I used hashcat's mode 13600 to crack this particular zip hash after providing rockyou.txt as the wordlist.

![image](https://user-images.githubusercontent.com/81070073/118604604-38113400-b76a-11eb-9b4c-2918fc05b9da.png)

After putting the password as Bo38AkRcE600X8DbK3600, I was able to get the flag.

Flag: dctf{r0cKyoU_f0r_tHe_w1n}
