---
title: DawgCTF 2021 - Two Truths and a Fib | Programming
published: false
---

## [](#header-2)Two Truths and a Fib

> Can you catch the fibber?

> nc umbccd.io 6000

![image](https://photos.google.com/lr/photo/AH6roBPiVfaNKNO5LZXGseCiJMEAMRyXfNYCaP5M-1qQNv36XUeOjTfBrDqS1E_ccAVPY1a3ofTvjSRDdUpGsNkvBphRWkToAQ)

### [](#header-3)Solution

I have seen a couple challenges like this that tests a person's programming skills when it comes to writing a script that provides correct outputs in a fast and accurate manner.

This was my first time solving it, so my script is a bit messy.

```
import socket
import re

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
    nc = Netcat('umbccd.io', 6000)
    counter = 0                     #I created a counter to see how many successful attempts I need to make.
    for i in range(100):
        answer = 0

        nc.buff = b''
        flag = nc.read_until(b"\n")         #At first I thought this will give me the flag, but it didn't because the flag's sentence didn't have "\n"
        print(flag)
        string = nc.read_until(b"]")        #read until the end of a list
        string = string.decode("utf-8")       
        string = re.findall(r"\d\d+", string)    #Find all numbers that are 2 digits higher. I want to skip 1 digit numbers because the example that the server gave contains 1 digit numbers.
        integer_list=list(map(int,string))      #Convert list of strings to list of integers
        print(integer_list)
        for n in integer_list:                
            c=0
            a=1
            b=1
            while c<n:                          #This is a fibonacci number checker function I found online
                c=a+b
                b=a
                a=c
            if c==n:
                answer = n
        answer = str(answer)                   #converting the number back to a string type
        print(answer)
        answer += "\n"                         #adding the "enter" or line break effect behind the number
        out = str.encode(str(answer))          #encode
        nc.write(out)                          #write the answer along with the line break
        counter += 1
        print("Counter: "+str(counter))

    nc.buff = b''
    flag = nc.read_until(b"}")      #I defined a new flag variable down here to make sure that I will get the flag this time by setting the end to "}"
    print(flag)
    
```

Flag: DawgCTF{jU$T_l1k3_w3lc0me_w33k}
