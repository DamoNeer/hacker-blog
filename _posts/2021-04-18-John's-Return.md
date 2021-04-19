---
title: UMDCTF 2021 - John's Return | Forensics
published: true
---

## [](#header-2) John's Return

> I received this network traffic from John, but I don't what he's trying to say? Can you figure it out?

> Downloadable: received.pcapng

### [](#header-3)Solution

![image](https://user-images.githubusercontent.com/81070073/115177732-acca5300-a084-11eb-87b9-89bd7ec5efbd.png)

Within the first ten packets, I see that there is the 4 way handshake, which hints that we need to crack the wifi password in order to continue.

I will be using the tool **aircrack-ng** to crack the password, but I must first convert the file format from pcapng to pcap and I can't just change the file extension.

I have to use a tool like this: [website](https://pcapng.com/)

```
//Linux

$aircrack-ng received.s0i0.pcap -w /usr/share/wordlists/rockyou.txt

Reading packets, please wait...
Opening received.s0i0.pcap
Read 76 packets.

   #  BSSID              ESSID                     Encryption

   1  00:23:69:AA:69:F5  linksys                   WPA (0 handshake, with PMKID)

Choosing first network as target.

Reading packets, please wait...
Opening received.s0i0.pcap
Read 76 packets.

1 potential targets


                               Aircrack-ng 1.6 

      [00:00:00] 150/10303727 keys tested (2226.68 k/s) 

      Time left: 1 hour, 17 minutes, 7 seconds                   0.00%

                           KEY FOUND! [ chocolate ]


      Master Key     : F8 FA E8 1B 93 20 46 CD F5 7C 0B 7C D0 70 0C 11 
                       F5 70 AD 93 5B 9C 91 97 8D 44 18 70 76 56 5E B6 

      Transient Key  : 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 

      EAPOL HMAC     : 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 

```

After knowing that the password is **chocolate**, we will return to Wireshark and put that as the password.

"Edit" > "Preferences" > "Protocols" > "IEEE 802.11 wireless LAN" > "Decryption key Edit" > select "wpa-pwd" and insert "chocolate" as key > press "OK" to see the changes.

![image](https://user-images.githubusercontent.com/81070073/115178173-97095d80-a085-11eb-8f0a-5ba0ebb78a14.png)

This time, there are a bunch of "No valid function found". However, if we look closely to the data...

![image](https://user-images.githubusercontent.com/81070073/115178466-2e6eb080-a086-11eb-9bbb-fc05b0f6ec10.png)

Flag: UMDCTF-{wh3r3_j0 hn}
