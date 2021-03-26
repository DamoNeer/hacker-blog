---
title: CryptoHack | Public Exponent
published: true
---

## [](#header-2)Salty

> Smallest exponent should be fastest, right?

### [](#header-3)Solution
This felt like a review questions since I was able to get the flag without making new changes.
n, c, e were given where n is not known to have prime factors.

```Python
\\Python
from Crypto.Util.number import long_to_bytes

#p =  #prime number 1
#q =  #prime number 2
e = 1 #encryption key
#n = p * q #prime product
n = 110581795715958566206600392161360212579669637391437097703685154237017351570464767725324182051199901920318211290404777259728923614917211291562555864753005179326101890427669819834642007924406862482343614488768256951616086287044725034412802176312273081322195866046098595306261781788276570920467840172004530873767
c = 44981230718212183604274785925793145442655465025264554046028251311164494127485 #ciphertext
phi = (n-1)

d = pow(e, -1, phi) #decryption key
pt = pow(c, d, n)
decrypted = long_to_bytes(pt)

print(decrypted)
```

## [](#header-2)Modulus Inutilis

> My primes should be more than large enough now!

### [](#header-3)Solution
Remember that: 
`c = pt^e % n`
Since n >> c and e is very small, it's reasonable to estimate:
`c = pt^e` 
This means we simply need to **take the eth root of c** (ciphertext) in order to find pt (plaintext).
The code for _iroot_ is taken from the _gmpy2_ module.
```Python
\\Python
def iroot(x, n):
    """Return (y, b) where y is the integer nth root of x and b is True if y is exact."""
    if x == 0:
        return x, True

    k = (x.bit_length() - 1) // n
    y = 1 << k
    for i in range(k - 1, -1, -1):
        z = y | 1 << i
        if z ** n <= x:
            y = z
    return y, x == y ** n
    
e = 3 #encryption key
c = 243251053617903760309941844835411292373350655973075480264001352919865180151222189820473358411037759381328642957324889519192337152355302808400638052620580409813222660643570085177957 #ciphertext
pt = iroot(c,3)[0]
decrypted = long_to_bytes(pt)
print(decrypted)
