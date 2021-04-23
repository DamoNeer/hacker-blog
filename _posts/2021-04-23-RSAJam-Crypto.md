---
title: HTB Cyber Apocalypse 2021 - RSA Jam | Crypto
published: false
---

## [](#header-2)RSA Jam

> Even aliens have TLA agencies trying to apply rubber hose cryptanalysis.

> Given Script

```
from Crypto.Util.number import getPrime, inverse
import random

def main():
    print("They want my private key, but it has sentimental value to me. Please help me and send them something different.")
    p = getPrime(512)
    q = getPrime(512)
    N = p*q
    phi = (p - 1) * (q - 1)
    e = 0x10001
    d = inverse(e, phi)
    print({'e': e, 'd': d, 'N': N})
    d2 = int(input("> "))
    assert d2 != d
    assert 0 <= d2 < phi
    with open("flag.txt") as f:
        flag = f.read()
    random.seed(str(d2) + flag)
    for _ in range(50):
        m = random.randrange(N)
        c = pow(m, e, N)
        assert m == pow(c, d2, N)
    print(flag)

if __name__ == "__main__":
    import os
    os.chdir(os.path.abspath(os.path.dirname(__file__)))
    main()

```

### [](#header-3)Solution

I have spent almost 24 hours on this question. This was the hardest Crypto question that I have done so far.

The script will give e (65537), d (randomly generated), and N (randomly generated).

The script asks for d2 value.

This is a pretty standard RSA problem. What's special about this is that they are looking for a second d value (private exponent) that falls into these categories:

1. d2 must be positive and be less than phi
2. d2 must not be the same as d
3. d2 must be able to decrypte just as well d

There is a quick and dirty way to find d2 here.

d2 = (d + phi//2) % phi

In order to get phi, we are going to need to find the prime factors of N. 

Thankfully, someone has already figured out how to find the prime factors of N. 

The first three functions are written by that [person](https://crypto.stackexchange.com/questions/6361/is-sharing-the-modulus-for-multiple-rsa-key-pairs-secure)

```
import random
import math

def remove_even(n):
    if n == 0:
        return (0, 0)
    r = n
    t = 0
    while (r & 1) == 0:
        t = t + 1
        r = r >> 1
    return (r, t)

def get_root_one(x, k, N):
    (r, t) = remove_even(k)
    oldi = None
    i = pow(x, r, N)
    while i != 1:
        oldi = i
        i = (i*i) % N
    if oldi == N-1:
        return None
    return oldi

def factor_rsa(e,d,N):
    k = e*d -1
    y = None
    while not y:
        x = random.randrange(2,N)
        y = get_root_one(x,k,N)
    p = math.gcd(y-1, N)
    q = N // p
    return(p,q)

N = 80015342967776525157771298882652731543325431238477846852879960793372048079302510420873059995091650359518023195824919788496576681421565566215922442597408633856150721107156382663198624561803704994636747344091365774180607052080439049206373238695998395536783532731246942107363312513507676712384094208973298472599
e = 65537
d = 63072655411680963267321746498586143880986187615847011129892713651610540668275442701715096500395725431019135424207933782042711008472131424244542066078430948884485749347815664303581546934987122110022769904523178801991978026094213277615386004898820209792569922458089898586948684964653209488818065495186959497953

def find_d2(p, q):
    d = 63072655411680963267321746498586143880986187615847011129892713651610540668275442701715096500395725431019135424207933782042711008472131424244542066078430948884485749347815664303581546934987122110022769904523178801991978026094213277615386004898820209792569922458089898586948684964653209488818065495186959497953
    phi = (p-1)*(q-1)
    d2 = (d+phi//2) % phi
    print("d2: "+ str(d2))

p, q = factor_rsa(e,d,N)
find_d2(p, q)

#d2 = 23064983927792700688436097057259778109323471996608087703452733254924516628624187491278566502849900251260123826295473887794422667761348641136580844779726640937887389349675950874779535306277762627593789346715052890782645235771560784631650497707047270485798193769202580703742568370300931224506513439843678771885

```

Flag = CHTB{lambda_but_n0t_lam3_bda}
