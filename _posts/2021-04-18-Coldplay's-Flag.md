---
title: UMDCTF 2021 - Coldplay_Flags  | Steg
published: true
---

## [](#header-2)Coldplay_Flags

> I just downloaded Coldplay's latest song and noticed that my song file seems a bit odd. Can you help me figure out what's up with my file?

Note: For the password, think words, not characters

[Flags.wav](https://drive.google.com/drive/folders/1e0D5LElerPu9VcUm2bKp5j542CLuNadB?usp=sharing)

### [](#header-3)Solution

The given audio file plays Coldplay's Flags song and there isn't any weird segment or hidden message in Spectrogram.

Using Binwalk on the given audio file, I was able to extract a bunch of files. Among those files, two of them actually zip files: 

![image](https://user-images.githubusercontent.com/81070073/115163931-d8d3dd00-a060-11eb-8256-f9e88291f730.png)

246D04E contains _flag.txt_

246D131 contains _hint.txt_ <--- can be extracted without password

![image](https://user-images.githubusercontent.com/81070073/115164514-3b2cdd80-a061-11eb-84c1-16f6dd229fc0.png)

Those look like timestamps! Perhaps they refer to the lyrics of the song. (Make sure to listen to the original song on either Spotify or Youtube in order to get the correct words)

Password of the zip: can_tchaikovsky_talk_to_skeletons_by_a_ouija?

After extracting the 246D04E.zip file using that password, I got the flag!

UMDCTF-{PY07r_11Y1CH_7CH41K0V5KY}
