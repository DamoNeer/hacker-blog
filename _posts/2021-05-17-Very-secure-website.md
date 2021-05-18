---
title: dCTF 2021 - Very secure website | Web
published: true
---

## [](#header-2)Very secure website

> Some students have built their most secure website ever. Can you spot their mistake?

> http://dctf1-chall-very-secure-site.westeurope.azurecontainer.io/

### [](#header-3)Solution

![image](https://user-images.githubusercontent.com/81070073/118605161-d3a2a480-b76a-11eb-8c4b-01021ed09510.png)

Clicking the source code gives the following:

![image](https://user-images.githubusercontent.com/81070073/118605223-e321ed80-b76a-11eb-8ca9-982edb4cabfb.png)

There is [php code vulnerability](https://www.whitehatsec.com/blog/magic-hashes/) about using the operators "!=" and "==".

The "51c3f5f5d8a8830bc5d8b7ebcb5717df" hash is easy to figure out. With a simple Google search, it's revealed to be "admin". The password requires a bit more research.

Long story short, it turns out I don't need to find out the identity of the tiger128,4 hash: "0e132798983807237937411964085731".

I just need to use one of the "magic hashes" which also begin with 0e or 00e and I just need to pass that on as the password.

![image](https://user-images.githubusercontent.com/81070073/118605784-7f4bf480-b76b-11eb-8a15-274c18eb2cf9.png)

![image](https://user-images.githubusercontent.com/81070073/118605828-8bd04d00-b76b-11eb-94f0-de9eb76f23fe.png)
