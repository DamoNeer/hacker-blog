---
title: Foobar CTF 2021 - YOU | OSINT
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

Flag: GLUG{-4.659, 55.405}

Note that the correct answer is 0.001 away from what the Google map says even though the pictures are identical. 
**The main lesson here is that I should always try answers that are +/- 0.001 from what I see.**
