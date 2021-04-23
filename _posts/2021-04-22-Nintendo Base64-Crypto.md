---
title: HTB Cyber Apocalypse 2021 - Nintendo Base64 | Crypto
published: false
---

## [](#header-2)Nintendo Base64

> Aliens are trying to cause great misery for the human race by using our own cryptographic technology to encrypt all our games.

> Fortunately, the aliens haven't played CryptoHack so they're making several noob mistakes. Therefore they've given us a chance to recover our games and find their flags.

> They've tried to scramble data on an N64 but don't seem to understand that encoding and ASCII art are not valid types of encryption!

> This challenge will raise 33 euros for a good cause.

> Downloadable: output.txt

### [](#header-3)Solution

![image](https://user-images.githubusercontent.com/81070073/115813668-72282980-a3a8-11eb-8160-bfb51bbce315.png)

This is what we get from the _output.txt_ file and seems like it's a hint that I should base64 decode the given string eight times.

I used Cyberchef to quickly get the flag.


![image](https://user-images.githubusercontent.com/81070073/115813869-dd71fb80-a3a8-11eb-83e0-27e8aafa5f5d.png)

