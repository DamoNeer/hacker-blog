---
title: HTB Cyber Apocalypse 2021 - PhaseStream 1	 | Crypto
published: true
---

## [](#header-2)PhaseStream 1	

> The aliens are trying to build a secure cipher to encrypt all our games called "PhaseStream". 

> They've heard that stream ciphers are pretty good. 

> The aliens have learned of the XOR operation which is used to encrypt a plaintext with a key. 

> They believe that XOR using a repeated 5-byte key is enough to build a strong stream cipher. 

> Such silly aliens! Here's a flag they encrypted this way earlier: 2e313f2702184c5a0b1e321205550e03261b094d5c171f56011904 

> Can you decrypt it (hint: what's the flag format?) 

### [](#header-3)Solution

In bitwise XOR operation, key ^ plaintext = ciphertext && key ^ ciphertext = plaintext && ciphertext & plaintext = key. (Also, order doesn't matter.)

We already know the first five characters of the plaintext and the first five characters of the ciphertext.

We can easily get the first five characters of the key as well.

```
from itertools import cycle

encoded = bytes.fromhex('2e313f2702184c5a0b1e321205550e03261b094d5c171f56011904')

flag_start = 'CHTB{'

# decrypt the first part of the encoded
key = [encoded[i] ^ ord(c) for i, c in enumerate(flag_start)]
print('Decrypted key part:', ''.join(chr(i) for i in key))



#mykey
```

After that, just extract the plaintext using the given key.

![image](https://user-images.githubusercontent.com/81070073/115814983-f9769c80-a3aa-11eb-9ab3-8f97f8ac8ed8.png)
