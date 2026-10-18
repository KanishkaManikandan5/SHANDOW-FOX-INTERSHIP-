# 🔐 ShadowFox Cybersecurity Internship

## 📌 Internship Overview

This repository documents my hands-on learning and practical tasks completed during my **Cybersecurity Internship at ShadowFox** from **1 September to 30 September 2026**.

The internship helped me develop practical knowledge of penetration testing, network security, vulnerability assessment, password security, and ethical hacking through beginner-, intermediate-, and advanced-level exercises.

**Internship Details**
- **Domain:** Cybersecurity
- **Organization:** ShadowFox
- **Internship Duration:** 1 September – 30 September 2026
- **Task Levels:** Beginner, Intermediate, and Advanced
- **Environment:** Kali Linux, Windows 10 Virtual Machine, and authorized training labs

## Overview

The internship was structured around three progressively advanced skill tiers:

1. **Beginner** — Reconnaissance and traffic analysis on a vulnerable web application.
2. **Intermediate** — Encryption/decryption, static binary analysis, payload creation (Metasploit), and wireless attacks.
3. **Advanced** — End-to-end machine penetration testing (TryHackMe's *Basic Pentesting* room).

Each task below follows a consistent reporting format: **Attack Name → Severity (CVSS) → Reason → Steps → Analysis → Impact → Mitigation**.

---

## Tools Used

| Tool | Purpose |
|---|---|
| **Kali Linux** | Primary penetration testing OS |
| **Nmap** | Port scanning and service/version enumeration |
| **Dirb / DirBuster** | Directory & file brute-forcing |
| **Wireshark** | Network traffic capture and analysis |
| **VeraCrypt** | Disk/volume encryption and decryption |
| **CrackStation** | Hash identification and cracking |
| **PE Explorer** | Windows PE (executable) static analysis |
| **Metasploit Framework / msfvenom** | Payload generation and reverse shell handling |
| **Aircrack-ng suite** (airmon-ng, airodump-ng, aireplay-ng, aircrack-ng) | WPA/WPA2 deauthentication and handshake cracking |
| **Gobuster** | Web directory brute-forcing |
| **Enum4linux** | SMB/user enumeration |
| **Hydra** | SSH credential brute-forcing |
| **John the Ripper (ssh2john)** | SSH private key passphrase cracking |

---

## Task Level — Beginner

**Target:** `http://testphp.vulnweb.com/` (Acunetix's deliberately vulnerable demo site)

| # | Task | Attack Name | Severity |
|---|---|---|---|
| 1 | Find all open ports on the target | Port Scanning | Medium (4.0–6.9) |
| 2 | Brute-force the site to discover hidden directories | Directory Brute Forcing | Medium (4.0–6.9) |
| 3 | Log in while capturing traffic and recover credentials | Network Sniffing | High (7.0–8.9) |

**Highlights:**
- `nmap -sV -Pn` / `nmap -A` identified port **80/tcp (Microsoft IIS 8.5)** open, with 999 ports filtered.
- `Dirb`/`DirBuster` enumerated directories such as `/_vti_cnf/`, `/aspnet_client/`, `/cgi-bin/`, `/jscripts/tiny_mce/`, and ASP files (`/Login.asp`, `/Register.asp`).
- Wireshark captured the login `POST` request in **plaintext HTTP**, exposing the submitted username and password fields directly in the packet.

**Core takeaway:** Unencrypted HTTP traffic and unrestricted directory exposure are classic, high-value findings for attackers performing initial reconnaissance.

---

## Task Level — Intermediate

| # | Task | Attack Name | Severity |
|---|---|---|---|
| 1 | Decode a hashed password, unlock a VeraCrypt volume, retrieve the secret code | Password Hash Decoding / Disk Encryption Bypass | Medium (4.0–6.9) |
| 2 | Identify a PE executable's entry point using PE Explorer | Static Binary Analysis | Informational |
| 3 | Generate a Metasploit payload and establish a reverse shell from a Windows 10 VM | Reverse Shell / RCE via Malicious Payload | Critical (9.0–10.0) |
| 4 | Perform a WiFi deauth attack, capture the WPA handshake, and crack the password | Wireless Deauthentication Attack | High (7.0–8.9) |

**Highlights:**
- Identified and cracked a weak/unsalted hash via **CrackStation**, then mounted the VeraCrypt volume to retrieve the secret message.
- Used **PE Explorer** to locate `AddressOfEntryPoint = 004237B0h` in VeraCrypt's installer binary, plus Image Base and section details — a core static-analysis skill for triaging unknown binaries.
- Built a Windows reverse-TCP payload with `msfvenom`, caught the session with `exploit/multi/handler`, and verified access via `sysinfo`, `shell`, and `screenshare` in Meterpreter.
- Ran a full **airmon-ng → airodump-ng → aireplay-ng → aircrack-ng** workflow on my own network to capture and crack a WPA handshake using a custom wordlist.

**Core takeaway:** Weak hashing, unprotected binaries, unpatched endpoints, and short WiFi passphrases each represent a distinct and serious attack surface — demonstrated here end-to-end in a controlled environment.

---

## Task Level — Advanced

**Platform:** [TryHackMe — Basic Pentesting Room](https://tryhackme.com/room/basicpentestingjt)

A full black-box penetration test against a vulnerable machine, covering:

1. **Service enumeration** — `nmap -sV -vv` revealed open ports `22, 80, 139, 445, 8009, 8080`.
2. **Directory discovery** — `gobuster dir` found a hidden `/development` directory.
3. **User & credential discovery** — `enum4linux -U -r` enumerated SMB users; `hydra` brute-forced SSH credentials for user **jan** (password: `armando`).
4. **Privilege escalation path** — Found a second user **kay**; recovered an `id_rsa` private key, cracked its passphrase using `ssh2john.py` + John the Ripper (`beeswax`).
5. **Lateral movement** — Used the key to SSH into **kay**, then retrieved the final password from `pass.bak`:
   `heresareallystrongpasswordthatfollowsthepasswordpolicy$$`

**Defensive recommendations produced:**
- Minimize exposed services (SSH, NetBIOS) where not required.
- Suppress service version banners; keep software updated.
- Avoid predictable/hidden directory names for sensitive paths.
- Enforce strong password policies (8+ characters, special characters, no personal info).

---

## Key Learnings

- **Reconnaissance fundamentals**: Nmap scanning, directory brute-forcing, and traffic analysis form the backbone of any assessment.
- **Encryption is only as strong as its secrets**: weak hashes and short passphrases defeat otherwise sound encryption schemes.
- **Static analysis matters**: understanding PE structure is essential before dynamic analysis of unfamiliar binaries.
- **Payload-based attacks are high-impact**: reverse shells demonstrate why endpoint protection and outbound filtering are critical.
- **Wireless networks are not inherently safe**: WPA2 handshakes are crackable against weak passphrases — WPA3 and strong passphrases are essential.
- **Privilege escalation often relies on information leakage**: exposed private keys and backup files (`pass.bak`) were the pivot point in the advanced room.

---
By 

Kanishka M
---







