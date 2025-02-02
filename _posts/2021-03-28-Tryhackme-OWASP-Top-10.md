---
title: Tryhackme - OWASP Top Ten | Web
published: true
---

## [](#header-2)Severity 1: Command Injection Practical

> What strange text file is in the website root directory?

### [](#header-3)Solution
![image](https://user-images.githubusercontent.com/81070073/112799398-24c3d100-9023-11eb-898a-ee01618b4f0b.png)

**ls** will show the contents inside the directory and we see that "drpepper.txt" is the strange txt file

> How many non-root/non-service/non-daemon users are there?

### [](#header-3)Solution
![image](https://user-images.githubusercontent.com/81070073/112800278-4cffff80-9024-11eb-9803-245029bdc13b.png)

There are 0 non-root/non-service/non-daemon users.

> What user is this app running as?

### [](#header-3)Solution
![image](https://user-images.githubusercontent.com/81070073/112800478-8e90aa80-9024-11eb-9040-6fb032f89d41.png)

> What is the user's shell set as?

### [](#header-3)Solution
![image](https://user-images.githubusercontent.com/81070073/112800768-f1824180-9024-11eb-9fa8-2c1eb1d725c6.png)

> What version of Ubuntu is running?

### [](#header-3)Solution
![image](https://user-images.githubusercontent.com/81070073/112801944-6144fc00-9026-11eb-87de-29d7b68ae136.png)

> Print out the MOTD. What favorite beverage is shown?

### [](#header-3)Solution
![image](https://user-images.githubusercontent.com/81070073/112801538-dcf27900-9025-11eb-9cb3-1c976565bcd2.png)

To view the message of the day (MOTD), I went to _/etc/update-motd.d_ directory and open the _00-header file_ 

## [](#header-2)Severity 2: Broken Authentication Practical

>  What is the flag that you found in darren's account?
>  Now try to do the same trick and see if you can login as arthur.

### [](#header-3)Solution

To view the flags, I need to login as the existing user. I was able to register as the user again by adding a space character in front of their usernames. 

![image](https://user-images.githubusercontent.com/81070073/112933700-35348400-90d5-11eb-9bba-be2387df11ba.png)

![image](https://user-images.githubusercontent.com/81070073/112933566-f56d9c80-90d4-11eb-99de-2604b2b81da5.png)

