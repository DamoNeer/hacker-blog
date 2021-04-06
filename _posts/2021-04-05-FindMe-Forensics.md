---
title: ShaktiCon CTF 2021 - Find me  | Forensics
published: true
---

## [](#header-2)Find me

> We found that there was a secret communication between two criminals. Can you find out the secret information?

> Downloadable: _network2.pcap_

### [](#header-3)Solution
I opened it on Wireshark and checked the TCP stream. There were two meaningful TCP stream.

![image](https://user-images.githubusercontent.com/81070073/113672955-36355a80-966d-11eb-900c-567a0306b60f.png)

These seem like ASCII code, so I converted them into string and I got a base64 string.

![image](https://user-images.githubusercontent.com/81070073/113673121-6c72da00-966d-11eb-929f-caed4962e8d7.png)

Then, I decoded it and got **n0tth4tea5y**

![image](https://user-images.githubusercontent.com/81070073/113672991-42b9b300-966d-11eb-9c6a-61cd8c3729b3.png)

Moving on to the second TCP stream, I see the word "flag.txt" backward in there. So I decided to reverse this on Cyberchef and retrieve it after. To do that, I need to 
first paste its raw form, which is in hex, on Cyberchef.

![image](https://user-images.githubusercontent.com/81070073/113673617-12bedf80-966e-11eb-9ec6-f769a2c9bb26.png)

After detecting that the file output is a zip file, I deselected that recipe and save the output to a zip file. 

![image](https://user-images.githubusercontent.com/81070073/113673711-2d915400-966e-11eb-80e6-6354cf091fe3.png)

Finally, I used the decoded base64 string as password for the zip file and retrieved the flag: **shaktictf{g00d_lUcK_4_tH3_n3xT_cH411eNg3}**
