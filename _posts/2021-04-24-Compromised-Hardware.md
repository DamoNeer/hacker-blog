---
title: HTB Cyber Apocalypse 2021 - Compromised | Hardware
published: true
---

## [](#header-2)Compromised

> An embedded device in our serial network exploited a misconfiguration which resulted in the compromisation of several of our slave devices in it, leaving the base camp exposed to intruders. 

> We must find what alterations the device did over the network in order to revert them before its too late

> Given: logicdata .sal file 

### [](#header-3)Solution

![image](https://user-images.githubusercontent.com/81070073/115851085-6d309d80-a3db-11eb-8508-d6c83e3315e0.png)

Logic Analyzer Software: Logic 2 Saleae

Analyzer: I2C with Default Settings

Clear text is shown in in the data tab, so that's a really good sign that I used the correct analyzer.

There seem to be two destinations. One is 0x34 and the other one is 0x2C. 

I spent a few minutes to convert the beginning hex to ASCII and I realized that the flag is being written to 0x2C because I saw that the beginning data was CHTB{

The game plan here is to export the data to a txt file for easy parsing. I will write a script to regex match and isolate all of the data that's being sent to 0x2C.

Then, convert each one from hex to ASCII and append the results into one big string.

```
import re
import sys

def read_data():
    phrase = ''
    file = open("data.txt", "r")
    with open('output.log', 'w') as f:
        sys.stdout = f
        lines = file.readlines()
        for line in lines:
            line = line.strip()
            result = re.search(r'write to 0x2C.*', line)
            if result:
                result = re.search(r'ack data: 0x\S\S', result.group(0))
                if result:
                    output = re.search(r'0x\S\S', result.group(0))
                    if output:
                        hex = output.group(0)
                        letter = chr(int(hex, base=16))
                        phrase += letter
                        print(phrase)

read_data()



#CHTB{nu11_732m1n47025_c4n_8234k_4_532141_5y573m!@52)#@%}CHTB{nu11_732m1n47025_c4n_8234k_4_532141_5y573m!@52)#@%}
```
