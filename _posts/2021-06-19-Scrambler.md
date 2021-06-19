---
title: HSCTF 2021 - Scrambler | Rev
published: true
---

## [](#header-2)Scrambler

Output: QQ.ap(OOOaQa.a($/FF.F/ZOZ..aQaQ/aa(a(ZZaaF.Q^FaZ,S/..(^]aF.FF.pFF-(==SFa/aOo.=/aFM/,.,Z=/aF/aQ/*<Q(^-OSZOO=a]Q](Q<].a~/ao/aYZaF=aQa]Q^FOZFsQn/^FOh.*aZ.OaaO/(Q.SZ</ QQ,a(OFaFF((QQQaQQ/^/Q.O-F(Z(=gQQ=k,OSF=F.a/]Q=Z(Qa(ao=a:ZQ/QpJ]/QQF.=FZ]QkFS^=Q:QQZQFa=."OS(=^Q.^Ja/(/Z^]F:]//./.Q=F=Ya/SO/]Oas=apS=(..)(.aF/(oZ(a/~.,,ZZZ/Oq=(.QF":.|O($FZ./(]]FO]FO.Oo"F+QO/FqY/Z-(a.=/F/aa/.=OZOFQ(=Z./pOa((O]..Q/]Q((a(]/aaSZJ.Q(*F]<//Fa/|]QFQZ(=S.ZQQZOFQa:Q/aQO=(]..a/^(QOQoF////(^kF-a-


Source code:

```
# uncompyle6 version 3.7.4
# Python bytecode 3.8 (3413)
# Decompiled from: Python 2.7.17 (default, Sep 30 2020, 13:38:04) 
# [GCC 7.5.0]
# Warning: this version of Python has problems handling the Python 3 "byte" type in constants properly.

# Embedded file name: chall.py
# Compiled at: 2021-04-15 21:21:48
# Size of source mod 2**32: 860 bytes
import random, time
with open('flag.txt') as (f):
    flag = list(f.read())
if len(flag) % 2 == 1:
    flag.append(' ')
x = ['t', 'Y', 'w', 'V', '|', ']', 'u', 'X', '_', '0', 'P', 'k', 'h', 'D', 'A', '4', 'K', '5', 'z',
 'Z', 'G', '7', ';', 'S', ' ', '/', '6', '%', '}', '\\', ',', ':', '>', '#', 'a', '$', '3', '`',
 '+', 'R', 'b', 'H', 'd', 's', '1', 'J', 'L', 'v', '9', '2', 'o', 'M', '<', 'e', '(', 'x', '-',
 'B', 'm', "'", 'y', 'Q', '"', 'W', 'l', '.', 'i', 'O', '^', 'p', '8', 'f', 'F', 'C', '?', 'g',
 '@', 'j', '[', 'r', '!', '=', 'E', '~', '*', 'T', '{', ')', 'U', 'N', 'c', '&', 'n', 'q', 'I']
random.seed(int(time.time()))
for _ in range(20):
    for i in range(len(flag)):
        flag[i] = x[(ord(flag[i]) - 32)]

else:
    random.shuffle(flag)

for i in range(0, len(flag), 2):
    flag[i], flag[i + 1] = flag[(i + 1)], flag[i]
else:
    print(''.join(flag))
```

### [](#header-3)Solution

I used an online decompiler for this. https://www.decompiler.com/ which told me the compiled time (which is used to determine the seed).

This script will first convert the flag into a list and add a space in the end if its length is not even.

Then, it will create a seed based on the current epoch time.

Next, for every character in the flag, it will replace it with the corresponding character in the x list after moving 32 places to the left.

After that it will shuffle the flag based on the seed.

Finally, it will switch the characters in pairs and convert the flag from list to string.. Ex: abcd -> badc

Here is my script that will basically reverse the steps it took to convert the flag to gibberish.

Unshuffling with known seed is probably the hardest step in my opinion. 

Credits to the source of my unshuffle function: https://stackoverflow.com/questions/57340713/how-do-i-un-shuffle-a-list-back-to-its-original-form

```
import random

x = ['t', 'Y', 'w', 'V', '|', ']', 'u', 'X', '_', '0', 'P', 'k', 'h', 'D', 'A', '4', 'K', '5', 'z',
     'Z', 'G', '7', ';', 'S', ' ', '/', '6', '%', '}', '\\', ',', ':', '>', '#', 'a', '$', '3', '`',
     '+', 'R', 'b', 'H', 'd', 's', '1', 'J', 'L', 'v', '9', '2', 'o', 'M', '<', 'e', '(', 'x', '-',
     'B', 'm', "'", 'y', 'Q', '"', 'W', 'l', '.', 'i', 'O', '^', 'p', '8', 'f', 'F', 'C', '?', 'g',
     '@', 'j', '[', 'r', '!', '=', 'E', '~', '*', 'T', '{', ')', 'U', 'N', 'c', '&', 'n', 'q', 'I']


def unshuffle(l, seed):
    random.seed(seed)
    perm = list(range(len(l)))
    random.shuffle(perm)
    res = [None] * len(l)
    for i, j in enumerate(perm):
        res[j] = l[i]
    l[:] = res


test = 'QQ.ap(OOOaQa.a($/FF.F/ZOZ..aQaQ/aa(a(ZZaaF.Q^FaZ,S/..(^]aF.FF.pFF-(==SFa/aOo.=/aFM/,.,Z=/aF/aQ/*<Q(^-OSZOO=a]Q](Q<].a~/ao/aYZaF=aQa]Q^FOZFsQn/^FOh.*aZ.OaaO/(Q.SZ</ QQ,a(OFaFF((QQQaQQ/^/Q.O-F(Z(=gQQ=k,OSF=F.a/]Q=Z(Qa(ao=a:ZQ/QpJ]/QQF.=FZ]QkFS^=Q:QQZQFa=."OS(=^Q.^Ja/(/Z^]F:]//./.Q=F=Ya/SO/]Oas=apS=(..)(.aF/(oZ(a/~.,,ZZZ/Oq=(.QF":.|O($FZ./(]]FO]FO.Oo"F+QO/FqY/Z-(a.=/F/aa/.=OZOFQ(=Z./pOa((O]..Q/]Q((a(]/aaSZJ.Q(*F]<//Fa/|]QFQZ(=S.ZQQZOFQa:Q/aQO=(]..a/^(QOQoF////(^kF-a-'
test_list = list(test)

for i in range(0, len(test_list), 2):  # For every even number between 0 and the length of flag
    # Switch characters in pairs
    test_list[i], test_list[i + 1] = test_list[(i + 1)], test_list[i]

# print(test_list)

for seed in range(1618405140, 1618491540): # The entire day of 4/15/2021 in GMT
    #I hardcoded this list because I don't want the unshuffling process to be recursive.
    shuffled_list = ['Q', 'Q', 'a', '.', '(', 'p', 'O', 'O', 'a', 'O', 'a', 'Q', 'a', '.', '$', '(', 'F', '/', '.', 'F', '/', 'F', 'O', 'Z', '.', 'Z', 'a', '.', 'a', 'Q', '/', 'Q', 'a', 'a', 'a', '(', 'Z', '(', 'a', 'Z', 'F', 'a', 'Q', '.', 'F', '^', 'Z', 'a', 'S', ',', '.', '/', '(', '.', ']', '^', 'F', 'a', 'F', '.', '.', 'F', 'F', 'p', '-', 'F', '=', '(', 'S', '=', 'a', 'F', 'a', '/', 'o', 'O', '=', '.', 'a', '/', 'M', 'F', ',', '/', ',', '.', '=', 'Z', 'a', '/', '/', 'F', 'Q', 'a', '*', '/', 'Q', '<', '^', '(', 'O', '-', 'Z', 'S', 'O', 'O', 'a', '=', 'Q', ']', '(', ']', '<', 'Q', '.', ']', '~', 'a', 'a', '/', '/', 'o', 'Y', 'a', 'a', 'Z', '=', 'F', 'Q', 'a', ']', 'a', '^', 'Q', 'O', 'F', 'F', 'Z', 'Q', 's', '/', 'n', 'F', '^', 'h', 'O', '*', '.', 'Z', 'a', 'O', '.', 'a', 'a', '/', 'O', 'Q', '(', 'S', '.', '<', 'Z', ' ', '/', 'Q', 'Q', 'a', ',', 'O', '(', 'a', 'F', 'F', 'F', '(', '(', 'Q', 'Q', 'a', 'Q', 'Q', 'Q', '^', '/', 'Q', '/', 'O', '.', 'F', '-', 'Z', '(', '=', '(', 'Q', 'g', '=', 'Q', ',', 'k', 'S', 'O', '=', 'F', '.', 'F', '/', 'a', 'Q', ']', 'Z', '=', 'Q', '(', '(', 'a', 'o', 'a', 'a', '=', 'Z', ':', '/', 'Q', 'p', 'Q', ']', 'J', 'Q', '/', 'F', 'Q', '=',
                     '.', 'Z', 'F', 'Q', ']', 'F', 'k', '^', 'S', 'Q', '=', 'Q', ':', 'Z', 'Q', 'F', 'Q', '=', 'a', '"', '.', 'S', 'O', '=', '(', 'Q', '^', '^', '.', 'a', 'J', '(', '/', 'Z', '/', ']', '^', ':', 'F', '/', ']', '.', '/', '.', '/', '=', 'Q', '=', 'F', 'a', 'Y', 'S', '/', '/', 'O', 'O', ']', 's', 'a', 'a', '=', 'S', 'p', '(', '=', '.', '.', '(', ')', 'a', '.', '/', 'F', 'o', '(', '(', 'Z', '/', 'a', '.', '~', ',', ',', 'Z', 'Z', '/', 'Z', 'q', 'O', '(', '=', 'Q', '.', '"', 'F', '.', ':', 'O', '|', '$', '(', 'Z', 'F', '/', '.', ']', '(', 'F', ']', ']', 'O', 'O', 'F', 'O', '.', '"', 'o', '+', 'F', 'O', 'Q', 'F', '/', 'Y', 'q', 'Z', '/', '(', '-', '.', 'a', '/', '=', '/', 'F', 'a', 'a', '.', '/', 'O', '=', 'O', 'Z', 'Q', 'F', '=', '(', '.', 'Z', 'p', '/', 'a', 'O', '(', '(', ']', 'O', '.', '.', '/', 'Q', 'Q', ']', '(', '(', '(', 'a', '/', ']', 'a', 'a', 'Z', 'S', '.', 'J', '(', 'Q', 'F', '*', '<', ']', '/', '/', 'a', 'F', '|', '/', 'Q', ']', 'Q', 'F', '(', 'Z', 'S', '=', 'Z', '.', 'Q', 'Q', 'O', 'Z', 'Q', 'F', ':', 'a', '/', 'Q', 'Q', 'a', '=', 'O', ']', '(', '.', '.', '/', 'a', '(', '^', 'O', 'Q', 'o', 'Q', '/', 'F', '/', '/', '(', '/', 'k', '^', '-', 'F', '-', 'a']
    unshuffle(shuffled_list, seed)
    unshuffled_list = shuffled_list
    for _ in range(20):
        for i in range(len(unshuffled_list)):
            number = x.index(unshuffled_list[i]) + 32
            unshuffled_list[i] = chr(number)
    else:
        joined = ''.join(unshuffled_list)
        if "flag{" in joined:
            print(joined)
            print("seed : ")

seed : 1618514508
```

flag{71me5t4mp_fun}
