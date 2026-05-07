
Knowing how networking works is crucial for every hacker. Sometimes we are hacking a remote system, and a good hacker knows that they need to understand how to connect and interact with the network. For instance, you may need to connect to a system while keeping your IP address hidden, or you might want to change your DNS to redirect traffic to your system. We will look at some essential Linux tools for analyzing and managing networks.

### Analyzing Networks with `ifconfig`

The `ifconfig` command is the most basic and essential tool for checking and managing active network connections.
[[01-ifconfig.png]]

**Analyzing the output:**

1. **`eth0`** at the top – Short for Ethernet0, the first wired network connection. If you have more, they will be displayed and numbered sequentially (eth1, eth2, etc.).

2. **`HWaddr`** – The MAC address (hardware address). This is a unique address physically written on your network adapter. It's often called the **physical address**.

3. **`inet`** – The IPv4 address (e.g., `192.168.106.132`).  
   - **`netmask`** – Determines which part of the IP belongs to the local network.  
   - **`broadcast`** – The address used to send information to all IPs on the subnet.

4. **`lo` (loopback)** – Also called `localhost`. This is a software interface that connects you to your own system. You can bind services to this address to stop them from being accessible externally.

5. **`wlan0`** – The wireless interface (Wi-Fi adapter) connected to the system.

### Network Statistics with `netstat` and `ss`

The `netstat` (network statistics) tool shows all connections that your system sends or receives. Use it to troubleshoot, monitor network connections, or even find malware connections on your system.
[[02-netstat.png]]

**Important switches for `netstat`:**
- `-t` : Display all TCP connections
- `-u` : Display all UDP connections
- `-l` : Display all listening connections
- `-a` : Display all connections

You can also pipe `netstat` output to `grep` to search for specific information.
[[03-netstat_http.png]]

**The `ss` tool** displays more information in a better-readable format.
[[04-ss.png]]

### Checking Wireless Network Devices with `iwconfig`

Use `iwconfig` to find important information about any external USB wireless devices connected to your system. When using wireless hacking tools like `aircrack-ng`, it's essential to know the information provided by `iwconfig`.
[[05-iwconfig.png]]

The wireless network interface is assigned as `wlan0` (no wireless interfaces for `eth0` or `lo`).

You can see the wireless standards (e.g., `802.11 IEEE` with letters like `b`, `g`, or `n`). You can also identify the **mode** of your wireless connection. There are three common modes:

- **Managed mode:** The normal client mode; your device connects to an access point.
- **Monitor mode:** Used for packet sniffing; the interface captures packets without associating with an access point.
- **Promiscuous mode:** The interface captures all packets on the network, not just those addressed to it.


### Changing Your Network Information

A crucial skill for a hacker is modifying network information to appear as a trusted device. Changing your IP address and MAC address are key techniques. For example, when performing a DoS (Denial-of-Service) attack, you should change your IP address so that system administrators trace a spoofed IP instead of your real one. This is simple and can be done with `ifconfig`.

#### Assigning a New IP Address

Use `ifconfig` with the interface name and your desired IP address. Remember to use `sudo`:

```bash
sudo ifconfig eth0 192.168.159.123
```

After running this, the terminal will show no output—just return to the command prompt, indicating success.
[[06-ip-change.png]]

#### Changing Your Network Mask and Broadcast Address

Exactly like assigning a new IP address, you can change netmask and broadcast address with `ifconfig`:

```bash
sudo ifconfig eth0 192.168.159.123 netmask 255.255.0.0 broadcast 192.168.1.255
```
[[07-ip-netmask-broadcast-change.png]]


#### Spoofing Your MAC Address

The MAC address (HWaddr) is a unique physical address written on your network hardware. It's used for security measures, but a hacker can often **spoof** (steal another MAC address and assign it to their own machine) to bypass these measures.

**Steps to spoof a MAC address:**

1. **Bring down the interface:**
   ```bash
   sudo ifconfig eth0 down
   ```

2. **Assign the new MAC address** using `hw ether`:
   ```bash
   sudo ifconfig eth0 hw ether 00:11:22:33:44:55
   ```

3. **Bring the interface back up:**
   ```bash
   sudo ifconfig eth0 up
   ```
[[08-spoofing-mac.png]]

### Assigning a New IP Address from the DHCP Server

Linux runs a **DHCP server daemon** called `dhcpd` (DHCP daemon). The DHCP server assigns IP addresses to all devices on the subnet and keeps logs of which IP was given to which system. This is important for tracking hackers after an attack.

When connected to a LAN and needing internet access, you should obtain a DHCP-assigned IP address. You can request a new IP without restarting your system.

**Command using `dhcpcd` (the preferred DHCP client on modern Kali) - (I used AI for this part):**

``` bash
sudo dhcpcd wlan0
```

> **Note:** Different Linux distributions have different DHCP clients. Kali now uses `dhcpcd` by default instead of the older `dhclient`. If you need to release your current IP first, use `sudo dhcpcd -k wlan0` then request a new one.

When you call the DHCP client, your interface sends a discovery request. If you receive an offer, you will have a new IP address. Verify with `ifconfig`.
[[09-dhcpcd-wlan0.png]]

### Manipulating the Domain Name System (DNS)

DNS translates domain names (like `www.google.com`) into IP addresses (like `8.8.8.8`). Without DNS, you would need to remember the IP address of every website you visit. DNS also contains valuable information for hackers.

#### Examining DNS with `dig`

The `dig` tool helps gather DNS information about a website, including:
- The **nameserver** (the server that translates the domain name to an IP address)
- The target's **mail exchange (MX) server**
- Any **subdomains** and their IP addresses

**Finding nameservers (NS):**
```bash
dig www.example.com ns
```
Look at the **ANSWER SECTION** to find the nameserver. The **ADDITIONAL SECTION** may contain the IP addresses of those DNS servers.
[[10-dig-ns.png]]

**Finding mail exchangers (MX):**
```bash
dig www.example.com mx
```
Check the **AUTHORITY SECTION** for mail server information.
[[11-dig-mx.png]]

> **Note:** Linux has a DNS server called **BIND** (Berkeley Internet Name Domain). It functions the same as DNS. Many Linux users prefer BIND.

### Changing Your DNS Server

To change your DNS server on Linux, edit the plaintext file `/etc/resolv.conf`. Use any text editor (I prefer `nano`):

```bash
sudo nano /etc/resolv.conf
```

Look for the line starting with `nameserver` (usually line 3). Change the IP address—for example, to Google's DNS (`8.8.8.8`). Save the file.
[[12-nano-etc-resolv.conf.png]]

**Faster method (echo command) - (I used AI for this part):**

The direct `echo >` command often fails due to permission issues. Instead, use:

``` bash
echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
```

This echoes the string and uses `tee` to write it with superuser privileges.
[[13-echo-resolv-conf.png]]

> **Note:** The system first queries the local DNS server. If it fails, it will query the public DNS server.
>
> **Note:** If you are using a DHCP server with its own DNS settings, it will overwrite `/etc/resolv.conf` when it assigns a new IP address.

### Mapping Your Own IP Address (Local Hosts File)

Imagine typing `www.cnn.com` but your browser goes to `www.bbc.com` instead. This is called **domain name mapping**. On a LAN, you can hijack TCP connections and redirect traffic to a malicious web server using tools like `dnsspoof`.

To manually map IP addresses to domain names, edit the `/etc/hosts` file:

```bash
sudo nano /etc/hosts
```

By default, only your localhost and system hostname are listed. You can add any IP address mapped to any domain. **Use a TAB between the IP address and the domain name** (not spaces).
[[14-nano-etc-hosts-part-1.png]]
[[15-nano-etc-hosts-part-2.png]]

> **Note:** Add your entry **above** the `::1` line (`::1` is reserved for IPv6). Example:
> ```
> 192.168.1.1    www.farsroid.com
> ```
> Now when you type `www.farsroid.com`, you will be mapped to `192.168.1.1`.
>
> To revert changes, you may need to run `sudo updatedb` or clear the cache.


As we learn more about tools like `dnsspoof` or `Ettercap`, we will see how to redirect any traffic on your LAN that visits a specific website to your own web server at a specific IP address.

