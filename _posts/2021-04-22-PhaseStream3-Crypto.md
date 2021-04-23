---
title: HTB Cyber Apocalypse 2021 - PhaseStream3 | Crypto
published: false
---

## [](#header-2)PhaseStream3

> The aliens have learned the stupidity of their misunderstanding of Kerckhoffs's principle.

> Now they're going to use a well-known stream cipher (AES in CTR mode) with a strong key. 
   
> And they'll happily give us poor humans the source because they're so confident it's secure!

> Downloadable: [ciphertext.txt](https://github.com/DamoNeer/hacker-blog/files/6362850/ciphertext.txt)

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


test = b"No right of private conversation was enumerated in the Constitution. I don't suppose it occurred to anyone at the time that it could be prevented."
print(encrypt(test))

with open('flag.txt', 'rb') as f:
    flag = f.read().strip()
print(encrypt(flag))
```
 

### [](#header-3)Solution

This challenge exploits the fact that the person encrypts two different messages with the same key.

If the attacker knows what keywords one of the plaintext contains and both ciphertexts, then he/she can

possibly decrypt them without knowing the shared key. This method is known as "Crib Drag".

I found this online tool that will do the job for us: [website](https://toolbox.lotusfa.com/crib_drag/)

The script exposed that one of the plaintext was : "No right of private conversation was enumerated in the Constitution. I don't suppose it occurred to anyone at the time that it could be prevented."

Hence, I entered that as "Crib words" on the website.

![image](https://user-images.githubusercontent.com/81070073/115821436-a525e980-a3b7-11eb-9e46-62e6c3ff7ceb.png)
