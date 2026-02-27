# 🍯 T-Pot Honeypot Lab

<img src="https://img.shields.io/badge/-T--Pot-FF6600?&style=for-the-badge&logoColor=white" /> <img src="https://img.shields.io/badge/-Docker-2496ED?&style=for-the-badge&logo=docker&logoColor=white" /> <img src="https://img.shields.io/badge/-Elastic_Stack-005571?&style=for-the-badge&logo=elastic&logoColor=white" />
<img src="https://img.shields.io/badge/-Kibana-E8478B?&style=for-the-badge&logo=kibana&logoColor=white" />
<img src="https://img.shields.io/badge/-Suricata-EF3B2D?&style=for-the-badge&logo=suricata&logoColor=white" />
<img src="https://img.shields.io/badge/-Linux-FCC624?&style=for-the-badge&logo=linux&logoColor=black" />

---

## 📖 Overview

This project documents the setup, configuration, and analysis of a **T-Pot Honeypot** deployed on a cloud virtual machine. T-Pot is an all-in-one, multi-honeypot platform developed by Deutsche Telekom Security, combining 20+ honeypot daemons with the Elastic Stack for real-time visualization and threat intelligence.

The goal of this lab is to expose a decoy system to the internet, capture live attack data, and analyze attacker behavior — including brute force attempts, malware drops, vulnerability exploitation, and port scanning — in a safe, controlled environment.

---

## 🎯 Objectives

- Deploy and configure T-Pot on a cloud-hosted Linux VM
- Capture real-world attack traffic across multiple protocols and services
- Analyze attacker TTPs (Tactics, Techniques, and Procedures) using Kibana dashboards
- Gain hands-on experience with honeypot technology, log analysis, and threat intelligence

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|---|---|
| **T-Pot** | Multi-honeypot platform and orchestration framework |
| **Docker** | Container runtime for all honeypot daemons |
| **Kibana** | Log visualization and dashboard analysis |
| **Elasticsearch** | Log ingestion and indexing backend |
| **Suricata** | Network Security Monitoring (NSM) engine |
| **Cowrie** | SSH/Telnet honeypot — captures brute force & shell activity |
| **Dionaea** | Malware-capturing honeypot |
| **Honeytrap** | Dynamic TCP port honeypot |
| **Spiderfoot** | Open source intelligence (OSINT) automation |
| **CyberChef** | In-browser data analysis and decoding |
| **Attack Map** | Animated live visualization of incoming attacks |
| **p0f** | Passive OS fingerprinting |
| **FATT** | Network metadata and fingerprint extraction from traffic |

---

## ⚙️ System Requirements

| Resource | Minimum |
|---|---|
| RAM | 8–16 GB |
| Disk Space | 128 GB SSD |
| OS | Debian / Ubuntu (64-bit) |
| Network | Non-proxied internet connection required |
| Architecture | amd64 or arm64 |

---

## 🚀 Installation & Setup

### 1. Provision a Cloud VM
Spin up a VM (e.g., Vultr, AWS, DigitalOcean) running a minimal **Debian/Ubuntu** installation with SSH enabled.

### 2. Configure Firewall Rules
Before installing, temporarily restrict inbound access to your IP only via SSH. After installation, open all TCP/UDP ports (1–64000) to allow attack traffic in, while keeping management ports (`>64000`) restricted to trusted IPs only.

| Port | Purpose |
|---|---|
| 64294 | T-Pot Admin UI |
| 64295 | SSH (Management) |
| 64297 | T-Pot Web UI |

### 3. Create a Non-Root User
```bash
adduser <username>
usermod -aG sudo <username>
su - <username>
```

### 4. Install curl
```bash
sudo apt update && sudo apt install curl -y
```

### 5. Clone and Run the T-Pot Installer
```bash
git clone https://github.com/telekom-security/tpotce.git
cd tpotce
sudo ./install.sh
```

### 6. Select Your Edition
During installation, select the edition that fits your use case:

- **HIVE (Standard)** — All honeypots, tools, and the Elastic Stack on a single host *(recommended for beginners)*
- **Sensor** — Lightweight sensor that reports to a remote HIVE
- **Custom** — Build your own configuration via `docker-compose`

### 7. Set Credentials
Create a web UI username and password when prompted. **Remember these** — they are required to access the T-Pot dashboard.

### 8. Reboot and Access
After installation completes and the system reboots, navigate to:
```
https://<your-server-ip>:64297
```
Log in with your credentials to access the T-Pot landing page.

---

## 📊 Dashboard & Analysis

Once T-Pot is running and exposed to the internet, attack data begins flowing in automatically. Key analysis tools available from the T-Pot landing page:

**Kibana** — The primary analysis interface. Use pre-built dashboards filtered by honeypot type (e.g., Cowrie, Honeytrap, Dionaea) to explore:
- Source IPs and geolocation of attackers
- Most targeted ports and services
- Attack frequency over time
- Credentials used in brute force attempts
- Malware samples captured

**Attack Map** — A real-time, animated global map showing incoming attacks by origin country.

**Spiderfoot** — Run OSINT lookups on attacker IPs to enrich threat intelligence.

**CyberChef** — Decode and analyze captured payloads.

---

## 🔍 Key Findings

> *(Update this section with your own observations after running the honeypot)*

- Most common attack vector: **SSH brute force via Cowrie**
- Top attacking countries: *(e.g., CN, RU, US, NL)*
- Most targeted ports: **22 (SSH), 23 (Telnet), 445 (SMB), 3389 (RDP)**
- Malware samples captured: *(e.g., Mirai botnet variants, cryptominers)*
- Notable credentials attempted: `root/root`, `admin/admin`, `admin/password`

---

## 📁 Log Storage

All honeypot logs are persisted locally in:
```
~/tpotce/data
```
Logs are retained for **30 days** by default. Elasticsearch indices follow the T-Pot Index Lifecycle Policy, configurable directly in Kibana.

---

## ⚠️ Disclaimer

> This honeypot is intentionally exposed to malicious traffic for research and educational purposes. **You are solely responsible** for the deployment and operation of T-Pot in your environment. Never run a honeypot on a network containing sensitive systems or data. A system compromise can never be fully ruled out.

---

## 📚 References

- [T-Pot GitHub Repository](https://github.com/telekom-security/tpotce)
- [T-Pot Official Documentation](https://github.com/telekom-security/tpotce#readme)
- [Elastic Stack Documentation](https://www.elastic.co/docs)
- [Cowrie Honeypot](https://github.com/cowrie/cowrie)

---

## 🙌 Acknowledgements

T-Pot is built and maintained by the **Deutsche Telekom Security Team** and powered by the open source community. This lab was set up as part of a personal cybersecurity learning journey to gain hands-on exposure to real-world attack patterns and threat intelligence workflows.
