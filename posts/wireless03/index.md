# Wireless Hacking Part 3


## About

In Part 3 of our Wireless Hacking series, we will delve into the practical application of wireless attacks. After exploring the basics of Wi-Fi and setting up a safe testing environment, it’s time to get hands-on. In this post, we’ll focus on attacking WEP (Wired Equivalent Privacy) — a once-standard but now outdated and highly vulnerable wireless encryption protocol.

In this post, you’ll learn how WEP works, the cryptographic weaknesses it suffers from, and step-by-step how to exploit these flaws using tools like **aircrack-ng** and **aireplay-ng**. By the end, you’ll understand why WEP is obsolete and how attackers can recover a WEP key within minutes.

> [!CAUTION]
> **Ethical Hacking Reminder**: Only perform wireless penetration tests on networks you own or have explicit permission to test. Unauthorized hacking is illegal and unethical.

## RC4/ Stream Cipher

The process starts when the user creates a root key of size 128 bits. This root key is then encrypted using the RC4 algorithm, which generates a keystream. Once the keystream is generated, it is combined with the plaintext data using an XOR gate (logical operation). The result of this operation gives us the **ciphertext**, which can then be sent.

```mermaid
graph TD
    RK[128-bit Root Key]
    RC4[RC4 Algorithm]
    KS[Pseudorandom Keystream]
    XOR[XOR Operation]
    PT[Plaintext Data]
    CT[Ciphertext]
    NET[Send over Network]

    RK --> RC4
    RC4 --> KS
    KS --> XOR
    PT --> XOR
    XOR --> CT
    CT --> NET

```

## WEP Packet

The process of sending a packet using WEP begins with appending a 24-bit Initialization Vector (IV) to the root key. This new combined key is then input into the RC4 encryption algorithm, which generates a keystream unique to that packet. Next, this keystream is combined with the plaintext data using an XOR operation, producing the ciphertext that will be transmitted over the wireless network.

Since the IV changes with every packet, it is appended along with the ciphertext. This is necessary because the receiving device (such as a router) needs the IV to recreate the same keystream and correctly decrypt the message. Without the IV, decryption would not be possible, as the key would not match the keystream used for encryption.

```mermaid
graph TD
        RK[128-bit Root Key]
        IV[24-bit Initialization Vector]
        RK --> IV

        KS[Generate Keystream with RC4]
        XOR[XOR Keystream with Plaintext]
        CT[Obtain Ciphertext]
        IV --> KS
        KS --> XOR
        XOR --> CT

        TX[Send Ciphertext + IV]
        CT --> TX
        IV --> TX

        RX[Receive Ciphertext + IV]
        KS2[Generate Keystream with RC4]
        XOR2[XOR Keystream with Ciphertext]
        PT[Retrieve Plaintext Data]
        RX --> KS2
        KS2 --> XOR2
        XOR2 --> PT
```

## Router-side Decryption

During router-side decryption, the process begins by appending the root key with the 24-bit Initialization Vector (IV) extracted from the received data packet. This new key is then used as input to the RC4 algorithm, which generates a pseudorandom keystream. Next, this keystream is combined with the ciphertext using an XOR operation. The result is the original data stream requested by the device. However, it is important to note that the mathematical weaknesses of RC4 expose this method to vulnerabilities: by collecting enough keystreams associated with different IV values, an attacker can analyze the data to recover the network’s secret key.

```mermaid
graph TD
    RX[Receive Ciphertext + 24-bit IV]
    RK[128-bit Root Key]
    IV[24-bit Initialization Vector from Packet]
    CONCAT[Append IV to Root Key]
    RC4[RC4 Algorithm]
    KS[Pseudorandom Keystream]
    XOR[XOR Operation]
    CT[Ciphertext Data]
    PT[Recovered Plaintext Data]

    RX --> IV
    RK --> CONCAT
    IV --> CONCAT
    CONCAT --> RC4
    RC4 --> KS
    CT --> XOR
    KS --> XOR
    XOR --> PT
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

