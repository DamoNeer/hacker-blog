---
title: dCTF 2021 - Just Take Your Time | Programming
published: true
---

## [](#header-2)Just Take Your Time

> Let's go. In and out. 2 second adventure.

> Downloadable python script

### [](#header-3)Solution

![image](https://user-images.githubusercontent.com/81070073/118599616-ace16f80-b764-11eb-8e3d-883b260a62f0.png)

This challenge comes with a downloadable python script, so it gave me a very clear idea of what kind of responses I will get from it.

First, it will prompt the user for the product of two randomly generated numbers.

Then, it will take the current time, do something to it, and make it a key.

Next, it will use Triple DES to encrypt a secret message using the key that was derived from time and an IV of b'00000000'

After that, it will give the user the hex value of the encrypted message and prompt user to decrypt it. 

I have written a script that will help me automatically get the product of the two given numbers, unhex and decrypt the secret message in order to get the flag.

```
import socket
import re
from Crypto.Cipher import DES3
import time
from time import time
from random import randint
from secrets import token_hex


class Netcat:
    """ Python 'netcat like' module """

    def __init__(self, ip, port):
        self.buff = ""
        self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        self.socket.connect((ip, port))

    def read(self, length=1024):
        """ Read 1024 bytes off the socket """

        return self.socket.recv(length)

    def read_until(self, data):
        """ Read data into the buffer until we have data """

        while not data in self.buff:
            self.buff += self.socket.recv(1024)

        pos = self.buff.find(data)
        rval = self.buff[:pos + len(data)]
        self.buff = self.buff[pos + len(data):]

        return rval

    def write(self, data):
        self.socket.send(data)

    def close(self):
        self.socket.close()


if __name__ == '__main__':
    for i in range(1000):
        nc = Netcat(
            'dctf-chall-just-take-your-time.westeurope.azurecontainer.io', 9999)

        nc.buff = b''
        string = nc.read()
        string = string.decode("utf-8")
        string = re.findall(r"\d\d+", string) #Focus on the two given numbers
        integer_list = list(map(int, string))
        product = integer_list[0]*integer_list[1] #Calculate the product of those two given numbers
        product = str(product)  # converting the number back to a string type
        product += "\n"  # adding the "enter" or line break effect behind the number
        out = str.encode(str(product))  # encode
        nc.write(out)  # write the answer along with the line break
        key = str(int(time())).zfill(16).encode("utf-8") #Get time and use it as the key
        nc.buff = b''
        string = nc.read()
        string = string.decode("utf-8")
        given_hex = re.findall(r"[0-9a-f]{10,}", string) #Focus on the encrypted hex value
        encrypted = (bytes.fromhex(given_hex[0])) #Unhex
        d_cipher = DES3.new(key, DES3.MODE_CFB, b"00000000") #Created a decrypting cipher with the same key and IV
        d_data = d_cipher.decrypt(encrypted) #utilize the decrypt function
        try:
            answer = (d_data.decode())
            answer += "\n"
            out = str.encode(str(answer)) 
            nc.write(out) #submit answer
            nc.buff = b''
            string = nc.read() #read flag
            print(string)
        except UnicodeDecodeError:
            pass
```

Flag: dctf{1t_0n1y_t0Ok_2_d4y5...}
