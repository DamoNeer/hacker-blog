---
title: HSCTF 2021 - extended-fibonacci-sequence | Algorithm
published: true
---

## [](#header-2)extended-fibonacci-sequence

[Extended Fibonacci Sequence New.pdf](https://github.com/DamoNeer/hacker-blog/files/6667282/Extended.Fibonacci.Sequence.New.pdf)

![image](https://user-images.githubusercontent.com/81070073/122329347-1f6d7880-cee6-11eb-8ca4-97bf26164f7b.png)

### [](#header-3)Solution

The program will give us a number and that number represents the number of terms we need to perform the summation for.

That number also represents how many Fibonacci numbers we need.

The S sequence is recursive and it is concatenating the corresponding Fibonacci number each time.

If the number given is 3, then these would be the steps we take to get the answer.

Step 1: Recognizing that F1 = 1, F2 = 1, F3 = 2 and S0 has no value.

Step 2: Determining S1, S2, S3

S1 = S0 // F1 = no value concatenates "1" = 1

S2 = S1 // F2 = 1 // 1 = 11

S3 = S2 // F3 = 11 // 2 = 112

Step 3: Find the sum of S1, S2, and S3

S1 + S2 + S3 = 1 + 11 + 112 = 124

Step 4: Only take the last 11 digits from the sum and remove leading zeroes (since that's what they ask for, this has nothing to do with the math)

I have written a script that implements the same logic for the random input they give me.

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
    nc = Netcat(
        'extended-fibonacci-sequence.hsc.tf', 1337)

    nc.buff = b''
    string = nc.read()
    while True:
        string = nc.read_until(b'\n')           # Overwrites the first line
        string = string.decode("utf-8")
        print(string)
        string = string.strip("\n")             # Takes away the new line character
        string = string.strip(": ")             # Takes away the colon character
        n = int(string)

        fibonacci_numbers = [0, 1]
        for i in range(2, n + 1):               # Put the first n fib numbers in a list
            fibonacci_numbers.append(
                fibonacci_numbers[i-1]+fibonacci_numbers[i-2])

        S_list = []
        s = ""
        for i in range(1, n+1):                 # Put the first n S numbers in a list
            s = s + str(fibonacci_numbers[i])   # Concatenation between Si and Fi
            S_list.append(s)

        S_list = [int(i) for i in S_list]       # Convert list of strings to list of integers

        answer = (str(sum(S_list))[-11:])       # Get the last 11 digits of the sum
	answer = answer.lstrip("0")             # Remove leading zeroes
        answer += "\n"
        out = str.encode(str(answer))
        nc.write(out)

```
flag{nacco_ordinary_fib}
