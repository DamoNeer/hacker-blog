---
title: UMDCTF 2021 - Pickle Rick  | Steg
published: true
---

## [](#header-2)Pickle Rick

> You recieve these audio files from someone named Alan Eliasen.

> Downloadable files: "rickroll.wav" and "together-forever-encoded.wav"

### [](#header-3)Solution

After trying strings command, binwalk, and spectrogram via Audacity, I decided to look at the question more carefully.
I noticed that the question gave us a specific name "Alan Eliasen". I did a google search and I found this (website)[https://futureboy.us/stegano/]

I first uploaded the "together-forever-encoded.wav" without a passphrase, and the output was "The password is "big_chungus"!"

Then, I uploaded the "rickroll.wav" with the passphrase "big_chungus" and I got the flag!

Flag: UMDCTF-{n3v3r_g0nna_l3t_y0u_d0wn}


