---
title: HSCTF 2021 - Audio Frequency | Steg
published: true
---

## [](#header-2)Audio Freqency

> What a mundane song, it's just the same note repeated over and over. But could there perhaps be two different notes?

### [](#header-3)Solution

As I zoomed into their waveforms, I see two different waves, which possibly means binary.

I verified that by looking at the first and last character. If they represent "f" and "}" in binary, then that validates my theory.

Longer wavelength = 0 ; shorter wavelength = 1. 

![image](https://user-images.githubusercontent.com/81070073/122635348-35229f80-d098-11eb-9ab5-2da4f40559ac.png)

Yes, "slight" was misspelled, but that was the correct flag.
