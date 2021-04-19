---
title: UMDCTF 2021 - Testudo's Message | Steg
published: true
---

## [](#header-2)Testudo's Message
> My friend Testudo keeps sending me this picture and I don't know what he is trying to tell me. Can you figure out what he is saying?

> You will have to wrap what you find in this challenge with UMDCTF-{} yourself before submitting

> Given image: ![testudos_message](https://user-images.githubusercontent.com/81070073/115178585-755ca600-a086-11eb-9cef-1c4ed9af5ab0.png)

### [](#header-3)Solution

There are only red lines and white lines. This picture is screaming "binary" to me. However, this seems to be a 7 bit binary rather than the usual 8 bit because
there are only seven lines in a tile.

![image](https://user-images.githubusercontent.com/81070073/115179164-8a860480-a087-11eb-8ef9-c5d4978e2194.png)

Just know that this is a 7 bit binary isn't enough. You will still need to guess the order correctly. When I unscrambled the characters a bit, I know there is going to be
"shell" in leet speak, so I tried to find the path that would lead to shell and try to find the full path from there.

Flag: UMDCTF-{h!dd3n-$h3lls}
