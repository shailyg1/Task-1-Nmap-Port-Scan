#  Task 1 - Network Reconnaissance using Nmap (Elevate Labs Cybersecurity Internship)

 Objective
The goal of this task is to perform **network reconnaissance** using the powerful tool **Nmap** to identify active devices on the local network and discover their **open ports**. This helps in understanding network exposure and recognizing services that could be potential entry points for attackers.-

 Tools & Environment

-  **Operating System:** Kali Linux (preferred for cybersecurity tasks)
-  **Nmap:** Open-source network scanner for port scanning and host discovery.
-  **Wireshark (optional):** For packet-level analysis of network activity.
-  **Local Network Setup:** Wi-Fi-connected LAN environment.

---

  Methodology

Step 1: Identify Local Network IP Range
Command used:
ip a

Identified my IP as 192.168.1.7, so I scanned the entire subnet: 192.168.1.0/24.
 Step 2: Perform a TCP SYN Scan

Command:

sudo nmap -sS 192.168.1.0/24 -oN scan_results.txt

    -sS: TCP SYN scan (stealthy and efficient)

    -oN: Output saved in scan_results.txt

File Structure
scan_results.txt
Raw Nmap scan output
README.md
Full explanation of task, commands, and analysis

Scan Results
Host IP	Open Ports	Services	MAC Address
192.168.1.1	53, 80, 443	DNS, HTTP, HTTPS	20:0C:86:D8:07:40 (GX India Pvt)
192.168.1.2	80	HTTP	E8:65:D4:93:C8:E8 (Tenda Technology)
192.168.1.3	None (All filtered)	-	E8:65:D4:94:0C:38 (Tenda Technology)
192.168.1.5	None (All filtered)	-	E8:65:D4:E6:E3:D8 (Tenda Technology)
192.168.1.7	5000, 7000, 49152	UPnP, AFS, Unknown	4E:2F:B8:50:56:63 (Unknown)
192.168.1.16	None (All closed)	-	(No MAC detected)

To see the full scan details, refer to scan_results.txt.

Risk Analysis of Open Ports

Port	Service	Potential Risks
80	HTTP	Unencrypted data transmission, may expose sensitive info
443	HTTPS	Secure, but may expose login portals
53	DNS	Can be abused for DNS tunneling or data exfiltration
5000	UPnP	Vulnerable if not restricted, often exploited by malware
7000	AFS	Rare service, may be unused or misconfigured
49152	Unknown	High-number port, might be a custom or vulnerable service
 

Wireshark Packet Analysis

To go beyond basic port scanning, I used **Wireshark** to capture and analyze the actual packets sent and received during the Nmap TCP SYN scan.

 Objective
Understand how open, closed, and filtered ports respond at the packet level using TCP handshake analysis.

---

Filter Used in Wireshark
```wireshark
I used two filters during Wireshark analysis:

1. `ip.addr == 192.168.1.1` — to focus on packets sent to/from my router.
2. `tcp.flags.syn == 1 && tcp.flags.ack == 0` — to view only SYN packets from the Nmap scan
This filter shows only the initial SYN packets sent by Nmap to initiate the scan.
 Observations
Behavior	Packet Signature	Meaning
Open Port	SYN → SYN-ACK	Port is open and accepting TCP
Closed Port	SYN → RST	Port is closed
Filtered Port	SYN → No Response	Firewall filtering the port
 Packet Capture File

The full .pcapng file is saved in this repository at:

/wireshark/nmap_scan_capture.pcapng

This file can be opened in Wireshark to review:

Source and destination IPs
TCP handshakes
Port responses
