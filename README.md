# SOC Lab 02 — Packet Capture & Network Analysis (Wireshark)

## 📑 Table of Contents
1. [Executive Summary](#executive-summary)
2. [Lab Objectives](#lab-objectives)
3. [Environment Overview](#environment-overview)
4. [Capture Workflow](#capture-workflow)
5. [Packet Analysis](#packet-analysis)
6. [Detection Engineering Insights](#detection-engineering-insights)
7. [Evidence](#evidence)
8. [Conclusions](#conclusions)
9. [Next Steps](#next-steps)

---

## Executive Summary
This lab demonstrates foundational SOC analyst skills by capturing and analyzing network traffic using Wireshark.  
The objective is to understand how core protocols behave on the wire — DNS, ARP, TCP — and how these artifacts support security detection, threat hunting, and incident investigations.

Packet captures (pcap files) form the bedrock of network forensics, enabling analysts to observe traffic at Layers 2–7 and identify malicious behavior in real time.  
All evidence is documented and stored in the `screenshots/` directory.

---

## Lab Objectives
- Launch and operate Wireshark on Linux  
- Capture live network traffic from the correct interface  
- Analyze packet structures at multiple OSI layers  
- Identify DNS queries, TCP handshakes, and ARP frames  
- Understand how Wireshark can support SOC investigations  
- Document findings using SOC-style evidence formatting  

---

## Environment Overview
**Host OS:** Ubuntu Linux (VMware Workstation)  
**Tools Used:**  
- Wireshark  
- ICMP (`ping`)  
- DNS tools (browser queries)  
- Git + GitHub  

---

## Capture Workflow

### 1. Identify Active Network Interface
Selected the correct network interface (e.g., `ens33`) based on traffic volume.

---

### 2. Start Packet Capture
Began live capture, validating traffic flow via constant packet updates.

---

### 3. Generate Traffic for Analysis

**DNS Traffic Trigger**
```bash
ping -c 4 google.com
```

**HTTP Traffic Trigger**  
Opened a website in the browser to generate TCP, DNS, and HTTP/HTTPS traffic.

---

### 4. Stop and Save Capture
Capture stopped and saved as a `.pcapng` file for later review.

---

## Packet Analysis

### 🔹 DNS Query Inspection

**Filter used:**
```text
dns
```

**Observations:**
- Query type (A/AAAA records)  
- UDP transport characteristics  
- Resolved IP address  
- Recursive resolution behavior  

---

### 🔹 TCP 3-Way Handshake

**Filter used:**
```text
tcp.flags.syn==1
```

**Observations:**
- SYN → SYN-ACK → ACK sequence  
- Sequence and acknowledgment numbers  
- MSS and Window Size options  
- Evidence of normal TCP session establishment  

---

### 🔹 ARP Request/Reply

**Filter used:**
```text
arp
```

**Observations:**
- ARP Request: “Who has <IP>?”  
- ARP Reply: “<IP> is at <MAC Address>”  
- Confirms L2 resolution and local network device mapping  

---

## Detection Engineering Insights

Packet captures support SOC teams in:

### 🚨 Threat Detection
- Identifying suspicious DNS queries  
- Detecting beaconing or repeating packet patterns  
- Observing failed TCP session attempts (useful for recon detection)  
- Detecting ARP spoofing or poisoning attacks  

### 🕵️ Incident Response
- Provides packet-level ground truth  
- Shows raw traffic behavior independent of logs  
- Supports timeline reconstruction  
- Helps confirm exfiltration, command-and-control, or scanning  

### 📡 Baselining
Understanding normal traffic patterns helps analysts quickly distinguish anomalies.

---

## Evidence

All screenshots are stored in `/screenshots`:

- Wireshark interface selection  
- Live capture view  
- DNS packet expanded  
- TCP handshake  
- ARP request/reply  
- Saved capture confirmation  

Each screenshot validates a specific stage of the lab and proves hands-on execution.

---

## Conclusions

This lab demonstrates fundamental network forensics capabilities using Wireshark, including:

- DNS inspection  
- TCP handshake analysis  
- ARP packet interpretation  
- Interface selection and capture workflow  

These are essential skills for SOC analysts, threat hunters, and IR teams.

---

## Next Steps

To continue developing strong SOC skills:

- **SOC Lab 03 — Detecting TCP Port Scanning**
- Analyze DNS exfiltration behavior  
- Work with Suricata or Zeek logs  
- Network baseline profiling  
- Create detection rules for packet anomalies  

This lab establishes strong foundations for advanced network detection workflows.
