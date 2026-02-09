# Lab 01 — Basic Packet Capture & Analysis (Wireshark)

## Objective
Capture a small sample of real network traffic and identify:
- Your local IP address
- Your default gateway
- One DNS query
- One TCP 3-way handshake
- One ARP request/response (if present)

This lab is designed to be quick and beginner-friendly while reinforcing core networking concepts.

---

## Lab Time
~15–30 minutes

---

## Requirements
Choose one setup:

### Option A (Recommended): Kali Linux
- Kali Linux (VM or bare metal)
- Wireshark installed (often preinstalled)

### Option B: Ubuntu / Linux Mint
- Ubuntu/Mint
- Wireshark installed

> Note: Capturing on an interface may require elevated permissions.

---

## Tools
- Wireshark
- Terminal (optional)

---

## Safety / Scope
This lab is **defensive** and is only for capturing traffic on your own machine/network.

---

## Step 1 — Install Wireshark (if needed)

### Kali
Wireshark is usually installed. If not:
```bash
sudo apt update
sudo apt install -y wireshark
```

### Ubuntu / Mint
```bash
sudo apt update
sudo apt install -y wireshark
```

---

## Step 2 — Identify Your Network Interface
In a terminal:
```bash
ip a
```

Look for your active interface, typically:
- `eth0` (wired) or
- `wlan0` (wireless)

---

## Step 3 — Start a Packet Capture
1. Open Wireshark  
2. Select your active interface (e.g., `eth0` / `wlan0`)  
3. Click the blue shark fin to start capture  
4. Generate traffic for ~10–20 seconds by doing ONE of these:
   - Open a website in your browser  
   - Run:  
     ```bash
     ping -c 4 1.1.1.1
     ```
     and:
     ```bash
     nslookup example.com
     ```
5. Stop capture (red square button)

---

## Step 4 — Find Your Local IP Address
### Method A (Terminal)
```bash
ip a
```

### Method B (Wireshark)
- Click any packet  
- Look in the **IP** layer for `Source` / `Destination`  
- Identify your local RFC1918 address (often `192.168.x.x`, `10.x.x.x`, or `172.16–31.x.x`)

✅ **Deliverable**
- Record your local IP address here:
  - `Local IP: ____________________`

---

## Step 5 — Find Your Default Gateway
### Method A (Terminal)
```bash
ip route
```

Look for the line like:
- `default via 192.168.1.1 dev wlan0`

✅ **Deliverable**
- Record your default gateway:
  - `Default Gateway: ____________________`

---

## Step 6 — Identify a DNS Query
In Wireshark, use this display filter:
```
dns
```

Click a DNS packet and look for:
- `Standard query`  
- The domain name requested (e.g., `example.com`)  
- The response with the resolved IP (if you captured both)

✅ **Deliverables**
- Domain queried: `____________________`  
- Resolved IP (if present): `____________________`

📸 Screenshot suggestion:
- Take a screenshot showing the DNS query details pane.

---

## Step 7 — Identify a TCP 3-Way Handshake
In Wireshark, filter:
```
tcp.flags.syn == 1 || tcp.flags.ack == 1
```

Find a handshake sequence:
1. **SYN**  
2. **SYN, ACK**  
3. **ACK**

Tip:
- Click a SYN packet → Right-click → **Follow → TCP Stream** (optional)

✅ **Deliverables**
- Handshake observed between:
  - Source IP: `____________________`
  - Destination IP: `____________________`
  - Destination Port: `____________________` (often 443 or 80)

📸 Screenshot suggestion:
- Capture the three packets (SYN, SYN-ACK, ACK) in the packet list.

---

## Step 8 — Identify ARP (Local Network Resolution)
Filter:
```
arp
```

If you see ARP:
- Look for “Who has X.X.X.X? Tell Y.Y.Y.Y”  
- Then a reply with the MAC address  

✅ **Deliverables**
- ARP “Who has” target IP: `____________________`  
- ARP reply MAC address (if present): `____________________`

---

## Results / Notes
Write 3–5 bullet points about what you observed:
- Example: “Most web traffic used TCP/443 (HTTPS).”
- Example: “DNS queries were in plaintext (UDP/53) on my network.”
- Example: “ARP appeared when my device looked up the gateway MAC.”

---

## Export (Optional but Nice for GitHub)
Save your capture:
- `File → Save As`
- Name it: `lab01_capture.pcapng`

> If you don’t want to upload raw traffic, don’t commit the pcap.  
> You can upload only screenshots + notes.

---

## Reflection Questions (Optional)
1. Why does DNS often use UDP instead of TCP?
2. What does ARP do that DNS does not?
3. Why is the TCP handshake required before most web traffic?

---

## Cleanup
Close Wireshark. No system changes are required.

