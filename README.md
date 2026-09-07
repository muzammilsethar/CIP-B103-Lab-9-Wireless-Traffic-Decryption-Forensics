# 📡 CIP-B103 Lab 9: Wireless Traffic Decryption & Packet Forensics

<p align="center">
  <a href="https://www.linkedin.com/in/muzammil-sethar/">
    <img src="https://img.shields.io/badge/LINKEDIN-CONNECT-0A66C2?style=for-the-badge&logo=linkedin" alt="LinkedIn">
  </a>
  <a href="mailto:Muzammilsethar@gmail.com">
    <img src="https://img.shields.io/badge/EMAIL-CONTACT_ME-D14836?style=for-the-badge&logo=gmail" alt="Email">
  </a>
  <img src="https://img.shields.io/badge/STATUS-OPEN_FOR_OPPORTUNITIES_|_CTFS_|_BUG_BOUNTIES_|_COLLABORATIONS-44CC11?style=for-the-badge" alt="Status">
</p>

---

**Author:** Mohammad Muzamil  
**Registration No:** C11/26/DFIT/17289  
**Role:** Digital Forensics Internship Trainee (DFIT) | ICDFA  
**Platform:** Kali Linux | Aircrack-ng | Airdecap-ng | TShark | Foremost  
**Domain:** Wireless Forensics | Offline Key Recovery | Network Traffic Carving  

---

## Overview
This repository documents the forensic analysis and offline decryption workflow for **CIP-B103 Lab 9**, executed under the Digital Forensics Internship Trainee (DFIT) program at **ICDFA**. 

The investigation focuses on recovering static WEP cryptographic keys using initialization vector (IV) statistical analysis, performing offline payload decryption, reconstructing network conversations, and carving artifacts from decrypted packet streams.

---

## Execution Methodology & Technical Screenshots

### 1. Packet Hierarchy & IV Distribution Analysis
Inspected raw capture parameters (`capinfos`), mapped IEEE 802.11 frame structures, and parsed repeated Initialization Vectors (IVs) using TShark filters.

![Capture Metadata & Protocol Hierarchy](b103%20lab9.2.png)

![Protected WEP Frame Analysis](b103%20lab9.3.png)

![WEP IV Repetition Logging](b103%20lab9.4.png)

---

### 2. Key Recovery & Offline Decryption
Executed statistical IV cracking via `aircrack-ng` to extract the 40-bit WEP key (`A4:3D:F6:F3:74`), followed by full payload decryption using `airdecap-ng`.

![WEP Key Recovery & Airdecap Execution](b103%20lab9.5.png)

![Decryption Integrity & File Inventory](b103%20lab9.6.png)

---

### 3. Layer 2 / Layer 3 Endpoint & Conversation Mapping
Analyzed Ethernet and IPv4 endpoints from the decrypted stream (`working/file_working-dec`) to identify active host IP-to-MAC associations.

![Ethernet and IP Endpoint Statistics](b103%20lab9.7.png)

![IPv4 Conversation Breakdown](b103%20lab9.8.png)

---

### 4. Protocol Dissection & File Carving
Parsed TCP/TLS stream sequences and carved exported HTTP objects using `tshark --export-objects` and `foremost`. Logged extracted file types and cryptographic hashes.

![Decrypted Frame Sequence Parsing](b103%20lab9.9.png)

![HTTP Object Extraction & Foremost Commands](b103%20lab9.10.png)

![Carved File Hashes & Manifest Inventory](b103%20lab9.11.png)

---

## Cryptographic Hashes & File Verification

| File Name | Description | SHA-256 Hash |
| :--- | :--- | :--- |
| `file_working` | Raw Protected PCAP | `c17a3f9b955e84f5befd476dbd55c67286d1e3eea9ab402d5359cac0874ebb2d` |
| `file_working-dec` | Decrypted PCAP Stream | `167c91994c269777f9048227deb89882caf3cf3c763977f2059604f9a6a40b04` |

---

## Key Forensic Findings

| Metric / Artifact | Identified Value | Forensic Relevance |
| :--- | :--- | :--- |
| **Target BSSID** | `00:26:66:55:97:D6` (`cgnetwork`) | Target Wireless Access Point |
| **Recovered Key** | `A4:3D:F6:F3:74` | 40-bit WEP Key (15,477 IVs processed) |
| **Primary Host** | `192.168.0.15` (`Apple_68:96:7c`) | Active internal client generating majority traffic |
| **Carved Artifacts** | Images, JS, HTML, Fonts | Extracted via TShark HTTP export & Foremost |

---

## Defensive Mitigation Strategies
- **Deprecate WEP/WPA1:** Upgrade wireless security profiles to WPA3-Enterprise or WPA2-AES (CCMP).
- **Network Segmentation:** Isolate legacy wireless devices into restricted VLANs.
- **Payload Encryption:** Enforce end-to-end TLS/HTTPS to protect application data against Link-Layer interception.
