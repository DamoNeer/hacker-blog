---
title: CryptoHack | RSA Starter
published: true
---

## [](#header-2)RSA Starter 1

> Find the solution to 10117 mod 22663

### [](#header-3)Solution

'''Python
//Python
answer = pow(101, 17, 22663)
'''
## [](#header-2)RSA Starter 2

> "Encrypt" the number 12 using the exponent e = 65537 and the primes p = 17 and q = 23. What number do you get as the ciphertext?

### [](#header-3)Solution

'''Python
//Python
m = 12 #message to be encrypted
e = 65537
p = 17
q = 23
n = p*q

answer = pow(b, e, n)
'''

## [](#header-2)RSA Starter 3

> Given two prime numbers, what's the totient of n ?

### [](#header-3)Solution

'''Python
//Python
p = 857504083339712752489993810777
q = 1029224947942998075080348647219
answer = (p-1)*(q-1)                     #totient of n
