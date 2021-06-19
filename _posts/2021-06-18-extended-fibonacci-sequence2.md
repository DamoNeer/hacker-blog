---
title: HSCTF 2021 - extended-fibonacci-sequence-2 | Algorithm
published: true
---

## [](#header-2)extended-fibonacci-sequence-2

[Extended Fibonacci Sequence 2 ACTUALLY ACTUALLY ACTUALLY NEW.pdf](https://github.com/DamoNeer/hacker-blog/files/6675294/Extended.Fibonacci.Sequence.2.ACTUALLY.ACTUALLY.ACTUALLY.NEW.pdf)

![image](https://user-images.githubusercontent.com/81070073/122521840-27064d80-cfca-11eb-8dd9-e46e4a9b5830.png)

### [](#header-3)Solution

This challenge is very similar to the previous version of the challenge. There are three differences.

1. The initial Fib numbers have changed.
2. S and F no longer concatenates together but rather they are added together
3. Each question is timed.

This time, I tried using the pwntools module instead of netcat module and I have to admit that I liked it much better than netcat.

After using a while true loop, I realized that I will get the flag after answering correctly 15 times.

Here is my script:

```
from pwn import *


def crack(n):
    fibonacci_numbers = [4, 5]  # Initialize the first two numbers
    for i in range(2, n + 1):               # Put the first n fib numbers in a list
        fibonacci_numbers.append(
            fibonacci_numbers[i-1]+fibonacci_numbers[i-2])

    S_list = []
    s = 0
    for i in range(0, n+1):                 # Put the first n S numbers in a list
        # Addition between Si and Fi
        s = s + (fibonacci_numbers[i])
        S_list.append(s)

    # Get the last 10 digits of the sum and remove leading zeroes
    answer = (str(sum(S_list))[-10:])
    answer = answer.lstrip("0")
    answer += "\n"
    out = str.encode(str(answer))
    return out


conn = remote('extended-fibonacci-sequence-2.hsc.tf', 1337)
conn.recvuntil(b'!\n')
string = conn.recvline()
n = int(string.decode("utf-8"))
print(n)  # 1
conn.send(crack(n))
counter = 2
for i in range(14):
    print("Attempt: " + str(counter))
    print(conn.recvline())
    print(conn.recvline())
    conn.recvuntil(b'!\n')
    string = conn.recvline()
    print(string)
    n = int(string.decode("utf-8"))
    print(crack(n))
    conn.send(crack(n))
    counter += 1
print(conn.recvline())
print(conn.recvline())
print(conn.recvline())
print(conn.recvline())
```

flag{i_n33d_a_fl4g._s0m3b0dy_pl3ase_giv3_m3_4_fl4g.}
