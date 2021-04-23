---
title: HTB Cyber Apocalypse 2021 - Serial Logs | Hardware
published: false
---

## [](#header-2)Serial Logs

> We have gained physical access to the debugging interface of the Access Control System which is based on a Raspberry Pi-based IoT device. 

> We believe that the log messages of this device contain valuable information of when our asset was abducted

> Downloadable: logicdata.sal file

### [](#header-3)Solution

![image](https://user-images.githubusercontent.com/81070073/115931860-be26ad00-a440-11eb-87c9-4fc2291650ea.png)

As shown in this picture, the bit rate will be 1 / (8.48 * 10 ^ -6) bit/second = 117925 bit/s

Logic Analyzer Software: Logic 2 Saleae

Analyzer: Async Serial with 117925 bit rate with the rest being standard settings

Clear text is shown in in the data tab, so that's a really good sign that I used the correct analyzer.

While parsing through the data, I have noticed that the readable data will change from LOG Connection to an error that indicates a change in baud rate is coming up.

Due to the baud rate being changed, the data that comes after it will become gibberish again.

In order to fix that, I had to recalculate the baud rate.

![image](https://user-images.githubusercontent.com/81070073/115932189-4d33c500-a441-11eb-9bcd-a2bbefbd6cd4.png)

As shown in this picture, the bit rate will be 1 / (13.46 * 10 ^ -6) bit/second = 74294 bit/s

The flag is located at the last few lines of the data.

![image](https://user-images.githubusercontent.com/81070073/115932384-b0255c00-a441-11eb-8e82-c1a4963884c0.png)
