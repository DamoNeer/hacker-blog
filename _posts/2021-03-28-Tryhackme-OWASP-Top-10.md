---
title: Tryhackme | OWASP Top Ten
published: true
---

## [](#header-2)Severity 1: Command Injection Practical

> What strange text file is in the website root directory?

### [](#header-3)Solution
![image](https://user-images.githubusercontent.com/81070073/112799398-24c3d100-9023-11eb-898a-ee01618b4f0b.png)

**whoami** will show that we are indeed at the website root directory already: _www-data_

**ls** will show the contents inside the directory and we see that "drpepper.txt" is the strange txt file

> How many non-root/non-service/non-daemon users are there?

### [](#header-3)Solution
![image](https://user-images.githubusercontent.com/81070073/112800278-4cffff80-9024-11eb-9803-245029bdc13b.png)

There are 0 non-root/non-service/non-daemon users.
