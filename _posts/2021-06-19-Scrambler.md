---
title: HSCTF 2021 - Scrambler | Rev
published: false
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

