# Wireless Hacking Part 3


## About

In Part 3 of our Wireless Hacking series, we will delve into the practical application of wireless attacks. Now that we have set up a secure and isolated testing environment in previous sections, it’s time to take a closer look at some of the most common and effective wireless attacks.
These types of attacks are a cornerstone in the world of wireless security testing. From there, we will continue to expand our knowledge by examining other attack techniques such as deauthentication, packet sniffing, and man-in-the-middle (MITM) attacks.

We will begin by exploring spoofing attacks, which can manipulate network traffic and impersonate trusted devices.

> [!CAUTION]
> **Ethical Hacking Reminder**: Only perform wireless penetration tests on networks you own or have explicit permission to test. Unauthorized hacking is illegal and unethical.

## RC4/ Stream Cipher

The process starts when the user creates a root key of size 128 bits. This root key is then encrypted using the RC4 algorithm, which generates a keystream. Once the keystream is generated, it is combined with the plaintext data using an XOR gate (logical operation). The result of this operation gives us the **ciphertext**, which can then be sent.

```mermaid
graph LR
    A[Create 128-bit Root Key] --> B[Encrypt with RC4 Algorithm]
    B --> C[Generate Keystream]
    C --> D[Combine Keystream with Plain Data using XOR Gate]
    D --> E[Get Ciphertext]
    E --> F[Send Ciphertext]
```

## Monitoring

As we continue to explore wireless security in a controlled and isolated environment, we can utilize tools like Wifite2 to audit and capture network data. For example, the following command:

```bash
┌──(proxygeek㉿VMware-kali)-[~]
└─$ sudo wifite --kill
   .               .    
 .´  ·  .     .  ·  `.  wifite2 2.7.0
 :  :  :  (¯)  :  :  :  a wireless auditor by derv82
 `.  ·  ` /¯\ ´  ·  .´  maintained by kimocoder
   `     /¯¯¯\     ´    https://github.com/kimocoder/wifite2

 [+] option: kill conflicting processes enabled
 [+] Using wlan0 already in monitor mode                                                                         
```

This command helps stop conflicting processes (like `NetworkManager` or `wpa_supplicant`), and sets your wireless interface to monitor mode, which is essential for packet sniffing and capturing data from nearby networks. Once in monitor mode, you can analyze various wireless networks in your environment.

Here is a sample of the output you might see when running wifite:

```bash
   NUM                      ESSID   CH  ENCR    PWR    WPS  CLIENT                                               
   ---  -------------------------  ---  -----   ----   ---  ------
     1       (1X:XX:XX:XX:XX:XX)     1  WPA     99db    no                                                       
     2       (2X:XX:XX:XX:XX:XX)    11  WPA     99db    no                                                       
     3              WIFI01-GUEST     8  WPA-P   63db    no                                                       
     4              WIFI02-GUEST     6  WPA-P   38db  lock                                                       
     5       (3X:XX:XX:XX:XX:XX)     6  WPA     38db    no                                                       
     6              WIFI03-GUEST     6  WPA-P   30db  lock                                                       
     7       (4X:XX:XX:XX:XX:XX)     6  WPA     29db    no                                                       
     8       (5X:XX:XX:XX:XX:XX)     1  WPA-P   21db    no                                                       
     9              WIFI04-GUEST     1  WPA-P   20db   yes                                                       
    10              WIFI05-GUEST     6  WPA-P   20db   yes                                                       
    11              WIFI06-GUEST    11  WPA-P   19db  lock
```

In this list, each entry represents a wireless access point (AP) detected within range, with information about its ESSID (network name), channel, encryption type, signal strength (PWR), WPS status, and connected clients.

This tool is essential for understanding and evaluating the security of wireless networks within a controlled and isolated environment, providing insight into nearby networks and preparing for more complex penetration testing in future parts of this series.


---

> Author: [ProxyGeek](https://github.com/Pr0xyG33k)  
> URL: https://Pr0xyG33k.github.io/posts/wireless03/  

