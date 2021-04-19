---
title: UMDCTF 2021 - Roy's Randomness  | Steg
published: true
---

## [](#header-2)Roy's Randomness

> Roy found some suspicious network traffic, wireshark shows so many errors with it! Can you figure out what's happening?

> Downloadable file: network traffic

### [](#header-3)Solution

![image](https://user-images.githubusercontent.com/81070073/115175416-c61cd080-a07f-11eb-992f-71c699bb1295.png)

There are only three types of packets: SYN, RST, and PSH. If we assume the PSH packet is a pause between characters, we will see that the group of characters are between 4 and 5,
which could be Morse code.

Assuming SYN is dot while RST is dash, I will get an output that seems to be in hexadecimals. Once I convert from hex to ASCII, I will get the flag!

![image](https://user-images.githubusercontent.com/81070073/115176186-7808cc80-a081-11eb-8a73-08bff3e7cbc2.png)


