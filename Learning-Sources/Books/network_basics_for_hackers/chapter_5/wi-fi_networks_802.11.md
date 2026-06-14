
### ⚠️ IMPORTANT DISCLAIMER & ETHICAL NOTE

> **All testing, scanning, and attacks described in this chapter were performed exclusively on my own network equipment and personally owned devices.**  
>  
> - I own the router (access point), two laptops, a mobile phone, and any other devices used in these exercises.  
> - All screenshots have been anonymized: any MAC address, BSSID, SSID, IP address, or other identifier not belonging to me has been **completely blacked out** (brushed) to protect the privacy of others.  
> - The deauthentication (deauth) attacks were only directed at **my own devices** connected to **my own AP** – no third‑party traffic was intercepted or disrupted.  
> - The techniques for hiding SSIDs, defeating MAC filtering, cracking WPA2‑PSK, building an Evil Twin, and other attacks were carried out **solely within my own controlled laboratory environment**.  
> - **I did NOT perform the Denial of Service (DoS) attack** on my network because of the risk of permanently damaging my older router hardware.  
> - **I did NOT perform the PMKID attack** because my router does not support the required features (RSN IE / PMKID).  
>  
> **The sole purpose of this documentation is educational.** Under no circumstances should these techniques be used against any network or device without explicit written permission. Unauthorized access to or disruption of networks is illegal in most countries. **Always hack your own gear, or get permission first.**

---

Wireless connections are the norm in our digital lives today. Wi‑Fi, Bluetooth, and cellular services are connections we use every day — to connect to a café Wi‑Fi, to a smartwatch, and to make calls with our phones. Each of these can be hacked and is vulnerable. In this book, we will glance at all these connection types (not fully covering all aspects, though). In this chapter, we will focus on Wi‑Fi.

In this chapter, we will examine some ways a Wi‑Fi AP could be attacked, from getting the password (PSK) to eavesdropping on Wi‑Fi traffic. We need some knowledge of Linux and Kali tools, as well as patience!

Remember: to protect something, we must know how it is attacked. In order to know how it is attacked, we need to understand its anatomy. Normal people just use Wi‑Fi, but we need to understand how it works. The IEEE calls Wi‑Fi **802.11**.

### Wi‑Fi / 802.11

Wi‑Fi has several names. For example, it is sometimes called **WLAN** (Wireless Local Area Network), and the IEEE (Institute of Electrical and Electronics Engineers) calls it **802.11 technologies**. The IEEE is an organization for creating standards.

The security protocols for Wi‑Fi (802.11) started with **WEP** (Wired Equivalent Privacy). WEP was hacked easily. Then we moved to **WPA** (Wi‑Fi Protected Access), which was an update for WEP but was vulnerable again. Next came **WPA2**, which is strong but can still be hacked. Now we have **WPA3**, which is very strong.

**Note from surfing the net:** WPA3 was also hacked in 2019 by the Dragonblood attack! Many Wi‑Fi access points still use WPA2 (not even WPA3) because of cost and the difficulty of upgrading frameworks. WE NEED TO MAKE THEM SAFER!

### Terminology

- **AP** – The device we connect to for Wi‑Fi and internet access (Access Point).
- **PSK** – The password for your authentication on your AP (Pre‑Shared Key).
- **SSID** – The name of your AP for identification in the Wi‑Fi list (Service Set Identifier).
- **ESSID** – Similar to SSID, but used for multiple APs in a wireless LAN (Extended Service Set Identifier).
- **BSSID** – The unique identifier for the AP, which is its MAC address (Basic Service Set Identifier).
- **Channels** – Wi‑Fi operates on channels 1 to 14, but in the U.S., it is limited to 1–11.
- **Power** – The signal is stronger when you are closer to the AP. In the U.S., the signal is limited to 0.5 watts by the FCC.
- **Security** – The protocol used to authenticate and encrypt Wi‑Fi traffic. The most common now is WPA2‑Personal or WPA2/WPA3‑Personal.
- **Modes** – Wi‑Fi has three operating modes: Master, Managed, and Monitor. APs work in Master mode. Wireless network interfaces normally work in Managed mode, and hackers work in Monitor mode.
- **Range** – With 0.5 watts, the range is up to 100 meters. With higher power (like using antennas), you can achieve up to 32 kilometers.
- **Frequency** – We have 2.4 GHz and 5 GHz. Most modern systems can operate on both.

### 802.11 Security Protocols

We have different security protocols. In order to test and attack, we must first identify the protocol.

#### WEP (Wired Equivalent Privacy)

In 2001, the first protocol for securing Wi‑Fi was published by the IEEE under the name WEP. It was hacked quickly by attackers cracking the user's password in minutes. The mistake was the poor implementation of the RC4 encryption algorithm. The IEEE later updated the security protocol. We might still find some APs using WEP even today.

#### WPA (Wi‑Fi Protected Access)

In 2003, the IEEE updated the security standard with WPA. It didn't require changing the hardware physically, only upgrading the firmware. WPA also uses the RC4 encryption algorithm but added features that make cracking the PSK much harder and more time‑consuming. These changes are:

1. The Initialization Vector increased from 48 bits to 128 bits.
2. TKIP (Temporal Key Integrity Protocol) generates different keys for each client.
3. MIC (Message Integrity Check) ensures the message is intact and unchanged.

#### WPA2

In 2004, WPA2 (802.11i) was released. This security protocol uses Counter Mode with Cipher Block Chaining Message Authentication Protocol, commonly known as **CCMP**. The CCMP is based on the Advanced Encryption Standard (AES) for authentication and encryption. CCMP was heavy on processors, so APs needed stronger hardware.

WPA2 has Personal and Enterprise modes. In Personal mode, all connected devices use the same password. In Enterprise mode, the PSK is combined with the SSID to create a Pairwise Master Key (PMK), making it more difficult to use rainbow table password cracking. The key generated from this method is unique to each person and session, making Wi‑Fi traffic sniffing harder.

### Wi‑Fi Adapters for Hacking

The adapter we need must be capable of **injecting frames** into a wireless AP. This injection capability is required for a tool called Aircrack‑ng in Kali, which we can use for Wi‑Fi hacking.

### Viewing Wireless Interfaces (on My Own System)

> *All scans below were performed on my personal Kali machine, and any unrelated networks or devices have been fully blacked out.*

To see the wireless interfaces, we can use the `ifconfig` command. (only my own interface visible)
![ifconfig output showing wireless interfaces](pictures/01-ifconfig.png)

To get a better and more specific output for wireless adapters, we can use the `iwconfig` command.
![iwconfig output for wireless adapter](pictures/02-iwconfig.png)

We can see all Wi‑Fi APs within my wireless network interface's range using:
(all non‑my‑own information blacked out)
```bash
iwlist wlan0 scan
```
![iwlist scan output](pictures/03-iwlist-scan.png)

By running that command, we can obtain the following from an AP: its MAC address (BSSID), channel, frequency, ESSID, and mode.

### Monitor Mode

In 802.11, we must use **monitor mode** to capture all packets traveling through the network. Monitor mode is equivalent to promiscuous mode in a wired network. Before putting the NIC into monitor mode, it is a good idea to kill some processes that may cause interference. We can do so by running:

```bash
sudo airmon-ng check kill
```
![airmon-ng check kill output](pictures/04-airmon-ng-check-kill.png)

**Note:** `hcxdumptool` (used later in this chapter) can enable monitor mode on its own. Modern versions of `hcxdumptool` advise against setting monitor mode with third‑party tools like `airmon-ng` before running it.

To put the wireless interface into monitor mode using `airmon-ng`:

```bash
sudo airmon-ng start wlan0
```
![airmon-ng start wlan0 output](pictures/05-airmon-ng-start.png)
When the interface is set to monitor mode, its name changes (e.g., `wlan0` becomes `wlan0mon`).

### Capturing Frames

Once we can see the traveling data, we must start capturing it. We can use `airodump-ng`, a part of the Aircrack‑ng suite: (all external SSIDs/BSSIDs blacked out except my own devices)
```bash
sudo airodump-ng wlan0mon
```
![airodump-ng scanning for networks](pictures/06-airodump-ng.png)

We can see the SSID of the APs, their BSSID, and clients connected to them (with their device BSSIDs). In my screenshot, only my own AP and my own devices appear; everything else has been brushed.

### Anatomy of Wi‑Fi Frames

To create our own tools in the networking field, we need to understand deeper layers of the network. In this case, we need to dig deeper into the 802.11 protocol.

We will now analyze the frame types and their Wireshark filters. Tools like `airodump-ng` and `Kismet` use these frames to provide the information needed for hacking APs.

|Frame Type|Description|Wireshark Filter|
|---|---|---|
|Association request|Sent by a station to associate with a BSS.|`wlan.fc.type==0x00`|
|Association response|Sent in response to an association request.|`wlan.fc.type==0x01`|
|Reassociation request|Sent by a station changing association to another AP in the same ESS (roaming between APs, or reassociating with the same AP).|`wlan.fc.type==0x02`|
|Reassociation response|The response to the reassociation request.|`wlan.fc.type==0x03`|
|Probe request|Sent by a station to "scan" for an SSID (this is how tools like `airodump-ng` find the AP even if the SSID is hidden).|`wlan.fc.type==0x04`|
|Probe response|Sent by each BSS participating in that SSID.|`wlan.fc.type==0x05`|
|Beacon|A periodic frame sent by the AP (or stations in an IBSS) providing information about the BSS.|`wlan.fc.type==0x08`|
|ATIM|The traffic indication map for IBSS (in a BSS, the TIM is included in the beacon).|`wlan.fc.type==0x09`|
|Disassociation|The frame used to terminate the association of a session.|`wlan.fc.type==0x0A`|
|Authentication|The frame used to perform 802.11 authentication (not any other type of authentication).|`wlan.fc.type==0x0B`|
|Deauthentication|The frame used to terminate the authentication of a station. This frame is used to "bump" users off the AP using `aireplay-ng` or to perform a Denial of Service (DoS) on the AP.|`wlan.fc.type==0x0C`|
|Action|A frame meant for sending information elements to other stations (when sending in a beacon is not possible or optimal).|`wlan.fc.type==0x0D`|
|PS‑Poll|The Power‑Save Poll frame, used to poll for buffered frames after a station wakes up.|`wlan.fc.type==0x1A`|
|RTS|Request‑to‑Send frame.|`wlan.fc.type==0x1B`|
|CTS|Clear‑to‑Send frame (often a response to RTS).|`wlan.fc.type==0x1C`|
|ACK|Acknowledgment frame, sent to confirm receipt of a frame.|`wlan.fc.type==0x1D`|
|Data frame|The basic frame containing data.|`wlan.fc.type==0x20`|
|Null frame|A frame meant to contain no data but carries flag information (very important).|`wlan.fc.type==0x24`|
|QoS Data|The QoS version of the data frame.|`wlan.fc.type==0x28`|
|QoS Null|The QoS version of the null frame.|`wlan.fc.type==0x2C`|
### Wireshark Display Filter for Wi‑Fi Frames

To apply the filters we just learned in Wireshark, go to the **Analyze** tab and select **Display Filter Expression**.
![Wireshark filter expression window](pictures/07-wireshark-filter-expression.png)

In the search bar, enter `wlan.fc.type` to quickly find the IEEE 802.11 Wireless LAN section.
![Wireshark search for wlan.fc.type](pictures/08-wireshark-wlan-search.png)

In the predefined values, you will see the filters we explained above.
![Wireshark predefined values for wlan.fc.type](pictures/09-wireshark-predefined-values.png)

If we know our filter before selecting our interface in Wireshark, we can set our filter and start capturing with it.
![Wireshark capture with wlan.fc.type filter](pictures/10-wireshark-wlan-filter.png)

When creating hacking tools and networking tools, we need to understand these frames!

### Attacking Wi‑Fi APs – All Performed on My Own Network

> **All attacks in this subsection were executed exclusively against my personal router and my own client devices (two laptops, one mobile phone). No third‑party networks or devices were involved.**

#### Hidden SSIDs (Tested on My Router)

Some security engineers think that hiding their SSID keeps them safe. This idea is **wrong** – I verified this by hiding my own SSID and then discovering it with `airodump-ng`.

When a user tries to connect to an AP, the probe request and response contain the SSID. We don't even need the SSID; we only need the BSSID to connect to the AP. I successfully found my hidden network's BSSID without knowing the SSID. (my own hidden SSID test, other networks blacked out)
![Testing hidden SSID discovery with airodump-ng](pictures/11-hide-ssid-test-part-1.png)
![Testing hidden SSID discovery with airodump-ng](pictures/12-hide-ssid-test-part-2.png)
![Testing hidden SSID discovery with airodump-ng](pictures/13-hide-ssid-test-part-3.png)

#### Defeating MAC Filtering (Tested on My Router)

I enabled MAC address blacklisting on my router, not allowing my laptop’s original MAC address. I then used `ifconfig` to spoof my phone's address and successfully connected to my own network.

Steps I followed (on my own devices):
1. Listen with the NIC in monitor mode: `sudo airmon-ng start wlan0`.
2. Capture the MAC address of my own authorized device. (Mobile Phone)
3. Stop monitor mode: `sudo airmon-ng stop wlan0mon`.
4. Take the interface down: `sudo ifconfig wlan0 down`.
5. Change the MAC address: `sudo ifconfig wlan0 hw ether <my mobile phone's MAC address>`.
6. Bring the interface back up: `sudo ifconfig wlan0 up`.

After this, my original laptop (with spoofed MAC) could join my own router again. (only my own MAC addresses shown)
![MAC address spoofing with ifconfig](pictures/14-mac-spoofing-part-1.png)
![MAC address spoofing with ifconfig](pictures/15-mac-spoofing-part-2.png)
![MAC address spoofing with ifconfig](pictures/16-mac-spoofing-part-3.png)
![MAC address spoofing with ifconfig](pictures/17-mac-spoofing-part-4.png)
![MAC address spoofing with ifconfig](pictures/18-mac-spoofing-part-5.png)
![MAC address spoofing with ifconfig](pictures/19-mac-spoofing-part-6.png)
![MAC address spoofing with ifconfig](pictures/20-mac-spoofing-part-7.png)
![MAC address spoofing with ifconfig](pictures/21-mac-spoofing-part-8.png)
![MAC address spoofing with ifconfig](pictures/22-mac-spoofing-part-9.png)
![MAC address spoofing with ifconfig](pictures/23-mac-spoofing-part-10.png)
![MAC address spoofing with ifconfig](pictures/24-mac-spoofing-part-11.png)
![MAC address spoofing with ifconfig](pictures/25-mac-spoofing-part-12.png)
![MAC address spoofing with ifconfig](pictures/26-mac-spoofing-part-13.png)
![MAC address spoofing with ifconfig](pictures/27-mac-spoofing-part-14.png)
![MAC address spoofing with ifconfig](pictures/28-mac-spoofing-part-15.png)
![MAC address spoofing with ifconfig](pictures/29-mac-spoofing-part-16.png)
![MAC address spoofing with ifconfig](pictures/30-mac-spoofing-part-17.png)


#### Attacking WPA2‑PSK (Tested on My Router)

I performed a WPA2 handshake capture on **my own router** using my own client device.

**Steps:**

1. Put the wireless network card in monitor mode.
2. Start capturing packets with `airodump-ng` focused on my AP's BSSID and channel.
3. Deauthenticate **my own client** (my second laptop) from **my own AP**:  – all only showing my own equipment.
   ```bash
   sudo aireplay-ng --deauth 100 -a <my_BSSID> -c <my_client_MAC> wlan0mon
   ```
   > **Note:** Older routers can be damaged by this – I used a test router that I was willing to risk.
4. The WPA handshake was captured and saved.
5. I then cracked the hash using `hashcat` with a wordlist (on my own password: 12345678).
![WPA2-PSK attack](pictures/31-wpa2-psk-attack-part-1.png)
![WPA2-PSK attack](pictures/32-wpa2-psk-attack-part-2.png)
![WPA2-PSK attack](pictures/33-wpa2-psk-attack-part-3.png)
![WPA2-PSK attack](pictures/34-wpa2-psk-attack-part-4.png)
![WPA2-PSK attack](pictures/35-wpa2-psk-attack-part-5.png)


#### WPS (Wi‑Fi Protected Setup) – Tested on My Router

I own a router that supports WPS 2.0. Using `wash` I verified that my router had WPS enabled. 
I did not use `bully` or `reaver` to attack WPS on my router due to possible hardware damages. 
![Wash output showing WPS status](pictures/36-wash-output.png)

#### Evil Twin Attack (Man‑in‑the‑Middle) – Tested on My Own Network

I set up an Evil Twin AP using `airbase-ng`. I just created that and check that is working or not. I did not attack my own systems and network using this technique. I used `dnsmsq` tool.

![Airbase-ng creating Evil Twin AP](pictures/37-airbase-ng-evil-twin-part-1.png)
![Airbase-ng creating Evil Twin AP](pictures/38-airbase-ng-evil-twin-part-2.png)
![Airbase-ng creating Evil Twin AP](pictures/39-airbase-ng-evil-twin-part-3.png)
![Airbase-ng creating Evil Twin AP](pictures/40-airbase-ng-evil-twin-part-4.png)
![Airbase-ng creating Evil Twin AP](pictures/41-airbase-ng-evil-twin-part-5.png)
![Airbase-ng creating Evil Twin AP](pictures/42-airbase-ng-evil-twin-part-6.png)
![Airbase-ng creating Evil Twin AP](pictures/43-airbase-ng-evil-twin-part-7.jpg)
![Airbase-ng creating Evil Twin AP](pictures/44-airbase-ng-evil-twin-part-8.jpg)
![Airbase-ng creating Evil Twin AP](pictures/45-airbase-ng-evil-twin-part-9.jpg)




#### Denial of Service (DoS) Attack – Not Performed

> **I did NOT perform the DoS attack described in this section.** The deauth flood can permanently damage older router hardware. Since I value my equipment, I chose to study the theory only.

#### PMKID Attack – Not Performed

> **I did NOT perform the PMKID attack** because my router does not support the required RSN IE / PMKID features. The following commands are included for educational reference only, based on updated syntax.
![hcxdumptool](pictures/46-hcxdumptool-part-1.png)
![hcxdumptool](pictures/47-hcxdumptool-part-2.png)
![hcxdumptool](pictures/48-hcxdumptool-part-3.png)
*(The PMKID command section remains as a theoretical reference, but no actual test was conducted.)*

