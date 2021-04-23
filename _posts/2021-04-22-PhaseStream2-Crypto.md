---
title: HTB Cyber Apocalypse 2021 - PhaseStream 2	 | Crypto
published: true
---

## [](#header-2)PhaseStream 2

> The aliens have learned of a new concept called "security by obscurity". 

> Fortunately for us they think it is a great idea and not a description of a common mistake. 

> We've intercepted some alien comms and think they are XORing flags with a single-byte key and hiding the result inside 9999 lines of random data.

> Can you find the flag?

> This challenge will raise 33 euros for a good cause.

> Downloadable text file: [given.txt](https://github.com/DamoNeer/hacker-blog/files/6362837/given.txt)

### [](#header-3)Solution

Wow, that's a lot of encrypted messages. The prompt mentioned that one of them is going to be the flag and it's only xor'ed with a single byte.

It is extremely easy to brute force the key if it's only a single byte. 

The following script will go through each one on the list and see if each one contains the flag format.

I had to implement a bruteforce function within another bruteforce function because the one above can't interpret all of the outputs while

the one below somehow prioritizes the key that provides partial, gibberish output.

```
file = open("given.txt", "r")
lines = file.readlines()

def bruteforce(input):
    from pwn import xor

    flagBytes = bytes.fromhex(input)
    byte = 0x00

    for i in range(256):
        flag = xor(flagBytes, byte).decode("utf-8")
        if ("CHTB{" in flag):
            print(flag)
            break;
        byte = byte + 0x01

for line in lines:

    input_str = bytes.fromhex(line)

    key = input_str[0] ^ ord('c')

    output = ''.join(chr(c ^ key) for c in input_str)

    if "chtb" in output:
        bruteforce(line)

    elif "CHTB" in output:
        bruteforce(line)
    else:
        continue
        
        

#Flag: CHTB{n33dl3_1n_4_h4yst4ck}
```

