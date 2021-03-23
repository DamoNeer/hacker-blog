---
title: TryHackMe Challenge | The Impossible Challenge
published: true
---

## [](#header-2)Task 1: Submit the flag

> Download the file, and find the Flag!
> qo qt q` r6 ro su pn s_ rn r6 p6 s_ q2 ps qq rs rp ps rt r4 pu pt qn r4 rq pt q` so
> pu ps r4 sq pu ps q2 su rn on oq o_ pu ps ou r5 pu pt r4 sr rp qt pu rs q2 qt r4 r4 ro su pq o5


### [](#header-3)Solution

After downloading the zip file, I was prompted to enter a password in order to view the content within "flag.txt". At that point, I assumed that I would have to decipher the prompt in order to get the password.

Using Cyberchef, a combination of ROT 13 and ROT 47 gave us something that resembled a hexadecimal output. After converting from hexadecimal, the output appeared to be base64.

![image](https://user-images.githubusercontent.com/81070073/112229894-0a3bc300-8bf1-11eb-8c08-7dbd74b4d442.png)

The output tells us that we need to take a step back and revisit somewhere we have been before. I could only think of one place and that would be the Tryhackme challenge page.
Looking at the source code, I have come across a suspicious code for the string "hmm" right below the challenge title.

![image](https://user-images.githubusercontent.com/81070073/112230059-6f8fb400-8bf1-11eb-89eb-03d0593f4dc2.png)

"zwnj" stands for Zero Width Non Jointer and there is a form of steganography that uses these zero width characters

I used this website: https://330k.github.io/misc_tools/unicode_steganography.html to help me reveal the hidden text within the string "hmm", and I got the password for the zip file.
