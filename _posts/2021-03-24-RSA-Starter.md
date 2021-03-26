---
title: CryptoHack | RSA Starter
published: true
---

## [](#header-2)RSA Starter 1

> Find the solution to 10117 mod 22663

### [](#header-3)Solution

```Python
\\Python
answer = pow(101, 17, 22663)
```
## [](#header-2)RSA Starter 2

> "Encrypt" the number 12 using the exponent e = 65537 and the primes p = 17 and q = 23. What number do you get as the ciphertext?

### [](#header-3)Solution

```Python
\\Python
m = 12 #message to be encrypted
e = 65537
p = 17
q = 23
n = p*q

answer = pow(b, e, n)
```

## [](#header-2)RSA Starter 3

> Given two prime numbers, what's the totient of n ?

### [](#header-3)Solution

```Python
\\Python
p = 857504083339712752489993810777
q = 1029224947942998075080348647219
answer = (p-1)*(q-1)                     #totient of n
```

## [](#header-2)RSA Starter 4

> Given two prime numbers and the exponent, what is the private key d ?

### [](#header-3)Solution
```Python
\\Python
p = 857504083339712752489993810777
q = 1029224947942998075080348647219
e = 65537
phi = (p-1)*(q-1)                        #totient of n
d = pow(e, -1, phi)                      
```

## [](#header-2)RSA Starter 5

> Use the private key that you found for these parameters in the previous challenge to decrypt this ciphertext.

### [](#header-3)Solution

```Python
\\Python
p = 857504083339712752489993810777
q = 1029224947942998075080348647219
n = 882564595536224140639625987659416029426239230804614613279163
e = 65537
c = 77578995801157823671636298847186723593814843845525223303932
phi = (p-1)*(q-1)                        #totient of n
d = pow(e, -1, phi)  

answer = pow(c, d, n)
```

## [](#header-2)RSA Starter 6

> Sign the flag crypto{Immut4ble_m3ssag1ng} using your downloaded private key (which contains n and d) and the SHA256 hash function.

### [](#header-3)Solution

```Python
\\Python
from Crypto.Hash import SHA256
from Crypto.Util.number import bytes_to_long

p = 857504083339712752489993810777 #prime number 1
q = 1029224947942998075080348647219 #prime number 2
e = 65537 #encryption key
#n = p * q #prime product
n = 15216583654836731327639981224133918855895948374072384050848479908982286890731769486609085918857664046075375253168955058743185664390273058074450390236774324903305663479046566232967297765731625328029814055635316002591227570271271445226094919864475407884459980489638001092788574811554149774028950310695112688723853763743238753349782508121985338746755237819373178699343135091783992299561827389745132880022259873387524273298850340648779897909381979714026837172003953221052431217940632552930880000919436507245150726543040714721553361063311954285289857582079880295199632757829525723874753306371990452491305564061051059885803
d = 11175901210643014262548222473449533091378848269490518850474399681690547281665059317155831692300453197335735728459259392366823302405685389586883670043744683993709123180805154631088513521456979317628012721881537154107239389466063136007337120599915456659758559300673444689263854921332185562706707573660658164991098457874495054854491474065039621922972671588299315846306069845169959451250821044417886630346229021305410340100401530146135418806544340908355106582089082980533651095594192031411679866134256418292249592135441145384466261279428795408721990564658703903787956958168449841491667690491585550160457893350536334242689
c = 77578995801157823671636298847186723593814843845525223303932 #ciphertext
phi = (p-1)*(q-1)
#d = pow(e, -1, phi) #decryption key



hash = SHA256.new(data=b'crypto{Immut4ble_m3ssag1ng}')
S = pow(bytes_to_long(hash.digest()), d, n)
print(hex(S)[2:])
```