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
Opening the _output.txt_ file will give us n, c, e .
Factor the given n into two prime numbers first, then use the downloaded python script as a reference as to how to get the decrypted message back.
```Python
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

## [](#header-2)Monoprime

> Why is everyone so obsessed with multiplying two primes for RSA. Why not just use one?

### [](#header-3)Solution
In the case of one prime number being used, **phi(n) will simply equal to n-1**. 

```Python
\\Python
from Crypto.Util.number import long_to_bytes

p = 1 #prime number 1
#q = n #prime number 2
e = 65537 #encryption key
#n = p * q #prime product
n = 171731371218065444125482536302245915415603318380280392385291836472299752747934607246477508507827284075763910264995326010251268493630501989810855418416643352631102434317900028697993224868629935657273062472544675693365930943308086634291936846505861203914449338007760990051788980485462592823446469606824421932591
c = 161367550346730604451454756189028938964941280347662098798775466019463375610700074840105776873791605070092554650190486030367121011578171525759600774739890458414593857709994072516290998135846956596662071379067305011746842247628316996977338024343628757374524136260758515864509435302781735938531030576289086798942 #ciphertext
phi = (n-1)
d = pow(e, -1, phi) #decryption key

pt = pow(c, d, n)
decrypted = long_to_bytes(pt)

print(decrypted)
```

## [](#header-2)Square Eyes

> It was taking forever to get a 2048 bit prime, so I just generated one and used it twice.

### [](#header-3)Solution
