---
title: HTB Cyber Apocalypse 2021 - PhaseStream4 | Crypto
published: false
---

## [](#header-2)PhaseStream4

> The aliens saw us break PhaseStream 3 and have proposed a quick fix to protect their new cipher.

> This challenge will raise 43 euros for a good cause.

> Downloadable: [output.txt](https://github.com/DamoNeer/hacker-blog/files/6362882/output.txt)

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

1. Both ciphertexts are given from _output.txt_
 
2. The flag format is CHTB{

3. The code hinted that one of the plaintext is a quote.

4. C1 ^ P1 = Key; C2 ^ P2 = Key; C1 ^ Key = P1 (Order doesn't matter; C1 and C2 are interchangeable)

In my mind, P1 refers to the quote, P2 refers to the flag, C1 refers to the ciphertext of the quote, and C2 refers to the ciphertext of the flag.

Remmeber that XOR is bitwise, which means using just the first five characters of the flag CHTB{ , I can xor it with C2 to get the key, which then can be xor'ed with P1

to reveal parts of the quote.

I decided to do this manually to save time by using this online XOR calculator: [website](http://xor.pw/#)

![image](https://user-images.githubusercontent.com/81070073/115822354-43ff1580-a3b9-11eb-9c96-a2be4da8eb79.png)

Keep in mind that 1 ASCII char = 2 Hex digits, so that's why I took 10 out from c2.

![image](https://user-images.githubusercontent.com/81070073/115822513-858fc080-a3b9-11eb-8eb1-25ac71d1a785.png)

Doing a google search on "I alo quote" prompted an autocorrection "I **alone** quote"

![image](https://user-images.githubusercontent.com/81070073/115822607-aeb05100-a3b9-11eb-82df-f9ea7aeec08c.png)

This potentially gives two more characters to play with. Let's try that!

![image](https://user-images.githubusercontent.com/81070073/115823049-7b21f680-a3ba-11eb-8e8e-c7fa3422cfa4.png)

![image](https://user-images.githubusercontent.com/81070073/115823118-9ab91f00-a3ba-11eb-92cf-edc1f5251e6b.png)

Yes, that seems to be working and is giving us three more letters (don't forget to add the space behind "alone") that most likely will spell "string" or "stream" because of *stream ciphers*.

After a bunch of manual deductions...

Quote: I alone cannot change the world, but I can cast a stone across the waters to create many ripples
Flag: CHTB{stream_ciphers_with_reused_keystreams_are_vulnerable_to_known_plaintext_attacks}
