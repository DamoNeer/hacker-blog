---
title: HSCTF 2021 - Hopscotch | Programming
published: true
---

## [](#header-2)Hopscotch

Keith wants to play hopscotch, but in order to make things interesting, 
he decides to use a random number generator to decide the number of squares n 
to draw for a round of hopscotch. 
He then creates a hopscotch board on the floor by randomly creating a 
sequence of ones (one square) and twos (two squares) such that 
the sum of all the numbers in the sequence is n. 

Given 1 <= n <= 1000, find the number of valid hopscotch boards (mod 10000) 
he can create.

Sample Input: 5 Sample Output: 8

![image](https://user-images.githubusercontent.com/81070073/122608968-08d43800-d032-11eb-97b6-e7490fb25eb7.png)

### [](#header-3)Solution

So exactly how did an input of 5 become 8 ? I used Python to create a "wordlist" that consists of "1" and "2" and I restricted the length of string to go from 1 to input. What I am doing here is the same as creating a password wordlist with provided characters and password length except in this case that the characters are only "1", "2" and the password is less than or equal to the input.

In the example of 5, I expect to get all of these combinations. Then, I will tell the program to compare the sum of each list with the input number and increase the counter by one each time it finds one.
```
Example input = 5
------------------

[1]
[2]
[1, 1]
[1, 2]
[2, 1]
[2, 2]
[1, 1, 1]
[1, 1, 2]
[1, 2, 1]
[1, 2, 2]
[2, 1, 1]
[2, 1, 2]
[2, 2, 1]
[2, 2, 2]
[1, 1, 1, 1]
[1, 1, 1, 2]
[1, 1, 2, 1]
[1, 1, 2, 2]
[1, 2, 1, 1]
[1, 2, 1, 2]
[1, 2, 2, 1]
[1, 2, 2, 2]
[2, 1, 1, 1]
[2, 1, 1, 2]
[2, 1, 2, 1]
[2, 1, 2, 2]
[2, 2, 1, 1]
[2, 2, 1, 2]
[2, 2, 2, 1]
[2, 2, 2, 2]
[1, 1, 1, 1, 1]
[1, 1, 1, 1, 2]
[1, 1, 1, 2, 1]
[1, 1, 1, 2, 2]
[1, 1, 2, 1, 1]
[1, 1, 2, 1, 2]
[1, 1, 2, 2, 1]
[1, 1, 2, 2, 2]
[1, 2, 1, 1, 1]
[1, 2, 1, 1, 2]
[1, 2, 1, 2, 1]
[1, 2, 1, 2, 2]
[1, 2, 2, 1, 1]
[1, 2, 2, 1, 2]
[1, 2, 2, 2, 1]
[1, 2, 2, 2, 2]
[2, 1, 1, 1, 1]
[2, 1, 1, 1, 2]
[2, 1, 1, 2, 1]
[2, 1, 1, 2, 2]
[2, 1, 2, 1, 1]
[2, 1, 2, 1, 2]
[2, 1, 2, 2, 1]
[2, 1, 2, 2, 2]
[2, 2, 1, 1, 1]
[2, 2, 1, 1, 2]
[2, 2, 1, 2, 1]
[2, 2, 1, 2, 2]
[2, 2, 2, 1, 1]
[2, 2, 2, 1, 2]
[2, 2, 2, 2, 1]
[2, 2, 2, 2, 2]
```

After comparing the sum to the input, which is 5 in this example.

```
[1, 2, 2]
[2, 1, 2]
[2, 2, 1]
[1, 1, 1, 2]
[1, 1, 2, 1]
[1, 2, 1, 1]
[2, 1, 1, 1]
[1, 1, 1, 1, 1]
counter: 8
```
Unfortunately, I cannot use this as the general solution to this challenge because creating a wordlist of 1000 character length takes way too long. However, applying this logic to the first 10 inputs from 1 to 10, I have noticed that the results follow the Fibonacci sequence!

```
import itertools
from pwn import *


def realization():

    chars = '12'
    list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]                 # Inputs
    output = []
    for input in list:
        counter = 0
        for i in range(1, input+1):
            for xs in itertools.product(chars, repeat=i):
                xs = [int(j) for j in xs]
                if sum(xs) == input:
                    counter += 1
        output.append(counter)
    print(output)


realization()

               Output
          -----------------
[1, 2, 3, 5, 8, 13, 21, 34, 55, 89]       #Fibonacci Sequence! Input 5 does give the ouput 8.
```

After that, I just used the same approach as the previous two Fibanocci challenges and made a script to answer every question correctly. In order to get the flag, I need to answer 20 times correctly this time.

```
import itertools
from pwn import *


def realization():

    chars = '12'
    list = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    output = []
    for input in list:
        counter = 0
        for i in range(1, input+1):
            for xs in itertools.product(chars, repeat=i):
                xs = [int(j) for j in xs]
                if sum(xs) == input:
                    counter += 1
        output.append(counter)
    print(output)


def crack(n):
    fibonacci_numbers = [1, 2]  # Initialize the first two numbers
    for i in range(2, n + 1):               # Put the first n fib numbers in a list
        fibonacci_numbers.append(
            fibonacci_numbers[i-1]+fibonacci_numbers[i-2])
    answer = str(int(fibonacci_numbers[n-1]) % 10000)
    answer = answer.lstrip("0")
    answer += "\n"
    out = str.encode(str(answer))
    return out


conn = remote('hopscotch.hsc.tf', 1337)
string = conn.recvline()
string = conn.recvline()
n = int(string.decode("utf-8"))
print(n)  # 1
conn.send(crack(n))
counter = 2
for i in range(19):
    print("Attempt: " + str(counter))
    counter += 1
    string = conn.recvline()
    string = (string.decode("utf-8"))
    n = int(string.replace(": ", ''))
    print(n)
    conn.send(crack(n))
string = conn.recvline()
print(string.decode("utf-8"))

```
flag{wh4t_d0_y0U_w4nt_th3_fla5_t0_b3?_'wHaTeVeR_yOu_wAnT'}



