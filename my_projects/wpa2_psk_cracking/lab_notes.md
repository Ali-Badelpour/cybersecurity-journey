# 🧪 Lab Notes – WPA2‑PSK Cracking Demonstration

## ⚠️ Critical Disclaimers

- **This is a lab‑only exercise.** Everything was performed on hardware I own, inside my own home, with my own AP and client.

- **Do not use these techniques** on any network or device that you do not own or have explicit written permission to test.

- The password (`12345678`) is intentionally weak to demonstrate how quickly a dictionary attack succeeds. **In production, always use complex, unpredictable passphrases (≥ 16 characters, mixing upper/lower, numbers, symbols).**

---
## 🔍 Technical Deep‑Dive

### Why WPA2‑PSK is Vulnerable to This Attack

- **Shared Key** – The Pre‑Shared Key (password) is used to derive the Pairwise Master Key (PMK), which never changes. An attacker who captures the 4‑way handshake can perform an offline brute‑force or dictionary attack against the PMK, without needing to interact with the AP again.

- **Deauthentication** – The deauth frame is unauthenticated in the 802.11 standard (no encryption or signature). This allows an attacker to forcibly disconnect any client, triggering a new handshake when it reconnects. **This is a protocol design flaw**, not a bug in any specific router.

- **Dictionary Attack** – The attack’s success depends entirely on password complexity. With `rockyou.txt` (≈14 million passwords), common or short passwords fall in seconds to minutes.

### Why My Integrated Intel AX201 Worked

Not all Wi‑Fi chips support monitor mode or packet injection. Intel’s AX201 (with proper Linux drivers) does, but:
- Monitor mode may reduce throughput and cause instability.

- Some drivers require firmware patches.

- Always test with `iw list | grep -A 10 "Supported interface modes"` and `aireplay-ng --test wlan0mon` before starting.

### Capturing the Handshake – What to Look For

In `airodump-ng`, the handshake is confirmed when you see: WPA handshake: A2:DF:95:85:F7:F1 at the top‑right of the terminal. If you don’t see this, the `.cap` file contains no usable handshake – repeat the deauth step or increase the deauth count (e.g., `--deauth 10`).

---
## 🧹 Cleanup After the Lab

After finishing, always:
```zsh
sudo airmon-ng stop wlan0mon
sudo systemctl restart NetworkManager
sudo systemctl restart wpa_supplicant   # if used
```
This restores normal networking. Verify with `ping 8.8.8.8` or connect to your normal AP.

---
## 🔑 Clarifying Weak Password on Purpose 

> **💡 Why a Weak Password Was Used**  
> The password `12345678` was chosen **intentionally** to demonstrate how quickly and trivially a dictionary attack succeeds against common, weak credentials. In under 10 seconds, `aircrack-ng` matched the hash against this entry in `rockyou.txt`.  
> 
> **This is the entire point of the demonstration:**  
> - WPA2‑PSK is cryptographically sound *if* the password has high entropy.  
> 
> - A strong password (e.g., `BlueFrog$Runs#Fast42!`) would render this attack infeasible with a standard dictionary, taking years or millennia to brute‑force.  
>
> - The takeaway is **never use dictionary words, personal info, or sequential numbers** as your Wi‑Fi password.

---
### ⚡ Monitor Mode & Packet Injection

- **Not all Wi‑Fi adapters are equal.** Many built‑in chips (especially Broadcom, Realtek, or older Intel) either lack monitor mode support or have broken packet injection.  

- **To verify your adapter’s capabilities**, run:
  ```zsh
  sudo iw list | grep -A 10 "Supported interface modes"
  ```
  Look for `monitor` in the list. Then test injection with:
```zsh
sudo aireplay-ng --test wlan0mon
```
You should see `Injection is working!` – if not, your adapter may not be suitable.

- **My Intel AX201 worked** after disabling power management and killing interfering processes (`airmon-ng check kill`). If you have a different chip, consider an external USB adapter with an Atheros or Ralink chipset (e.g., Alfa AWUS036ACH) for guaranteed compatibility.

