---
title: dCTF 2021 - This one is really basic | Crypto
published: true
---

## [](#header-2)This one is really basic

> The answer to life, the universe, and everything.

> Downloadable cipher.txt

### [](#header-3)Solution

The title of this challenge gave me a huge hint that the I need to decode the encrypted message with Base64.

Given how huge the string is, I tried multiple attempts to see how many times I need to decode.

It turns out to be 41 times.

```
import os
import sys
import base64


with open('cipher.txt', 'r') as file:

    encrypted_string = file.read()
    sys. stdout = open("output.txt1", "w")
    decrypted = base64.b64decode(encrypted_string)
    for i in range(41):
        decrypted = base64.b64decode(decrypted)
    print(decrypted)
    sys.stdout.close()
```

Flag = dctf{Th1s_l00ks_4_lot_sm4ll3r_th4n_1t_d1d}
