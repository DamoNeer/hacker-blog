---
title: Foobar CTF 2021 - Profezzor revenge | Crypto
published: true
---

## [](#header-2)revenge

> The Profezzor has sent an encrypted pdf assignment, but he told it's easily crackable. Can you guys figure out the assignment, I need to submit it ASAP.

> Downloadable file: assignment

### [](#header-3)Solution
The file comes with no extension. I changed it to PDF and it did nothing. 

Using **Hexed.it**, I saw that the header didn't resemble a PDF header at all. Normally a pdf header would begin with 25 50 44 46 (%PDF).

![image](https://user-images.githubusercontent.com/81070073/113377128-4c1ee480-9328-11eb-8f8b-275a4b60ef47.png)

Looking at the title again and considering the fact that this is a Crypto challenge, the word "profezzor" sort of screams "xor" to me. 
So, I checked to see if the first few bytes was indeed encrypted using xor.

FYI: pdf has the file signature of _\x25\x50\x44\x46_

```Python
//Python
from pwn import xor
encrypted = b'\x8b\xaa\xfe\xf8'
pdf = b'\x25\x50\x44\x46'
key = (xor(encrypted, pdf))
print(key)

b'\xae\xfa\xba\xe'                #key
```
Ok. In order to test this key, we simply need to open this file, xor it, and turn the output into a pdf.

```Python
//Python
from pwn import xor
encrypted = b'\x8b\xaa\xfe\xf8'
pdf = b'\x25\x50\x44\x46'
key = (xor(encrypted, pdf))

file = open('assignment', 'rb')         #open the assignment file
read_file = file.read()                 #read the file content
result = open('assignment.pdf', 'wb')   #make a new pdf file
result.write(xor(read_file, key))       #xor the read_file with key and write it as the new pdf file

file.close()
result.close()
```

Opening the pdf, we will get this scanned document, which contains the flag itself.

![image](https://user-images.githubusercontent.com/81070073/113378989-319b3a00-932d-11eb-9071-27f52ee7a84a.png)
