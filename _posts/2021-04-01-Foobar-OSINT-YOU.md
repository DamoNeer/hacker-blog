---
title: Foobar CTF 2021 - YOU, HOME  | OSINT
published: true
---

## [](#header-2)YOU

> h4x0rl4dy - "Oversized shades, floral shirt, and a lean physique. That is all I remembered from our first meeting. A paradise on Earth they say, help me locate the place where we first met
Flag format : GLUG{latitude_longitude} (upto 3 decimal places).


### [](#header-3)Solution
I immediately googled h4x0rl4dy and it turns out to be she has an Instagram account and a Twitter account. 
Her instagram account has two posts and a story named **paradise**.

![image](https://user-images.githubusercontent.com/81070073/113341987-c2005d00-92e2-11eb-9907-9e08f37f532a.png)

Taking a screenshot of her story and doing it a reverse image search would reveal that she was in Constance Ephelia, which is a beautiful resort.

![image](https://user-images.githubusercontent.com/81070073/113342142-f7a54600-92e2-11eb-9865-8b149d5ef1e3.png)

Navigating through Google map pictures of the place, one can see that the latitude and longitude are provided in the URL.

![image](https://user-images.githubusercontent.com/81070073/113342519-74382480-92e3-11eb-81c9-74c13d08a437.png)

Flag: GLUG{-4.659_55.405}

Note that the correct flag is 0.001 away from what the Google map says even though the pictures are identical. 
**The main lesson here is that I should always try answers that are +/- 0.001 from what I see.**

## [](#header-2)HOME

> h4x0rl4dy - That is the last I saw of him. The only link to him is his address. He gave it to me but I cannot find it. Help me!

FLAG FORMAT - GLUG{address}

### [](#header-3)Solution
This new prompt doesn't really give us additional information and we are back to square one. 
This is all we know so far about her:
- Real name: Kristina Parker 
- Handle: h4x0rl4dy
- Lives in Pasadena, California (From one of her two posts on Instagram)
- We are looking for a **link**

Let's do more digging...

Using [https://www.peekyou.com/username=h4x0rl4dy/](peekyou) with the username input "h4x0rl4dy", I was able to see that she has a valid **GitHub** and Reddit account besides Instagram and Twitter. 

![image](https://user-images.githubusercontent.com/81070073/113347833-96817080-92ea-11eb-9889-44c6303de761.png)

From her [https://github.com/h4x0rl4dy](github), I found out that she went to _Caltech_ and that there is a mysterious short link (UTDgPMGE) underneath it. Her commits in her repository also told me that she has a valid email: h4x0rl4dy@gmail.com and that matches with what we found on peekyou already.

![image](https://user-images.githubusercontent.com/81070073/113347752-7d78bf80-92ea-11eb-83ad-45b7cf5c23d9.png)

The short link was actually used for [https://pastebin.com/UTDgPMGE](pastebin) and it led us to a locked document, so we need to find more about her in order to find the correct password.

![image](https://user-images.githubusercontent.com/81070073/113349493-fc6ef780-92ec-11eb-8ceb-aa7a91ee4ef1.png)

Doing a quick google search of "Kristina Parker + Caltech", I see that she has a [https://www.linkedin.com/in/kristina-parker-366629206/](Linkedin).

She has a OSINT PPT and there is a link on the last page of it (She purposefully put a space between 5 and 7 to make people think that the page doesn't exit)
https://gist.github.com/h4x0rl4dy/b32511c29ef72700fbe57c90e5e857ac

![image](https://user-images.githubusercontent.com/81070073/113348706-cbda8e00-92eb-11eb-8919-f966f45c34ae.png)

There is a wordlist and a hash.

![image](https://user-images.githubusercontent.com/81070073/113348977-3095e880-92ec-11eb-872c-12f3cb0d9d6b.png)

The wordlist is fairly short. There is a chance that the password contains 2-3 of these words here, so I combined the wordlist with itself two times before cracking the hash with hashcat.

```Linux
\\Linux command
$/usr/share/hashcat-utils/combinator.bin wordlist.txt wordlist.txt > wordlistx2.txt
$hashcat -m 0 -a 1 hash wordlistx2.txt wordlist.txt

df6584e59b2f57173db5191fa6ed84ac:h4x0r69loves
```

Entering the password into pastebin, I received the following:

![image](https://user-images.githubusercontent.com/81070073/113352314-eb27ea00-92f0-11eb-99cd-181148753792.png)

Flag: GLUG{10880_Malibu_Point}
