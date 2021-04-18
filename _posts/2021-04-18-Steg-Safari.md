---
title: UMDCTF 2021 - Steg Safari  | Steg
published: true
---

## [](#header-2)Steg Safari
> There are a lot of steps to this challenge. Are you ready to get on the ride with me to find this wild flag?

> Given image: ![steg_safari](https://user-images.githubusercontent.com/81070073/115165332-3157aa00-a062-11eb-8c87-e809c3c0d6c1.jpg)

### [](#header-3)Solution

The prompt was not lying about having numerous steps in this challenge. This steg challenge was one of the best steg challenges that I have ever done.

I am going to talk about my process step by step.

**Step 1: Binwalk the given image (steg_safari.jpg)**

![image](https://user-images.githubusercontent.com/81070073/115165416-b216a600-a062-11eb-8528-36bbb77abca3.png)

Binwalk should have extracted this folder and within this folder, there are four files, which Binwalk told us that they could be zip files.

![image](https://user-images.githubusercontent.com/81070073/115165473-e38f7180-a062-11eb-8df8-3d720bbe7daf.png)

Turns out, 39734 was the only zip file that could be opened and here are what's inside.

![image](https://user-images.githubusercontent.com/81070073/115165546-3406cf00-a063-11eb-80b7-72f016d7e853.png)

**Step 2: Explore the extracted folder and find the next clue.**

After spending some time exploring, I would like to focus on _document.xml_ within the _word_ folder.

![image](https://user-images.githubusercontent.com/81070073/115165612-80eaa580-a063-11eb-9b7c-ef8a512bc550.png)

It mentioned that there would be a *compass* that can help me and I remember seeing a safari image within the _media_ folder.

**Step 3: Zsteg the safari icon png**

Using Zsteg, I see that there is another png file within and I can extract it using the "b1,rgb,lsb,xy" payload.

![image](https://user-images.githubusercontent.com/81070073/115165728-1ab25280-a064-11eb-9d2b-5cb125493fd5.png)

```
//Command
$zsteg image1.png -E "b1,rgb,lsb,xy" > output.png
```

![output.png](https://user-images.githubusercontent.com/81070073/115165801-81377080-a064-11eb-8688-db9d872f1969.png)

Step 4: QR code then Google Drive

I used this [QR decoder](https://zxing.org/w/decode.jspx) and the google drive link has an audio file "flag.wav" for me to download.

Step 5: Binary

The author pranked us and hinted that there is something interesting happening in the beginning of the audio file.

I used Audacity to open the audio file and I pressed the zoom in button like ten times to see it clearly in waveform.

![image](https://user-images.githubusercontent.com/81070073/115165923-3ec26380-a065-11eb-85ce-e5e5ee93ff68.png)

Dot = 0 and Line = 1

![image](https://user-images.githubusercontent.com/81070073/115165966-91038480-a065-11eb-91bb-89f9841e4a5e.png)
