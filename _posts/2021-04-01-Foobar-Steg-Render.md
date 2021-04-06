---
title: Foobar CTF 2021 - Render | Steg
published: true
---

## [](#header-2)Render

> My friend AFKAP send me a latest rendered song but i think it has some issues can you please do some basic checks before he release the song

> Downloadable file: chall.wav

### [](#header-3)Solution
I will be using a tool called **Audacity** to analyze and decode the audio.
After listening to the song, a segment of it is out of place as it contains machinery sound, so I googled common ways of audio steganography and I was able to find the solution.

This is in waveform.
![image](https://user-images.githubusercontent.com/81070073/113341300-dabc4300-92e1-11eb-851d-7dc079bf552a.png)

After changing it to spectrogram, I was able to retrieve the flag: GLUG:{5p37rum_fun_101}
![image](https://user-images.githubusercontent.com/81070073/113341171-b6f8fd00-92e1-11eb-8d73-27545d95113a.png)

