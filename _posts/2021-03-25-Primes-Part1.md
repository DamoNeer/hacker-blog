---
title: CryptoHack | Primes Part 1
published: true
---

## [](#header-2)Factoring

> Factorise the 150-bit number 510143758735509025530880200653196460532653147 into its two constituent primes. Give the smaller one as your answer.

### [](#header-3)Solution
http://factordb.com/index.php?query=510143758735509025530880200653196460532653147+

## [](#header-2)Inferius Prime

> Here is my super-strong RSA implementation, because it's 1600 bits strong it should be unbreakable... at least I think so!

### [](#header-3)Solution
Factor the given n into two prime numbers first, then use the output.txt as a reference as to how to get the decrypted message back.
```
\\Python
from Crypto.Util.number import long_to_bytes

p = 986369682585281993933185289261 #prime number 1
q = 752708788837165590355094155871 #prime number 2
e = 3 #encryption key
#n = p * q #prime product
n = 742449129124467073921545687640895127535705902454369756401331
c = 39207274348578481322317340648475596807303160111338236677373 #ciphertext
phi = (p-1)*(q-1)
d = pow(e, -1, phi) #decryption key

pt = pow(c, d, n)
decrypted = long_to_bytes(pt)

print(decrypted)
```

