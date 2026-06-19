# WPA2-PSK Handshake Capture & Cracking

> **⚠️ Legal Notice**  
> This project is **strictly for educational purposes** and should only be performed on **your own Wi‑Fi network and devices** that you own or have explicit permission to test. Unauthorized access to networks is illegal. The author assumes no liability for misuse.

---

> **📡 Note on Visible Identifiers**  
> I have deliberately chosen to keep the SSID (`Ali`), BSSID (`A2:DF:95:85:F7:F1`), and client MAC addresses visible in this documentation.  
> 
> **Why?**
>  
> - **Transparency & Reproducibility** – Readers can verify that all captured frames, handshake exchanges, and deauth packets in the screenshots are genuine and belong to a single, isolated test environment.  
> 
> - **Lab Environment** – These identifiers belong exclusively to my own mobile hotspot and my own second laptop. No third‑party networks or devices are exposed.  
> 
> - **Educational Value** – Seeing real MAC addresses and SSIDs helps beginners understand how these values appear in tools like `airodump-ng` and Wireshark, bridging the gap between theory and practical observation.  
> 
> **Security Consideration:** Since these devices are used only for this demonstration and will be reset/reconfigured afterward, and since the password is a trivial example (`12345678`), there is no real‑world risk. I do not recommend exposing production network identifiers.

---
## 📖 Overview

This documents a hands‑on demonstration of capturing a WPA2‑PSK (Pre‑Shared Key) handshake and cracking the password using **aircrack-ng**. The attack was performed on my own router and my own client device to understand the vulnerabilities of the WPA2 protocol.

The project provides:
- A **step‑by‑step guide** to reproduce the attack.
- Reference images of the actual execution.

---
## 🧰 Requirements

| Component            | My device                                                                |
| -------------------- | ------------------------------------------------------------------------ |
| **OS**               | I used Kali Linux Live boot 2025.3                                       |
| **Wireless Adapter** | I used Intel(R) Wi-Fi 6 AX201 160MHz (Integrated on a laptop)            |
| **Target**           | My own Mobile Phone's Hotspot (WPA2‑PSK) and my another laptop           |
| **Tools**            | `airmon‑ng`, `airodump-ng`, `aircrack-ng` , `wordlist` (`rockyou.txt`)   |
| **Hardware**         | Two laptops (one client and one attacker) and one mobile phone (hotspot) |

---

## 📁 Repository Structure

```
my_projects/
└── wpa2_psk_cracking/
	├── README.md               # This file
	├── lab_notes.md            # Project notes
	├── pictures/               # Screenshots
	    ├── 01-kali-info.png
	    ├── hotspot-1.jpg
	    ├── hotspot-2.jpg
	    ├── second-laptop.jpg
	    ├── NIC.png
	    ├── 02-airmon-ng-check-kill.png
	    ├── 03-airmong-ng-start-wlan0.png
	    ├── 04-airodump-ng-wlan0mon.png
	    ├── 05-aireplay-working.png
	    ├── 06-handshake-captured.png
	    ├── 07-rockyou-unziped.png
	    ├── 08-ls-cap-files.png
	    ├── 09-wirehark-capture01-analyze.png
	    ├── 10-aircrack-ng-part-1.png
	    ├── 11-aircrack-ng-part-2.png
	    ├── 12-stoping-wlan0mon-restarting-networkmanager.png
	    ├── 13-connecting-to-hotspot.png
	    └── 14-connected-to-ap.png
```

---
## 🕹️ Hardware Specifications
### 1. Kali Linux Live (Main Laptop - Attacker)

![Kali Linux Info](wpa2_psk_cracking/pictures/01-kali-info.png)
I used Kali Linux Live 2025.3 on USB Flash drive running on my laptop. I am using this laptop as the `attacker` system and booting it live. 

### 2. The Hotspot (AP)

![Hotspot 1](hotspot-1.jpg)
I used my Mobile Phone's Hotspot as the AP to get my other laptop connected to that as a client. The SSID is `Ali`, the BSSID is `A2:DF:95:85:F7:F1`, the password is `12345678`, and the security is `WPA2-Personal`. 

![Hotspot 2](hotspot-2.jpg)

### 3. Second Laptop (Target)

![Second Laptop](second-laptop.jpg)
I used my second laptop as the client (target) connected to my Hotspot of my mobile. The OS is Windows 10 with the BSSID of the interface `98:22:EF:02:65:36`. 

### 4. The NIC for Testing

![NIC](NIC.png)
On my First Laptop (Running Kali Linux Live), I have the Intel(R) Wi-Fi 6 AX201 160MHz which is capable of monitoring mode and packet injection. 

## 🔧 Step‑by‑Step Actions

### 1. Checking PID before Running Monitor Mode

```zsh
sudo airmon-ng check kill
```

![Airmon-ng check kill](02-airmon-ng-check-kill.png)
I only have one wireless interface which is `wlan0` on Kali Linux. I used the command above to stop some processes that might interrupt my further actions. 

### 2. Enable Monitor Mode

```zsh
sudo airmon-ng start wlan0
```

![Airmon-ng start wlan0](03-airmong-ng-start-wlan0.png)
This created a new interface on my device, which is `wlan0mon`. I used that to get the traffic and packet injection.   

### 3. Focus Capture on My Target AP

I wanted to scan only for my system. I do not want to get any information about others, only my own systems. I ran `airodump-ng` to capture packets on my specific channel and BSSID:

```zsh
sudo airodump-ng -c 11 --bssid A2:DF:95:85:F7:F1 wlan0mon
```

![Airodump-ng wlan0mon](04-airodump-ng-wlan0mon.png)
I kept this terminal running.

### 4. Deauthenticate the Client (My second Laptop)

In the previous part, I found my second laptop's MAC address in the STATION part of the `airodump-ng` terminal (which I already knew). Next, in a **new terminal**, I forced my own client device (second laptop) to reconnect by sending deauth packets:

```zsh
sudo aireplay-ng --deauth 5 -a A2:DF:95:85:F7:F1 -c 98:22:EF:02:65:36 wlan0mon
```

![Aireplay working](05-aireplay-working.png)

**⚠️ Caution:** Some older routers may experience instability or damage from repeated deauth attacks – I tested this only on my Mobile Phone's Hotspot I was willing to risk.

### 5. Verify Handshake Capture

Back in the `airodump-ng` terminal, watch the top‑right corner. I got **`WPA handshake: A2:DF:95:85:F7:F1 `**, the handshake has been captured. I stopped the capture (`Ctrl+C`). The `.cap` file now contains the handshake.

![Handshake captured](06-handshake-captured.png)

### 6. Extracting `rockyou.txt`.gz

I used Kali Live, so I am forced to do some actions every time I boot the system **live**. For example, extracting my wordlist is one of it. I used this command to extract `rockyou.txt.gz` wordlist in `/usr/share/wordlists`.

```zsh
sudo gunzip /usr/share/wordlists/rockyou.txt.gz
```

![Rockyou unzipped](07-rockyou-unziped.png)

Now, the captured files are ready for cracking!

![LS cap files](08-ls-cap-files.png)

### 7. Analyzing `.cap` File in the Wireshark

I analyzed the capture-01.cap file using Wireshark.

```zsh
sudo wireshark capture-01.cap
```

![Wireshark capture 01 analyze](09-wirehark-capture01-analyze.png)

### 8. Cracking the Hash with `aircrack-ng`

I used `aircrack-ng` to crack the hash and find the password (which is 12345678 and I knew it from the beginning).

> **💡 Why a Weak Password Was Used**  
> The password `12345678` was chosen **intentionally** to demonstrate how quickly and trivially a dictionary attack succeeds against common, weak credentials. In under 10 seconds, `aircrack-ng` matched the hash against this entry in `rockyou.txt`.  
> 
> **This is the entire point of the demonstration:**  
> - WPA2‑PSK is cryptographically sound *if* the password has high entropy.  
> 
> - A strong password (e.g., `BlueFrog$Runs#Fast42!`) would render this attack infeasible with a standard dictionary, taking years or millennia to brute‑force. 
> 
> - The takeaway is **never use dictionary words, personal info, or sequential numbers** as your Wi‑Fi password.

```zsh
sudo aircrack-ng -w /usr/share/wordlists/rockyou.txt capture-01.cap
```

![Aircrack-ng part 1](10-aircrack-ng-part-1.png)
![Aircrack-ng part 2](11-aircrack-ng-part-2.png)

I found the password in the `aircrack-ng` terminal which is `KEY FOUND! [ 12345678 ]`.

### 9. Stopping Monitor Mode

Everything is done now and processes worked correctly. I stopped my `wlan0mon` interface and restarted `NetworkManager` to **be able to connect** to the AP I hacked. 

```zsh
sudo airmon-ng stop wlan0mon
sudo systemctl restart NetworkManager
```

![Stopping wlan0mon restarting NetworkManager](12-stoping-wlan0mon-restarting-networkmanager.png)

### 10. Connecting to the Hacked AP

With the password I found, I successfully connected to my Mobile Phone's Hotspot of myself that I hacked.
![Connecting to hotspot](13-connecting-to-hotspot.png)

![Connected to AP](14-connected-to-ap.png)

---
## 📊 Results

| Metric | Outcome |
|--------|---------|
| **Handshake Capture** | ✅ Successfully captured the 4‑way handshake from my own mobile hotspot |
| **Password Complexity** | `12345678` – 8‑digit numeric, trivial to crack |
| **Time to Crack** | ≈ **2 seconds** (dictionary lookup in `rockyou.txt`) |
| **Attack Complexity** | Low – no special hardware, only a standard Wi‑Fi adapter and Kali Linux |

**Key Observation:** The attack succeeded not because WPA2 is broken at the cryptographic level (it isn’t), but because the password was predictable. If the password had been even moderately complex (e.g., `My$ecureP@ss2024`), `aircrack-ng` would have failed, and brute‑forcing would take centuries.

**Quantitative Note:** `rockyou.txt` contains ~14 million passwords. My `12345678` appeared at line ~1,000, making it an extremely early match. This underscores that **length and randomness are the real defenses**.

---
## 📑 What I Learned

### Technical Takeaways
- **WPA2‑PSK’s vulnerability is not the cipher (AES‑CCMP) but the authentication mechanism.** The 4‑way handshake exposes the PMK, allowing offline attacks.
- **Deauthentication is a protocol‑level flaw** that cannot be fully patched without breaking backwards compatibility – it’s a design trade‑off.
- **Monitor mode requires hardware support** – not every chip can do it, and even those that do may need driver tweaks.
- **Wireshark analysis** helped visualize the EAPOL frames and confirm the handshake sequence – an invaluable debugging skill.

### Operational Takeaways
- **Preparation is key** – having `rockyou.txt` extracted, correct interface, and channel locked saved time.
- **Screenshots are essential** for documenting a live attack – without them, it’s just text.
- **Cleanup (`airmon-ng stop`, `restart NetworkManager`) is just as important** – forgetting this leaves your system offline.

### Security Mindset
- **Assume your attacker has the same tools I used.** The only effective defense is a strong passphrase and, where possible, WPA3.
- **Security through obscurity (hiding SSID) does nothing** – the SSID is broadcast in beacon frames, and deauth attacks don’t need it.
---
## ⚡ Why This Matters

WPA2‑PSK remains the most common Wi‑Fi security protocol in homes and small businesses. This demonstration exposes two fundamental weaknesses:
1. **The deauthentication attack** – because deauth frames are not authenticated, an attacker can force a client to reconnect and capture the handshake at will.
2. **Offline password cracking** – once the handshake is captured, the attacker can crack it offline without ever touching the network again. Only password strength stands in the way.
Understanding these vectors is the first step to defending against them – by using WPA3, strong passphrases, and monitoring for deauth floods.

---
## 🔮 For Future – Next Steps in Wi‑Fi Security Research

This demonstration is just the beginning. I plan to expand my knowledge along these lines:

1. **WPA3 & Dragonblood**  
   Investigate the 2019 Dragonblood vulnerabilities (downgrade attacks, side‑channel leaks) and test whether my router’s WPA3 implementation is vulnerable. Understand how SAE (Simultaneous Authentication of Equals) improves on PSK.

2. **PMKID Attacks (without a client)**  
   Learn and execute the PMKID attack (introduced by Steube in 2018) which captures the PMKID from the AP itself, eliminating the need to deauth a client. (We have it in `Network Basics for Hackers by occupytheweb)

3. **Advanced Cracking**  
   - Use `hashcat` with GPU acceleration to crack faster.  
   - Combine `airodump-ng` with `hcxdumptool` and `hcxpcaptool` for modern hash formats (`22000`).  
   - Explore rule‑based attacks (e.g., `--rules` in `hashcat`) to mutate dictionary words.

4. **Defensive Measures**  
   - Configure router/AP to detect and log deauth floods (e.g., via `wireshark` or `snort` rules).  
   - Experiment with **WPA3‑Enterprise** (802.1X) to understand certificate‑based authentication.  
   - Test **802.11w (Management Frame Protection)** – this can protect deauth frames if both AP and client support it.

5. **Write a Companion Defensive Guide**  
   Create a follow‑up repository titled *“Securing Your Home Wi‑Fi Against Deauth & Cracking”* with actionable steps for non‑technical users.

All future tests will remain strictly within my own lab, on devices I own, and with explicit logging to ensure traceability.

---
## 🔧 Troubleshooting Common Issues

| Problem | Likely Cause | Solution |
|---------|--------------|----------|
| `airodump-ng` shows no networks | Adapter not in monitor mode | Re‑run `airmon-ng start wlan0` and check `iwconfig` |
| No handshake after deauth | Client didn’t reconnect; or wrong channel | Increase deauth count (`--deauth 10`); verify channel with `airodump-ng` first |
| `aircrack-ng` says "No valid handshake" | The `.cap` file is corrupt or incomplete | Recapture with fresh `airodump-ng` and force a new deauth |
| Injection test fails | Driver/hardware limitation | Switch to an external adapter known for injection (e.g., Alfa) |
| `rockyou.txt` not found | Not extracted | Run `sudo gunzip /usr/share/wordlists/rockyou.txt.gz` |

---
## ⚖️ Ethical Use & Legal Compliance

This project is a **proof of concept for educational purposes only**. Laws regarding network probing vary by country. For example:

- In the US, the CFAA (Computer Fraud and Abuse Act) criminalizes unauthorized access.
- In the EU, GDPR and national laws impose strict penalties for intercepting communications.
- My country is like them as well.

**You are solely responsible** for ensuring you have explicit authorization before testing any network that is not your own. The author and repository assume no liability for misuse.

---
## 📚 References 

- `Network Basics for Hackers - occupytheweb`
- `Linux Basics for Hackers - occupytheweb`
-  `Deepseek AI`

---
## 📜 License

This project is provided under the **MIT License** – feel free to use and modify for educational purposes. Always respect the law and the privacy of others.

---
## 🤝 Contributing

Found a bug or have a suggestion? Open an issue or submit a pull request. All contributions that improve clarity or safety are welcome.

---
