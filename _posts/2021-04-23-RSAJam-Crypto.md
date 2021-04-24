---
title: HTB Cyber Apocalypse 2021 - RSA Jam | Crypto
published: true
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
3. d2 must be able to decrypte just as well as d

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

There is actually another method to solve this, but it's not as reliable in my opinion.

Rather than solving d2 from phi, I tried solving bruteforcing k first then phi and d2.

This method surrounds the following formula:

ed = k * phi + 1

phi = (e * d -1)//k

```
import random
e = 65537
d = 21449697719230116087273950325996171573130106195873481685360452811261935641972975903038202587302168977183228465904066286969470722947662588600816360073429193087776517146775827925592501250792179016703659210189164131057348950849586823605300862872193412703302122087610702786409053047356196415374032392836389673473
N = 94656847311641244226764048381577745363155866255401007959966870641146958195945250943196733079524762524924735301996821240934496180043159589868136946342490696170787924628554741985248815341343863284657657996685888523595653292790392833859235716830054061099986062945989369893225305862457763043012238438289062378701
x = 1

for k in range(10000, 60000):       #That's what k usually is at after trying a couple times

    if x == 1:

        phi = (e*d-1)//k

        phi2 = phi // 2

        d2 = d+ phi2 %N

        for i in range(1):

            if d2 != d:
                if 0 <= d2 < phi:

                    for _ in range(1):
                        m = random.randrange(N)
                        c = pow(m, e, N)
                        if m == pow(c, d2, N):
                            print("Correct! Your d2 value is: " + str(d2))
                            print("The k value you used is: "+ str(k))
                            x = 2

                        else:
                            print(k)
                            continue
                else:
                    print("d2 is too big!")
                    
```

I actually did get the flag from this method once, but it was based on luck because my script luckily guessed the right k value.

**It's much more reliable to aim for the prime factors of N which leads to the phi value directly (without having to guess k), and get d2.**

Flag = CHTB{lambda_but_n0t_lam3_bda}
