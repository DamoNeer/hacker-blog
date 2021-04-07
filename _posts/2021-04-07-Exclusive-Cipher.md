---
title: ShaktiCon CTF 2021 - Exclusive Cipher  | Crypto
published: true
---

## [](#header-2)Exclusive Cipher

> Clam decided to return to classic cryptography and revisit the XOR cipher! Here's some hex encoded ciphertext:

> ae27eb3a148c3cf031079921ea3315cd27eb7d02882bf724169921eb3a469920e07d0b883bf63c018869a5090e8868e331078a68ec2e468c2bf13b1d9a20ea0208882de12e398c2df60211852deb021f823dda35079b2dda25099f35ab7d218227e17d0a982bee7d098368f13503cd27f135039f68e62f1f9d3cea7c

> The key is 5 bytes long and the flag is somewhere in the message.

### [](#header-3)Solution

I used this [website](https://www.dcode.fr/xor-cipher) to help me decipher the message.

I have identified the input as "Hexadecimal Extended ASCII" and key size = 5.

![image](https://user-images.githubusercontent.com/81070073/113944366-44908d00-97b9-11eb-9567-945c0c9276c9.png)


