---
title: HTB Cyber Apocalypse 2021 - PhaseStream4 | Crypto
published: true
---

## [](#header-2)PhaseStream4

> The aliens saw us break PhaseStream 3 and have proposed a quick fix to protect their new cipher.

> Downloadable: [output.txt](https://github.com/DamoNeer/hacker-blog/files/6363009/output.txt)

> Given Script

```
from Crypto.Cipher import AES
from Crypto.Util import Counter
import os

KEY = os.urandom(16)


def encrypt(plaintext):
    cipher = AES.new(KEY, AES.MODE_CTR, counter=Counter.new(128))
    ciphertext = cipher.encrypt(plaintext)
    return ciphertext.hex()


with open('test_quote.txt', 'rb') as f:
    test_quote = f.read().strip()
print(encrypt(test_quote))

with open('flag.txt', 'rb') as f:
    flag = f.read().strip()
print(encrypt(flag))

```

### [](#header-3)Solution

1. Both ciphertexts are given
2. C1 ^ P1 = Key; C1 ^ Key = P1 (C1 and C2 are interchangeable; order doesn't matter)
3. The script mentioned that one of the plaintexts is going to be a quote
4. The flag format first five characters begins with CHTB{

C1 refers to ciphertext1

C2 refers to ciphertext2

P1 refers to plaintext1 (quote)

P2 refers to plaintext2 (flag)

Knowing the first five characters of the flag can help us figure out the first five characters of the quote because P2 ^ C2 = Key && Key ^ C1 = P1

I used an online XOR [calculator](http://xor.pw/) for this because I find it faster to deduce the flag manually.

![image](https://user-images.githubusercontent.com/81070073/115824498-dce36000-a3bc-11eb-8696-0f58bf7fe0fb.png)

Left: P2 ^ C2 = Key
Right: Key ^ C1 = P1

![image](https://user-images.githubusercontent.com/81070073/115824598-09977780-a3bd-11eb-8560-ffb853b5b41f.png)

Google is suggesting "alo" to be "alone", which I also find plausible.

![image](https://user-images.githubusercontent.com/81070073/115824798-6004b600-a3bd-11eb-857b-37ed6c128397.png)

Left: P1 ^ C1 = Key
Right: Key ^ C2 = C1

"str" could mean "stream" because of *stream cipher*.

After a couple more manual deduction:

Quote: I alone cannot change the world, but I can cast a stone across the waters to create many ripples
Flag: CHTB{stream_ciphers_with_reused_keystreams_are_vulnerable_to_known_plaintext_attacks}
