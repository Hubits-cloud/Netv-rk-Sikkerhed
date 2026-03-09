# 🔐 Cisco NetAcad – Network Security 1.0
### Complete Notes — All 22 Modules & Every Submodule

---

> **Course:** Cisco Networking Academy – Network Security 1.0  
> **Coverage:** All 22 modules with all submodules included  
> **Format:** Concise, exam-focused, with configuration examples

---

## 📋 Table of Contents

| Module | Title |
|--------|-------|
| 1 | [Securing Networks](#module-1-securing-networks) |
| 2 | [Network Threats](#module-2-network-threats) |
| 3 | [Mitigating Threats](#module-3-mitigating-threats) |
| 4 | [Secure Device Access](#module-4-secure-device-access) |
| 5 | [Assigning Administrative Roles](#module-5-assigning-administrative-roles) |
| 6 | [Device Monitoring and Management](#module-6-device-monitoring-and-management) |
| 7 | [Authentication, Authorization, and Accounting (AAA)](#module-7-authentication-authorization-and-accounting-aaa) |
| 8 | [Access Control Lists](#module-8-access-control-lists) |
| 9 | [Firewall Technologies](#module-9-firewall-technologies) |
| 10 | [Zone-Based Policy Firewalls](#module-10-zone-based-policy-firewalls) |
| 11 | [IPS Technologies](#module-11-ips-technologies) |
| 12 | [IPS Operation and Implementation](#module-12-ips-operation-and-implementation) |
| 13 | [Endpoint Security](#module-13-endpoint-security) |
| 14 | [Layer 2 Security Considerations](#module-14-layer-2-security-considerations) |
| 15 | [Cryptographic Services](#module-15-cryptographic-services) |
| 16 | [Basic Integrity and Authentication](#module-16-basic-integrity-and-authenticity) |
| 17 | [Public Key Cryptography](#module-17-public-key-cryptography) |
| 18 | [VPNs](#module-18-vpns) |
| 19 | [Implement Site-to-Site IPsec VPNs](#module-19-implement-site-to-site-ipsec-vpns) |
| 20 | [Introduction to the ASA](#module-20-introduction-to-the-asa) |
| 21 | [ASA Firewall Configuration](#module-21-asa-firewall-configuration) |
| 22 | [Network Security Testing](#module-22-network-security-testing) |

---

# 🔐 Cisco NetAcad – Network Security 1.0
### Complete Notes — All 22 Modules & Submodules

---

# Module 1: Securing Networks

## 1.1 Current State of Affairs

Networks are under constant, evolving attack. Key drivers of risk:
- **Expanding attack surface** – IoT, cloud, remote work, BYOD
- **Organised threat actors** – cybercrime is a multi-billion dollar industry
- **High-value targets** – financial data, PII, intellectual property
- **Regulatory pressure** – GDPR, HIPAA, PCI-DSS require technical controls

**Why breaches happen:**
- Misconfigured devices
- Unpatched vulnerabilities
- Weak credentials
- Untrained users (social engineering)

**The CIA Triad – Core of All Security:**

| Pillar | Definition | Threatened by | Control |
|---|---|---|---|
| **Confidentiality** | Only authorised parties read data | Sniffing, theft | Encryption, access control |
| **Integrity** | Data is accurate and unmodified | MitM, tampering | Hashing, digital signatures |
| **Availability** | Services accessible when needed | DoS, hardware failure | Redundancy, rate limiting |

Additional properties:
- **Non-repudiation** – sender cannot deny sending (digital signatures)
- **Authentication** – verify identity before granting access
- **Authorisation** – define what an authenticated entity may do
- **Accounting** – log all activity for audit and forensics

## 1.2 Network Topology Overview

Understanding the network layout is essential before applying security controls.

| Network Type | Description | Security Concern |
|---|---|---|
| **SOHO** | Small/home office; consumer router | Weak default configs, no enterprise security |
| **Campus (CAN)** | Buildings in close proximity | Insider threats, physical access |
| **WAN** | Spans large geographic areas | Transit traffic exposure |
| **Data Centre** | Off-site, VPN-connected facility | VM sprawl, physical security, hyperjacking |
| **Cloud** | Third-party hosted infrastructure | Shared responsibility, misconfiguration |
| **Virtual Networks** | VMs, SDN environments | Hyperjacking, instant-on activation, VM escape |

**Virtualisation-specific threats:**
- **Hyperjacking** – attacker hijacks the VM hypervisor, launching attacks from it
- **Instant-on activation** – dormant VM brought online with outdated security policies
- **VM escape** – malware breaks out of a VM to affect the hypervisor/host
- **VM sprawl** – forgotten/unmanaged VMs increase attack surface

**Cisco security tools aligned to topology:**

| Tool | Purpose |
|---|---|
| **ESA** (Email Security Appliance) | Anti-spam, anti-phishing, malware scanning in email |
| **WSA** (Web Security Appliance) | URL filtering, malware scanning for web traffic |
| **ASA** (Adaptive Security Appliance) | Stateful firewall + VPN concentrator |
| **ISE** (Identity Services Engine) | AAA, 802.1X, posture assessment |
| **Firepower / NGIPS** | Next-gen IPS + application awareness |

## 1.3 Securing Networks Summary

> Security must be planned into the network architecture, not bolted on afterwards. Every layer — physical, data link, network, and application — needs its own controls. Understand your topology, your threats, and your assets before choosing controls.

---

# Module 2: Network Threats

## 2.1 Who is Attacking Our Network?

**Threat actor categories:**

| Actor | Motivation | Skill Level |
|---|---|---|
| **Script kiddies** | Curiosity, notoriety | Low – run pre-built tools |
| **Hacktivists** | Political/social cause | Moderate |
| **Cybercriminals** | Financial gain | Moderate–High |
| **State-sponsored / APT** | Espionage, sabotage, disruption | Very High |
| **Insiders** | Revenge, financial, accidental | Varies |
| **Competitors** | Industrial espionage | Varies |

**Hacker terminology:**
- **White hat** – ethical hacker; authorised testing
- **Black hat** – malicious; criminal intent
- **Grey hat** – breaks rules without malicious intent; may self-disclose
- **Cracker** – breaks into systems to steal/damage
- **Hacktivist** – motivated by political/social causes
- **Script kiddie** – uses others' tools with no understanding

## 2.2 Threat Actor Tools

Threat actors use a wide range of tools to conduct attacks:

| Tool Type | Examples | Purpose |
|---|---|---|
| **Password crackers** | Hashcat, John the Ripper | Brute-force or dictionary-attack hashed passwords |
| **Wireless tools** | Aircrack-ng | Crack WEP/WPA wireless keys |
| **Network scanners** | Nmap, Zenmap | Discover hosts, open ports, OS fingerprinting |
| **Packet sniffers** | Wireshark, tcpdump | Capture and analyse network traffic |
| **Vulnerability scanners** | Nessus, OpenVAS | Identify known vulnerabilities in systems |
| **Exploitation frameworks** | Metasploit | Automate exploitation of vulnerabilities |
| **Rootkit tools** | Various | Hide attacker presence from OS |
| **Fuzzers** | Peach Fuzzer | Send malformed input to find crashes/vulnerabilities |
| **Forensic tools** | Autopsy, FTK | (Defenders) recover evidence; attackers use to find data |

> 💡 The same tools used by attackers are also used by ethical hackers and security professionals — intent and authorisation are what matter.

## 2.3 Malware

### Viruses
- Attach to legitimate files/programs and require **user action** to spread
- Types:
  - **File infector** – infects executable files (.exe, .com)
  - **Boot sector** – infects the MBR; loads before OS
  - **Macro** – embedded in Office documents (Word macros)
  - **Polymorphic** – changes its signature code each time to evade AV detection
  - **Metamorphic** – rewrites itself entirely; hardest to detect

### Worms
- **Self-replicating** – no user action required
- Exploit network vulnerabilities (unpatched OS, services) to spread autonomously
- Can consume significant network bandwidth
- Famous examples: Code Red, Slammer, WannaCry (used EternalBlue/MS17-010)

### Trojans
- **Disguised** as legitimate, useful software
- Do **not** self-replicate
- Subtypes:
  - **RAT** (Remote Access Trojan) – full remote control of victim machine
  - **Backdoor** – hidden persistent access bypassing normal auth
  - **Downloader** – fetches additional malware once installed
  - **Banking Trojan** – steals online banking credentials (e.g., Zeus)
  - **Ransomware dropper** – installs ransomware

### Ransomware
- Encrypts victim's files; demands payment (cryptocurrency) for decryption key
- Delivery: phishing emails, RDP brute force, exploit kits
- Notable: WannaCry (2017), CryptoLocker, REvil/Sodinokibi, Conti

### Rootkits
- Operate at **kernel or firmware level**
- Conceal presence of other malware from the OS and security tools
- Extremely difficult to detect and remove
- **Bootkits** persist even through OS reinstallation

### Spyware & Adware
- **Spyware** – captures keystrokes, browsing history, credentials; sends to attacker
- **Adware** – displays unwanted advertisements; typically bundled with free software
- **Grayware** – not clearly malicious, but unwanted and potentially risky (PUPs)

### Botnets
- **Bot** (zombie) – a compromised host under attacker remote control
- **Botnet** – a network of thousands/millions of bots
- **C2 (Command & Control)** server – issues instructions to the botnet
- **Uses:** DDoS attacks, spam campaigns, credential stuffing, crypto-mining, click fraud

## 2.4 Common Network Attacks – Reconnaissance, Access, and Social Engineering

### Reconnaissance Attacks
Goal: gather information to plan a more effective attack.

**Passive reconnaissance** (no packets sent to target):
- Packet sniffing (Wireshark in promiscuous mode)
- OSINT – WHOIS, DNS records, LinkedIn, job postings, social media
- Traffic analysis – identifying communication patterns

**Active reconnaissance** (packets are sent; may trigger IDS):
- **Ping sweep** – ICMP echo to find live hosts (Nmap -sn)
- **Port scan** – identify open ports (Nmap -sS)
- **OS fingerprinting** – deduce OS from TCP/IP stack behaviour
- **Vulnerability scanning** – automated discovery of known weaknesses (Nessus)

### Access Attacks
Goal: gain unauthorised access to resources.

**Password attacks:**

| Attack Type | Description |
|---|---|
| **Brute force** | Try every possible combination of characters |
| **Dictionary attack** | Try words from a wordlist (rockyou.txt) |
| **Credential stuffing** | Use leaked username/password pairs from breaches |
| **Password spraying** | Try one common password against many accounts |
| **Rainbow table** | Look up hashes in pre-computed hash→plaintext tables |
| **Keylogging** | Capture keystrokes with malware or hardware device |

**Trust exploitation** – leveraging existing trusted relationships between systems to gain access to another system.

**Port redirection** – using a compromised host to forward traffic to another target.

### Social Engineering
Manipulating humans rather than exploiting technical vulnerabilities.

| Technique | Description |
|---|---|
| **Phishing** | Fraudulent email mimicking a trusted entity |
| **Spear phishing** | Personalised phishing using researched details about the victim |
| **Whaling** | Spear phishing targeting senior executives (CEO, CFO) |
| **Vishing** | Voice phishing via phone calls |
| **Smishing** | SMS phishing |
| **Pretexting** | Fabricating a convincing scenario ("I'm from IT support...") |
| **Baiting** | Leaving infected USB drives in car parks/lobbies |
| **Tailgating/Piggybacking** | Following an authorised person through a secure door |
| **Quid pro quo** | Offering a service/item in exchange for credentials or access |
| **Watering hole** | Infecting websites known to be visited by targets |

> 💡 Social engineering exploits human psychology: **urgency, authority, fear, scarcity, and helpfulness.** The best defences are awareness training and strict verification procedures.

## 2.5 Network Attacks – Denial of Service, Buffer Overflows, and Evasion

### Denial of Service (DoS)
Goal: make a resource unavailable to legitimate users.

| Attack | Mechanism |
|---|---|
| **SYN Flood** | Sends many SYN packets; never completes handshake; exhausts connection table |
| **Ping of Death** | Oversized ICMP packets cause buffer overflow on target |
| **Smurf Attack** | ICMP echo-request to broadcast address with spoofed source (victim IP) |
| **UDP Flood** | Floods target with UDP packets to random ports |
| **HTTP Flood** | Overwhelming web server with GET/POST requests |

### Distributed DoS (DDoS)
- Attack originates from thousands/millions of compromised hosts (botnet)
- Much harder to filter because traffic comes from many legitimate IPs
- **Amplification attacks** – small request with spoofed source IP → large response sent to victim
  - **DNS amplification** – 60x amplification factor
  - **NTP amplification** – up to 556x amplification factor

### Buffer Overflow
- Writing data **beyond the allocated memory buffer**
- Overwrites adjacent memory — can overwrite **return addresses** to redirect execution
- Allows an attacker to execute arbitrary code (Remote Code Execution / RCE)
- Common in C/C++ code lacking bounds checking
- Mitigation: Address Space Layout Randomisation (ASLR), Data Execution Prevention (DEP), stack canaries, safe coding practices

### Evasion Techniques
Attackers use evasion to avoid detection by IDS/IPS and firewalls:

| Technique | Description |
|---|---|
| **Fragmentation** | Split malicious payload across multiple small packets |
| **Encryption/tunnelling** | Encrypt C2 traffic or tunnel it over HTTPS/DNS |
| **Protocol-level evasion** | Exploit ambiguity in how different systems interpret protocols |
| **Traffic obfuscation** | Encode/scramble traffic to avoid signature matching |
| **Session splicing** | Send parts of an attack across multiple TCP sessions |
| **Flooding** | Overwhelm IDS with noise to hide the real attack |
| **Pivoting** | Compromise one host and attack others from inside the network |

## 2.6 Network Threats Summary

> Threats come from many actor types using a broad range of tools. Malware exploits systems; social engineering exploits people. DoS attacks target availability; access attacks target confidentiality and integrity. Evasion techniques specifically target security tools — a layered defence is essential.

---

# Module 3: Mitigating Threats

## 3.1 Defending the Network

**Defence-in-Depth:** Layer multiple, overlapping security controls so that the failure of any one layer does not result in a total breach.

```
Internet
   ↓ [Perimeter Firewall / NGFW / IPS]
   ↓ [DMZ — Web, Mail, DNS servers]
   ↓ [Internal Firewall]
   ↓ [Router ACLs / Segmentation]
   ↓ [Switch — Port security, DHCP snooping, DAI]
   ↓ [Endpoint — AV, HIPS, EDR, Encryption]
   ↓ [Data — Encryption, Access Control, DLP]
```

**Core security principles:**
- **Least privilege** – grant only the minimum access necessary
- **Minimum attack surface** – disable all unused services, ports, and features
- **Separation of duties** – no single person controls a critical process end-to-end
- **Need to know** – access to data restricted to those who require it for their role

## 3.2 Network Security Policies

A **security policy** is a formal document defining rules and procedures for protecting an organisation's assets.

**Key policy types:**

| Policy | Content |
|---|---|
| **Acceptable Use Policy (AUP)** | What users may and may not do with systems |
| **Data Classification Policy** | Sensitivity labels: Public / Internal / Confidential / Restricted |
| **Password Policy** | Minimum length, complexity, expiry, no reuse |
| **Incident Response Policy** | Steps for detecting, containing, eradicating, and recovering from incidents |
| **Remote Access Policy** | Requirements for VPN, device compliance |
| **Bring Your Own Device (BYOD) Policy** | Rules for personal devices accessing corporate resources |

**Data classification example:**

| Level | Description | Example |
|---|---|---|
| **Public** | Can be freely shared | Marketing materials, press releases |
| **Internal** | For employees only | Org charts, internal procedures |
| **Confidential** | Limited distribution; business impact if disclosed | Financial data, contracts |
| **Restricted** | Highly sensitive; strict access; regulatory implications | PII, health records, credentials |

## 3.3 Security Tools, Platforms, and Services

**Security tools categories:**

| Category | Examples | Purpose |
|---|---|---|
| **Firewall** | Cisco ASA, Firepower | Traffic enforcement, stateful inspection |
| **IDS/IPS** | Snort, Cisco Firepower IPS | Detect/block attacks |
| **SIEM** | Splunk, IBM QRadar, Sentinel | Centralised log correlation and alerting |
| **Vulnerability scanner** | Nessus, OpenVAS | Find known vulnerabilities |
| **Penetration testing** | Metasploit, Kali Linux | Authorised attack simulation |
| **EDR** | Cisco AMP, CrowdStrike | Advanced endpoint detection and response |
| **NAC** | Cisco ISE | Enforce endpoint compliance before network access |
| **DLP** | Symantec, Forcepoint | Prevent unauthorised data exfiltration |

## 3.4 Mitigating Common Network Attacks

| Attack | Mitigation |
|---|---|
| Phishing | Security awareness training, email filtering, MFA |
| Password attacks | Account lockout, MFA, password policies, salted hashing |
| DoS/DDoS | Rate limiting, upstream ISP filtering, scrubbing centres |
| Buffer overflow | ASLR, DEP, stack canaries, secure coding, patching |
| Reconnaissance | Disable unnecessary services, close unused ports, firewall |
| ARP spoofing | Dynamic ARP Inspection (DAI) |
| DHCP attacks | DHCP Snooping |
| VLAN hopping | Disable DTP, set unused native VLAN |
| STP attacks | BPDU Guard, Root Guard |
| IP spoofing | Unicast Reverse Path Forwarding (uRPF), BCP38 |

## 3.5 Cisco Network Foundations Protection Framework

Cisco's **NFP (Network Foundations Protection)** framework divides the router into three planes, each requiring its own security:

| Plane | What it is | Security Measures |
|---|---|---|
| **Management plane** | Traffic to/from the router itself (SSH, SNMP, syslog) | SSH only, ACLs on VTY, AAA, SNMPv3 |
| **Control plane** | Routing protocol traffic, OSPF, BGP updates | Routing protocol authentication, CoPP |
| **Data plane** | User traffic transiting the router | ACLs, firewall, IPS, uRPF, QoS |

**Control Plane Policing (CoPP):**
- Protects the router's CPU from being overwhelmed by excessive control-plane traffic
- Rate-limits traffic destined to the router (e.g., OSPF hello floods, ICMP)

```
! Example: CoPP to rate-limit ICMP to the router
class-map match-any ICMP-CLASS
 match protocol icmp

policy-map COPP-POLICY
 class ICMP-CLASS
  police rate 1000 pps

control-plane
 service-policy input COPP-POLICY
```

## 3.6 Mitigating Threats Summary

> No single tool or technique can stop all attacks. Defence-in-depth, clear policies, the right tools, and a trained team together form an effective security posture. Cisco's NFP framework reminds us that the router itself — its management, control, and data planes — must each be explicitly protected.

---

# Module 4: Secure Device Access

## 4.1 Secure the Edge Router

The **edge router** is the first (and often only) device between the organisation and the Internet. If it is compromised, all traffic can be intercepted or manipulated.

**Hardening steps:**
1. Restrict physical access to the router
2. Remove unused interfaces and services
3. Apply the latest IOS patches
4. Change all default credentials
5. Restrict remote access to SSH only (never Telnet)
6. Apply ACLs to management access (restrict by source IP)
7. Enable logging to a remote syslog server

## 4.2 Configure Secure Administrative Access

### Password Security

```
! Use enable secret (hashed with MD5/Scrypt) — never enable password
enable secret Str0ng!P@ssw0rd

! Encrypt all plaintext passwords in running-config
service password-encryption

! Enforce minimum password length
security passwords min-length 10

! Create local admin account
username admin privilege 15 secret LocalAdmin@123
```

### Console Port

```
line console 0
 login local
 exec-timeout 5 0          ! Auto-logout after 5 min idle
 logging synchronous        ! Prevent syslog messages interrupting input
```

### Auxiliary Port (AUX)

The AUX port is typically used for out-of-band modem access — usually not needed in modern networks:

```
line aux 0
 no exec
 transport input none
 exec-timeout 0 1
```

### Login Banners

```
banner motd ^
========================================================
  AUTHORISED ACCESS ONLY
  All activity on this system is monitored and logged.
  Unauthorised access is prohibited and will be
  reported to the appropriate authorities.
========================================================
^
```

> ⚠️ Never use "Welcome" or "Hello" in banners — this can legally imply consent to access and weaken prosecution of intruders. Banners must communicate that access is restricted and monitored.

## 4.3 Configure Enhanced Security for Virtual Logins

```
! Block logins for 120 seconds after 3 failures within 60 seconds
login block-for 120 attempts 3 within 60

! Optional: allow only specific hosts to bypass block (management subnet)
login quiet-mode access-class MGMT-HOSTS

! Log login failures and successes
login on-failure log
login on-success log
```

**Verify:**
```
show login                    ! Current login block status
show login failures           ! Recent failed login attempts
```

## 4.4 Configure SSH

SSH replaces Telnet for encrypted, authenticated remote management.

```
! Step 1: Set hostname and domain name
hostname R1
ip domain-name company.com

! Step 2: Generate RSA key pair (2048-bit minimum)
crypto key generate rsa modulus 2048

! Step 3: Enforce SSH version 2 only
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 3

! Step 4: Restrict VTY lines to SSH only
line vty 0 4
 login local
 transport input ssh
 exec-timeout 5 0

! Verify SSH is enabled
show ip ssh
show ssh
```

**Comparing Telnet vs SSH:**

| Feature | Telnet | SSH |
|---|---|---|
| Encryption | None — plaintext | Yes — encrypted |
| Authentication | Password only | Password + keys |
| Port | TCP 23 | TCP 22 |
| Security | ❌ Never use in production | ✅ Always preferred |

## 4.5 Secure Device Access Summary

> Securing a device starts with strong passwords, removing default credentials, disabling unused services, and restricting management access to SSH only. Login blocking and enhanced authentication logging add additional protection against brute force attacks. Physical security and appropriate banners complete the baseline.

---

# Module 5: Assigning Administrative Roles

## 5.1 Configure Privilege Levels

IOS has **16 privilege levels (0–15)** to control what commands a user may execute.

| Level | Description |
|---|---|
| **0** | Minimal access: `disable`, `enable`, `exit`, `help`, `logout` only |
| **1** | User EXEC (`>` prompt) — default; read-only, limited commands |
| **2–14** | Custom; administrator-defined |
| **15** | Privileged EXEC (`#` prompt) — full access to all commands |

```
! Assign commands to a custom level
privilege exec level 5 show running-config
privilege exec level 5 show ip interface brief
privilege exec level 5 ping
privilege exec level 5 traceroute

! Create user at that level
username netops privilege 5 secret NetOps@P@ss

! Set the enable password for level 5
enable secret level 5 Level5@P@ss

! Apply privilege level to VTY lines
line vty 0 4
 login local
 privilege level 5
```

**Verify:**
```
show privilege              ! Current privilege level
```

**Limitations of privilege levels:**
- Cannot restrict specific subcommands of a command (e.g., allow `show interfaces` but not `show running-config`)
- No concept of grouping or delegation hierarchy
- CLI Views solve these limitations

## 5.2 Configure Role-Based CLI

**CLI Views** provide precise, command-level access control — more granular than privilege levels.

**View types:**
- **Root view** – Has access to all IOS commands; required to create/manage other views; accessed with `enable view`
- **CLI view** – Custom named set of permitted commands
- **Superview** – A collection of CLI views (users inherit commands from all included views)

**Configuration:**

```
! Step 1: Enable AAA (required for views)
aaa new-model

! Step 2: Enter root view
enable view             ! Prompted for enable secret

! Step 3: Create a CLI view
parser view HELPDESK
 secret HelpDesk@P@ss
 commands exec include show version
 commands exec include show interfaces
 commands exec include show ip interface brief
 commands exec include ping
 commands exec include exit

! Step 4: Create a superview combining views
parser view NETWORK-ADMIN superview
 secret NetAdmin@P@ss
 view HELPDESK             ! Inherits all commands from HELPDESK view
```

**Switching to a view:**
```
enable view HELPDESK      ! Enter a specific view (prompted for view password)
show parser view          ! Show current active view
show parser view all      ! List all configured views
```

**Privilege levels vs CLI Views:**

| Feature | Privilege Levels | CLI Views |
|---|---|---|
| Granularity | Coarse (command families) | Fine (exact commands and subcommands) |
| Subcommand control | No | Yes |
| Grouping (superviews) | No | Yes |
| Ease of setup | Simple | More complex |
| Recommended for | Small networks | Enterprise environments |

## 5.3 Assigning Administrative Roles Summary

> Privilege levels and CLI views both provide role-based access control. CLI views are more powerful — allowing per-command restrictions and logical grouping via superviews. For any network where multiple administrators with different responsibilities manage devices, proper role assignment is essential to enforce least privilege.

# Module 6: Device Monitoring and Management

## 6.1 Secure Cisco IOS Image and Configuration Files

Protecting the IOS image and configuration files prevents an attacker from replacing firmware or stealing the running configuration.

```
! Enable IOS image resilience (stores a secure copy of IOS and startup-config)
secure boot-image
secure boot-config

! Verify
show secure bootset
```

**Configuration archiving:**
```
! Archive running config to TFTP automatically
archive
 path tftp://10.0.0.100/$h-config     ! $h = hostname
 write-memory
 time-period 1440                      ! Archive every 24 hours (in minutes)

! Manually back up configuration
copy running-config tftp://10.0.0.100/r1-backup.cfg
```

**File system integrity:**
```
! Verify MD5 hash of IOS image to ensure it hasn't been tampered with
verify /md5 flash:/c1900-universalk9-mz.SPA.154-3.M2.bin
```

## 6.2 Lock Down a Router Using AutoSecure

**AutoSecure** is a Cisco IOS feature that automatically applies a security hardening baseline in a single command.

```
auto secure           ! Interactive mode — asks questions about your network
auto secure no-interact   ! Non-interactive — applies defaults without prompts
```

**What AutoSecure does:**
- Disables unnecessary services (Finger, HTTP, CDP, BOOTp, PAD, IP source route)
- Enables `service password-encryption`
- Enables `login block-for` for VTY anti-brute-force
- Configures SSH and disables Telnet
- Enables TCP intercept and uRPF where applicable
- Applies basic ACLs to block RFC 1918 and bogon addresses
- Enables `security passwords min-length`
- Sets `exec-timeout` on console and VTY

> 💡 AutoSecure is useful as a quick baseline, but always review the resulting config — it may disable features you actually need (like CDP for VoIP).

## 6.3 Routing Protocol Authentication

Without authentication, an attacker can inject false routing information (route poisoning / BGP hijacking) to redirect or drop traffic.

### OSPF MD5 Authentication

```
! Enable MD5 authentication on OSPF interface
interface GigabitEthernet0/0
 ip ospf message-digest-key 1 md5 OSPFSecret@123
 ip ospf authentication message-digest

! Or configure per OSPF area (applies to all interfaces in area)
router ospf 1
 area 0 authentication message-digest
```

### EIGRP Authentication

```
! Create key chain
key chain EIGRP-KEYS
 key 1
  key-string EIGRPSecret@123

! Apply to interface
interface GigabitEthernet0/0
 ip authentication mode eigrp 100 md5
 ip authentication key-chain eigrp 100 EIGRP-KEYS
```

### RIPv2 Authentication

```
key chain RIP-KEYS
 key 1
  key-string RIPSecret@123

interface GigabitEthernet0/0
 ip rip authentication mode md5
 ip rip authentication key-chain RIP-KEYS
```

> 💡 SHA-based authentication (HMAC-SHA) is available in newer IOS versions and preferred over MD5 for OSPF.

## 6.4 Secure Management and Reporting

**Principles:**
- Management traffic should use a **dedicated out-of-band (OOB) management network** wherever possible
- If OOB is not possible, use an **encrypted in-band** protocol (SSH, SNMPv3, HTTPS)
- Restrict management access by source IP using ACLs
- Send all logs to a centralised, protected **syslog server**

```
! Restrict SSH access to management subnet only
access-list 10 permit 10.10.10.0 0.0.0.255
access-list 10 deny any log

line vty 0 4
 access-class 10 in
 login local
 transport input ssh
```

## 6.5 Network Security Using Syslog

**Syslog** provides centralised logging from network devices.

**Syslog Severity Levels (0–7):**

| Level | Name | Description | Memory Aid |
|---|---|---|---|
| 0 | **Emergency** | System unusable | **E**very |
| 1 | **Alert** | Immediate action required | **A**wful |
| 2 | **Critical** | Critical conditions | **C**isco |
| 3 | **Error** | Error conditions | **E**ngineer |
| 4 | **Warning** | Warning conditions | **W**ill |
| 5 | **Notice** | Normal but significant | **N**eed |
| 6 | **Informational** | Informational messages | **I**ce |
| 7 | **Debug** | Debug-level (very verbose) | **D**aily |

```
! Send logs to syslog server (UDP 514 by default)
logging host 10.0.0.100
logging trap warnings         ! Log severity 4 (Warning) and above (levels 0-4)
logging source-interface Loopback0
logging on

! Add millisecond timestamps to every log message
service timestamps log datetime msec
service timestamps debug datetime msec

! Buffer logs locally (useful for recent events if syslog server is down)
logging buffered 16384 debugging

! Verify
show logging
```

> 💡 `logging trap warnings` means levels 0–4. Lower numbers = higher severity. "Trap level X" means log X and everything more severe (lower number).

## 6.6 NTP Configuration

**NTP (Network Time Protocol)** ensures all devices have synchronised clocks — essential for:
- **Log correlation** across devices (events at the same moment should share the same timestamp)
- **Certificate validity** checks (certificates have Not Before / Not After fields)
- **Kerberos authentication** (requires clocks within 5 minutes of each other)
- **Forensic investigation** accuracy

```
! Configure as NTP client
ntp server 10.0.0.1

! NTP Authentication (prevents accepting time from rogue NTP servers)
ntp authenticate
ntp authentication-key 1 md5 NTPSecret@Key
ntp trusted-key 1
ntp server 10.0.0.1 key 1

! Restrict which devices can query this device as an NTP server
ntp access-group peer 10         ! ACL 10 defines permitted NTP peers

! Verify
show ntp status                  ! Sync status and reference clock
show ntp associations            ! List of NTP peers and their status
```

**NTP stratum:**
- **Stratum 0** – Atomic clocks, GPS (reference clocks)
- **Stratum 1** – Servers directly connected to stratum 0 sources
- **Stratum 2** – Synced from stratum 1
- Higher stratum = more hops from reference = less accurate

## 6.7 SNMP Configuration

**SNMP** allows monitoring and management of network devices.

**Components:**
- **NMS (Network Management Station)** – collects and displays SNMP data (e.g., SolarWinds, PRTG)
- **Agent** – software on the managed device that stores MIB data
- **MIB (Management Information Base)** – structured database of device attributes
- **OID (Object Identifier)** – unique dotted-number ID for each MIB variable

**SNMP Operations:**

| Operation | Description |
|---|---|
| `GET` | NMS requests a specific value from agent |
| `GET-NEXT` | NMS walks through MIB table |
| `SET` | NMS changes a value on the device |
| `TRAP` | Agent sends unsolicited notification to NMS |
| `INFORM` | Like TRAP but requires acknowledgement (v2c/v3) |

**SNMP Versions:**

| Version | Security | Use |
|---|---|---|
| v1 | Community string in cleartext | ❌ Never |
| v2c | Community string in cleartext (improved MIB) | ❌ Avoid |
| **v3** | Authentication (HMAC) + Privacy (AES encryption) | ✅ Always |

```
! SNMPv3 configuration (recommended)
snmp-server group SECUREGROUP v3 priv                         ! Require auth + encryption
snmp-server user SNMPUSER SECUREGROUP v3 auth sha AuthP@ss priv aes 128 PrivP@ss
snmp-server host 10.0.0.50 version 3 priv SNMPUSER

! Restrict SNMP to NMS only
access-list 20 permit host 10.0.0.50
snmp-server community READONLY ro 20                          ! If v2c must be used

! Enable SNMP traps
snmp-server enable traps
snmp-server trap-source Loopback0

! Verify
show snmp
show snmp user
```

## 6.8 Device Monitoring and Management Summary

> Securing device management means protecting the IOS image, using encrypted management protocols, locking down access with ACLs, and feeding all logs to a centralised syslog server. NTP provides the accurate time foundation for log correlation and certificate validity. SNMPv3 is the only acceptable version for production SNMP monitoring.

---

# Module 7: Authentication, Authorization, and Accounting (AAA)

## 7.1 AAA Characteristics

**AAA** is the framework for controlling access to network resources.

| Component | Question | Purpose |
|---|---|---|
| **Authentication** | Who are you? | Verify identity before granting access |
| **Authorization** | What can you do? | Define what an authenticated entity is permitted to do |
| **Accounting** | What did you do? | Record all activity for audit and compliance |

**Without AAA:**
- Local username/password on each device
- No centralised policy
- No unified audit trail
- Unscalable beyond a handful of devices

**With AAA:**
- Centralised policy management
- Single change propagates to all devices
- Full audit trail with timestamps, commands executed, session duration

## 7.2 Configure Local AAA Authentication

Local AAA uses credentials stored on the device itself — suitable for small networks or as a fallback.

```
! Enable AAA
aaa new-model

! Create local user database
username admin privilege 15 secret Admin@P@ss
username netops privilege 7 secret NetOps@P@ss

! Create authentication method list
! "default" applies to all lines unless a named list is specified
aaa authentication login default local

! Named list (apply selectively to specific lines)
aaa authentication login VTY-AUTH local

line vty 0 4
 login authentication VTY-AUTH
```

**Method list fallback:**
```
! Try local first; if local has no entry, deny (none = allow without auth — risky!)
aaa authentication login default local none
! Better — if local fails, also fail (no fallback to no-auth):
aaa authentication login default local
```

## 7.3 Server-Based AAA Characteristics and Protocols

### RADIUS (Remote Authentication Dial-In User Service)
- RFC 2865/2866 open standard
- **UDP ports:** 1812 (authentication), 1813 (accounting)
- Encrypts **only the password** field in the Access-Request packet
- **Combines authentication and authorisation** in a single Accept/Reject response
- Best for **network access** (802.1X, VPN, Wi-Fi, remote dial-up)

### TACACS+ (Terminal Access Controller Access-Control System Plus)
- Cisco proprietary (though widely supported)
- **TCP port 49**
- Encrypts the **entire payload** of every packet
- **Separates** Authentication, Authorisation, and Accounting into independent functions
- Allows granular **per-command authorisation**
- Best for **device administration** (managing routers, switches, firewalls)

**Comparison table:**

| Feature | RADIUS | TACACS+ |
|---|---|---|
| Standard | Open RFC | Cisco proprietary |
| Transport | UDP | TCP |
| Port | 1812/1813 | 49 |
| Encryption | Password only | Full packet |
| AAA separation | No (combined) | Yes (fully separated) |
| Command authorisation | Limited | Granular |
| Multiprotocol support | Limited | Yes |
| Best for | Network access | Device admin |

## 7.4 Configure Server-Based Authentication

```
! Define TACACS+ server
aaa new-model

tacacs server ISE-TACACS
 address ipv4 10.0.0.50
 key TacacsSecret@123
 timeout 5

! Define RADIUS server (alternative)
radius server ISE-RADIUS
 address ipv4 10.0.0.51 auth-port 1812 acct-port 1813
 key RadiusSecret@123

! Authentication method list — try TACACS+ first, fall back to local
aaa authentication login default group tacacs+ local

! Apply to console (keep local for console as safety net)
line console 0
 login authentication default

line vty 0 4
 login authentication default
```

## 7.5 Configure Server-Based Authorization and Accounting

```
! Authorisation — what exec commands a user can run
aaa authorization exec default group tacacs+ local if-authenticated
aaa authorization commands 15 default group tacacs+ local if-authenticated

! Accounting — log session start/stop and commands to TACACS+ server
aaa accounting exec default start-stop group tacacs+
aaa accounting commands 15 default start-stop group tacacs+

! Verify AAA
show aaa servers
show aaa sessions
show tacacs
```

**Key authorisation keywords:**
- `if-authenticated` – grant local exec if TACACS+ server is unreachable and user authenticated locally
- `none` – allow access even without authorisation being checked (dangerous)

## 7.6 AAA Summary

> AAA centralises and standardises access control across all network devices. TACACS+ is preferred for device administration due to full packet encryption and granular command authorisation. RADIUS is preferred for network access (802.1X, VPN). Always configure a local fallback so administrators can still log in if the AAA server is unreachable.

---

# Module 8: Access Control Lists

## 8.1 Introduction to Access Control Lists

An **ACL** is an ordered list of **ACEs (Access Control Entries)** that the router processes top-to-bottom to permit or deny packets.

**Key principles:**
- **First match wins** — processing stops at the first matching ACE
- **Implicit deny all** — every ACL ends with an invisible `deny any any`; any packet not explicitly permitted is dropped
- Applied to an **interface** in a **direction**:
  - **Inbound (in):** ACL processed before routing; applied to incoming packets
  - **Outbound (out):** ACL processed after routing; applied to outgoing packets
- One ACL per interface, per protocol, per direction

**When to use ACLs:**
- Permit or deny traffic between networks
- Restrict management access (VTY, SNMP)
- Define "interesting traffic" for VPNs or QoS
- Limit routing protocol updates

## 8.2 Wildcard Masking

Wildcard masks define which bits of an IP address must match.
- **0 bit** = this bit must match exactly
- **1 bit** = ignore this bit (any value)

Wildcard mask = **inverse of the subnet mask** (255.255.255.255 minus the subnet mask).

| Subnet | Subnet Mask | Wildcard Mask | Matches |
|---|---|---|---|
| Specific host /32 | 255.255.255.255 | 0.0.0.0 | One host (`host` keyword) |
| /24 network | 255.255.255.0 | 0.0.0.255 | All 256 addresses in a /24 |
| /16 network | 255.255.0.0 | 0.0.255.255 | All addresses in a /16 |
| /8 network | 255.0.0.0 | 0.255.255.255 | All addresses in a /8 |
| Any | 0.0.0.0 | 255.255.255.255 | Any address (`any` keyword) |

**IOS shortcuts:**
```
permit host 192.168.1.5       ! = permit 192.168.1.5 0.0.0.0
deny any                      ! = deny 0.0.0.0 255.255.255.255
```

## 8.3 Configure ACLs

### Standard ACLs
- Match **source IP address only**
- Numbered 1–99 and 1300–1999
- Place **close to the destination** (to avoid blocking traffic unnecessarily)

```
! Numbered standard ACL
access-list 10 remark Permit HR subnet to payroll server
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 deny any log

interface GigabitEthernet0/1
 ip access-group 10 out

! Named standard ACL
ip access-list standard MGMT-ACCESS
 remark Permit only management hosts
 permit 10.10.10.0 0.0.0.255
 deny any log
```

### Extended ACLs
- Match **source IP, destination IP, protocol, and port**
- Numbered 100–199 and 2000–2699
- Place **close to the source** (blocks unwanted traffic early, saves bandwidth)

```
! Permit HTTP/HTTPS from any source to web server; deny all else
access-list 110 remark Allow web traffic to server
access-list 110 permit tcp any host 10.0.1.10 eq 80
access-list 110 permit tcp any host 10.0.1.10 eq 443
access-list 110 deny ip any any log

interface GigabitEthernet0/0
 ip access-group 110 in

! Named extended ACL
ip access-list extended FILTER-OUTBOUND
 remark Block Telnet
 deny tcp any any eq 23 log
 remark Block unencrypted FTP
 deny tcp any any eq 21 log
 remark Permit established TCP return traffic
 permit tcp any any established
 remark Permit all other traffic
 permit ip any any

interface GigabitEthernet0/1
 ip access-group FILTER-OUTBOUND out
```

**Common port numbers to know:**

| Protocol | Port |
|---|---|
| FTP data | TCP 20 |
| FTP control | TCP 21 |
| SSH | TCP 22 |
| Telnet | TCP 23 |
| SMTP | TCP 25 |
| DNS | UDP/TCP 53 |
| HTTP | TCP 80 |
| HTTPS | TCP 443 |
| SNMP | UDP 161 |
| Syslog | UDP 514 |

## 8.4 Modify ACLs

**Named ACLs** support editing with sequence numbers:

```
! View current ACL with sequence numbers
show ip access-lists FILTER-OUTBOUND

! Delete a specific ACE by sequence number
ip access-list extended FILTER-OUTBOUND
 no 30                         ! Remove ACE at sequence 30

! Insert a new ACE at a specific position
ip access-list extended FILTER-OUTBOUND
 15 deny tcp any any eq 3389 log   ! Block RDP — insert at sequence 15

! Numbered ACLs must be deleted and recreated to modify
no access-list 110
access-list 110 ...            ! Rewrite from scratch
```

## 8.5 Implement ACLs

**Best practices:**
- Put **more specific** rules before **general** rules
- Always add **remarks** for documentation: `access-list 110 remark Block Telnet`
- Use **named ACLs** — they are easier to read and edit
- Add **log** keyword to important deny statements for visibility
- Regularly review and remove stale ACEs
- Avoid having both inbound and outbound ACLs on same interface unless necessary

**Testing ACLs:**
```
show ip access-lists                     ! View all ACLs with match counters
show ip access-lists FILTER-OUTBOUND     ! Specific ACL
show ip interface GigabitEthernet0/0     ! See which ACLs are applied to interface
clear ip access-list counters            ! Reset hit counters for testing
```

## 8.6 Mitigate Attacks with ACLs

ACLs can filter many common attacks at the network layer:

```
ip access-list extended ANTI-SPOOFING
 ! Block RFC 1918 private addresses arriving on the outside interface (ingress filtering)
 deny ip 10.0.0.0 0.255.255.255 any log
 deny ip 172.16.0.0 0.15.255.255 any log
 deny ip 192.168.0.0 0.0.255.255 any log
 ! Block loopback arriving from outside
 deny ip 127.0.0.0 0.255.255.255 any log
 ! Block IP packets with source = own network (spoofing)
 deny ip 203.0.113.0 0.0.0.255 any log
 permit ip any any

interface GigabitEthernet0/1
 ip access-group ANTI-SPOOFING in      ! Apply inbound on outside interface

! Block Telnet from outside entirely
ip access-list extended NO-TELNET
 deny tcp any any eq 23 log
 permit ip any any
```

## 8.7 IPv6 ACLs

IPv6 ACLs are **named only** (no numbered IPv6 ACLs exist). The syntax is similar to extended IPv4 ACLs.

**Critical difference:** IPv6 ACLs must **explicitly permit NDP (Neighbor Discovery Protocol)** messages — otherwise IPv6 communication will break entirely. NDP replaces ARP in IPv6.

```
ipv6 access-list IPv6-FILTER
 ! Must explicitly permit NDP
 permit icmp any any nd-na           ! Neighbor Advertisement
 permit icmp any any nd-ns           ! Neighbor Solicitation
 permit icmp any any router-advertisement
 permit icmp any any router-solicitation
 ! Permit desired traffic
 permit tcp any any eq 443
 permit tcp any any eq 22
 ! Block everything else
 deny ipv6 any any log

interface GigabitEthernet0/0
 ipv6 traffic-filter IPv6-FILTER in
```

## 8.8 ACLs Summary

> ACLs are a fundamental tool for enforcing traffic policy. Know the difference between standard (source only) and extended (source, destination, protocol, port) ACLs. Wildcard masks define address matching. Named ACLs are preferred in production. Always account for the implicit deny at the end, and explicitly permit NDP in IPv6 ACLs.

---

# Module 9: Firewall Technologies

## 9.1 Secure Networks with Firewalls

A **firewall** controls traffic flow between network segments based on a configured security policy.

**What firewalls provide:**
- Permit or deny traffic (access control)
- Track connection state (stateful)
- Network Address Translation (NAT/PAT)
- Logging of allowed and denied traffic
- First line of defence at network boundaries

**What firewalls do NOT protect against (without additional features):**
- Encrypted malware in HTTPS traffic (requires SSL inspection)
- Insider threats (traffic that doesn't cross the firewall boundary)
- Application-layer attacks on permitted services (needs NGFW/IPS)

### Firewall Types

**Packet Filtering (Stateless):**
- Examines Layer 3–4 headers only (IP, protocol, port)
- No connection tracking — each packet evaluated independently
- Fast but limited — cannot distinguish new from return traffic
- Implemented as ACLs on routers
- Susceptible to IP spoofing, fragmentation, and return-traffic attacks

**Stateful Inspection Firewall:**
- Maintains a **state table** of active connections
- Automatically allows return traffic matching established sessions
- Much more secure than stateless — blocks unsolicited inbound traffic
- Default mode for most modern firewalls (ASA, Firepower)
- Cannot inspect application-layer content

**Application Layer Gateway (Proxy Firewall):**
- Operates at Layer 7 (application layer)
- **Full proxy** — terminates the client connection, inspects content, re-originates to server
- Can deep-inspect and filter protocol commands (e.g., block FTP PUT but allow FTP GET)
- Protocol-aware: HTTP, FTP, DNS, SMTP
- Higher latency due to processing overhead

**Next-Generation Firewall (NGFW):**
- Combines stateful inspection + application identification + integrated IPS
- **Application ID** — identifies apps regardless of port (blocks Tor even on port 443)
- **User identity** — integrates with Active Directory / Cisco ISE
- **SSL/TLS inspection** — decrypt, inspect, re-encrypt (requires certificate deployment)
- **Threat intelligence** integration (Talos, reputation feeds)
- Examples: Cisco Firepower, Palo Alto PA-series, Fortinet FortiGate

**Unified Threat Management (UTM):**
- All-in-one appliance: Firewall + IPS + AV + URL filtering + VPN + email filter
- Common in SMB environments
- Simpler to manage; single point of failure

## 9.2 Firewalls in Network Design

### DMZ (Demilitarised Zone)

A **DMZ** is a semi-trusted network segment hosting public-facing servers, isolated from both the untrusted Internet and the trusted internal LAN.

```
Internet ──→ [External Firewall] ──→ DMZ ──→ [Internal Firewall] ──→ Internal LAN
                                      │
                               Web / Mail / DNS
                            (Public-facing servers)
```

**Traffic flow rules:**
- Internet → DMZ: Permitted for specific services only (HTTP 80, HTTPS 443, SMTP 25)
- DMZ → LAN: **Denied by default** — a compromised DMZ server must not reach the LAN
- LAN → DMZ: Permitted (internal users access DMZ resources)
- DMZ → Internet: Permitted for updates, outbound mail (restricted)

**Single vs Dual Firewall:**

| Design | Description | Security Level |
|---|---|---|
| **Single firewall (3-leg)** | One firewall, three interfaces: outside/DMZ/inside | Moderate — all eggs in one basket |
| **Dual firewall** | Separate firewalls for outside→DMZ and DMZ→inside | High — independent failure domains |

### Transparent (Bridged) Firewall
- Acts as a Layer 2 bridge — invisible to the routing infrastructure
- No IP address assigned to firewall interfaces
- Easy to insert without redesigning IP addressing
- Still enforces full stateful inspection

### Firewall Best Practices
- Deny all by default; permit only what is explicitly needed
- Log all denied traffic
- Keep firewall OS and signatures updated
- Regularly audit firewall rules; remove stale entries
- Never manage the firewall from the untrusted interface

## 9.3 Firewall Technologies Summary

> Firewalls range from simple packet filters (ACLs) to full NGFWs with application awareness, user identity, and integrated IPS. The DMZ architecture is the standard model for hosting public-facing services while protecting the internal network. Always follow the principle of deny-by-default.

# Module 10: Zone-Based Policy Firewalls

## 10.1 ZPF Overview

**Zone-Based Policy Firewall (ZPF)** transforms a Cisco IOS router into a full stateful firewall using a zone-and-policy model. It supersedes the older **CBAC (Context-Based Access Control)** model.

**Key advantages over classic IOS firewall (CBAC):**
- Intuitive zone model — policies between groups of interfaces
- Not affected by ACLs on the same interface
- Default-deny between zones (more secure default)
- A single interface can only belong to **one zone**

**Core concepts:**

| Concept | Description |
|---|---|
| **Zone** | A logical group of interfaces with the same trust level |
| **Zone-pair** | Directional relationship between two zones — has a policy applied |
| **Self zone** | Special zone representing the router itself (traffic to/from the router) |
| **Class map** | Defines what traffic to match |
| **Policy map** | Defines what action to take on matched traffic |
| **Service policy** | Binds a policy map to a zone-pair |

**Traffic behaviour between zones:**

| Scenario | Default Behaviour |
|---|---|
| Same zone | Permitted (no inspection needed) |
| Different zones (no zone-pair) | **Dropped** — traffic denied |
| Different zones (zone-pair with policy) | Controlled by policy |
| Interface not in a zone | Cannot communicate with zoned interfaces |

## 10.2 ZPF Operation

**Three actions available in policy maps:**

| Action | Description |
|---|---|
| **Inspect** | Stateful tracking — return traffic automatically permitted |
| **Pass** | Permit traffic without state tracking (no automatic return) |
| **Drop** | Deny traffic (optionally with log) |

**Processing flow:**
1. Packet arrives on interface → find zone membership
2. Find the zone-pair: source zone → destination zone
3. If no zone-pair exists → **drop**
4. Look up class map in policy map → apply matching action

**Important:** Zone-pairs are **unidirectional**. To allow bidirectional traffic using `pass`, you need two zone-pairs. Using `inspect`, the return traffic is handled automatically.

## 10.3 Configure a ZPF

```
! ─────────────────────────────────────────
! Step 1: Define zones
! ─────────────────────────────────────────
zone security INSIDE
zone security OUTSIDE
zone security DMZ

! ─────────────────────────────────────────
! Step 2: Assign interfaces to zones
! ─────────────────────────────────────────
interface GigabitEthernet0/0
 zone-member security INSIDE

interface GigabitEthernet0/1
 zone-member security OUTSIDE

interface GigabitEthernet0/2
 zone-member security DMZ

! ─────────────────────────────────────────
! Step 3: Define class maps (match traffic)
! ─────────────────────────────────────────
class-map type inspect match-any INSIDE-ALLOWED-PROTOCOLS
 match protocol http
 match protocol https
 match protocol dns
 match protocol icmp
 match protocol smtp

class-map type inspect match-any OUTSIDE-TO-DMZ-PROTOCOLS
 match protocol http
 match protocol https

! ─────────────────────────────────────────
! Step 4: Define policy maps (what to do)
! ─────────────────────────────────────────
policy-map type inspect INSIDE-TO-OUTSIDE-POLICY
 class type inspect INSIDE-ALLOWED-PROTOCOLS
  inspect                        ! Stateful — return traffic auto-permitted
 class class-default
  drop log                       ! Drop everything else, log it

policy-map type inspect OUTSIDE-TO-DMZ-POLICY
 class type inspect OUTSIDE-TO-DMZ-PROTOCOLS
  inspect
 class class-default
  drop log

! ─────────────────────────────────────────
! Step 5: Create zone-pairs and apply policies
! ─────────────────────────────────────────
zone-pair security INSIDE-TO-OUTSIDE source INSIDE destination OUTSIDE
 service-policy type inspect INSIDE-TO-OUTSIDE-POLICY

zone-pair security OUTSIDE-TO-DMZ source OUTSIDE destination DMZ
 service-policy type inspect OUTSIDE-TO-DMZ-POLICY

! ─────────────────────────────────────────
! Self zone — protect the router itself
! ─────────────────────────────────────────
class-map type inspect match-any SELF-MGMT-TRAFFIC
 match protocol ssh
 match protocol ntp
 match protocol snmp

policy-map type inspect INSIDE-TO-SELF-POLICY
 class type inspect SELF-MGMT-TRAFFIC
  inspect
 class class-default
  drop

zone-pair security INSIDE-TO-SELF source INSIDE destination self
 service-policy type inspect INSIDE-TO-SELF-POLICY
```

## 10.4 Verify ZPF

```
show zone security                        ! List all zones and their interfaces
show zone-pair security                   ! List all zone-pairs and applied policies
show policy-map type inspect zone-pair    ! Detailed policy with match statistics
show class-map type inspect               ! View class maps
```

## 10.4 Zone-Based Firewalls Summary

> ZPF provides a clean, scalable firewall model on IOS routers. Traffic between zones is denied by default unless explicitly permitted by a policy. Use `inspect` for stateful tracking of TCP/UDP/ICMP sessions. Always configure the self zone to control management access to the router itself.

---

# Module 11: IPS Technologies

## 11.1 IDS and IPS Characteristics

**IDS (Intrusion Detection System):**
- Deployed **out-of-band** — receives a copy of traffic via SPAN/TAP
- **Detects and alerts** — cannot stop an attack in progress
- No impact on live traffic; latency is not a concern
- False positives generate alerts but don't block legitimate traffic

**IPS (Intrusion Prevention System):**
- Deployed **inline** — all traffic physically passes through the device
- **Detects and blocks** — can drop packets, reset connections, block IPs
- Adds some latency (must inspect before forwarding)
- False positives block legitimate traffic → must be tuned carefully

**Comparison:**

| Feature | IDS | IPS |
|---|---|---|
| Deployment | Out-of-band (passive) | Inline (active) |
| Response | Alert only | Block/drop/alert |
| Traffic impact | None | Adds latency |
| False positive impact | Alert noise | Blocks legitimate traffic |
| False negative impact | Attack missed | Attack succeeds |

> 💡 **IDS = smoke alarm. IPS = sprinkler system.**

### Detection Methods

**Signature-Based:**
- Compares traffic against a database of known attack patterns
- ✅ Low false positives for known attacks; fast processing
- ❌ Cannot detect zero-day attacks; requires constant signature updates
- Similar in concept to antivirus

**Anomaly-Based (Behaviour-Based):**
- Establishes a **baseline** of normal behaviour
- Alerts when deviation exceeds threshold
- ✅ Can detect novel and zero-day attacks
- ❌ Higher false positive rate; requires training period

**Policy-Based:**
- Alerts when traffic violates a defined rule regardless of content
- Example: "Alert any time Telnet is seen on the network"

### False Positive/Negative Matrix

| Result | Meaning | Impact |
|---|---|---|
| **True Positive** | Real attack, correctly detected | ✅ Desired |
| **True Negative** | Legitimate traffic, correctly passed | ✅ Desired |
| **False Positive** | Legitimate traffic flagged as attack | Alert fatigue; blocks legitimate users |
| **False Negative** | Real attack, not detected | Attack succeeds unnoticed |

## 11.2 IPS Implementations

| Type | Deployment | Coverage |
|---|---|---|
| **NIPS (Network-Based)** | Inline at network chokepoints | All passing network traffic |
| **HIPS (Host-Based)** | Agent on individual endpoints | Local system: calls, files, registry, processes |

**NIPS placement best practices:**
- **Outside the firewall** — see all external threats (high noise)
- **Inside the firewall** — see only threats that bypassed the firewall (more actionable)
- **In front of critical servers** — most targeted placement

**HIPS advantages:**
- Can inspect encrypted traffic (after decryption at the host)
- Can detect local anomalies: process injection, file changes, registry modifications
- Not dependent on network topology

## 11.3 IPS on Cisco ISRs

Cisco ISR (Integrated Services Routers) can run **IOS IPS** — a basic signature-based IPS integrated into IOS.

```
! Step 1: Create a directory for IPS signature files
mkdir flash:/ipsdir

! Step 2: Define IPS config and signature storage location
ip ips config location flash:/ipsdir
ip ips notify log              ! Send IPS alerts to syslog

! Step 3: Create an IPS rule (named)
ip ips name IOSIPS

! Step 4: Apply IPS rule to interface (inbound direction)
interface GigabitEthernet0/1
 ip ips IOSIPS in

! Step 5: Load appropriate signatures
ip ips signature-category
 category all
  retired true                 ! Disable all signatures first (reduce false positives)
 category ios_ips basic
  retired false                ! Enable only the basic category

! Verify
show ip ips all
show ip ips signature count
show ip ips statistics
```

## 11.4 Cisco Switched Port Analyzer (SPAN)

**SPAN (Switched Port Analyzer)** copies traffic from one or more source ports to a destination port where a monitoring device (IDS, Wireshark) is connected.

**Types:**
- **SPAN** – copies traffic within the same switch
- **RSPAN** – copies traffic to a remote switch over a trunk
- **ERSPAN** – copies traffic over IP (encapsulated in GRE) to a remote sensor

```
! Local SPAN — mirror port Gi0/1 to Gi0/10 (where IDS is connected)
monitor session 1 source interface GigabitEthernet0/1 both
monitor session 1 destination interface GigabitEthernet0/10

! SPAN entire VLAN
monitor session 2 source vlan 10 both
monitor session 2 destination interface GigabitEthernet0/10

show monitor session 1
```

> 💡 The destination SPAN port cannot communicate on the normal network while used for SPAN — it only receives the mirrored traffic.

## 11.5 IPS Technologies Summary

> IDS passively monitors and alerts; IPS actively blocks inline. Signature-based detection is accurate for known threats; anomaly-based can catch zero-days. SPAN/RSPAN allows traffic to be mirrored to IDS sensors without going inline. Cisco IOS IPS provides basic IPS capability on ISR routers.

---

# Module 12: IPS Operation and Implementation

## 12.1 IPS Signatures

A **signature** is a rule or pattern that matches a known attack or suspicious behaviour.

**Signature types:**
- **Atomic signature** – matches a single packet (e.g., port scan probe)
- **Composite (stateful) signature** – matches a sequence of packets or events over time (e.g., port scan, multi-step exploit)

**Signature actions:**

| Action | Description |
|---|---|
| **Alert** | Generate log entry and send notification |
| **Drop** | Silently discard the offending packet |
| **Reset TCP** | Send TCP RST to both parties to terminate session |
| **Block attacker** | Deny all traffic from the source IP for a defined period |
| **Log** | Capture packet for forensic purposes |
| **Shun** | ASA-specific: block host IP in the connection table |

**Signature tuning:**

| Issue | Solution |
|---|---|
| Too many false positives | Tune (adjust severity/threshold) or retire irrelevant signatures |
| Missing attacks | Enable additional signature categories; enable anomaly detection |
| High resource usage | Retire rarely triggered or irrelevant signatures |
| Alert fatigue | Prioritise critical signatures; filter alerts in SIEM |

**Why tuning is essential:**
- Out-of-box signatures are generic and generate many false positives
- Not all signatures are relevant to every environment (e.g., IIS signatures on an Apache-only network)
- Untuned IPS creates alert fatigue → analysts start ignoring alerts → attacks succeed

## 12.2 Cisco Snort IPS

**Snort** is the open-source IPS engine that powers many Cisco IPS products (including Firepower).

**Snort operating modes:**
- **Sniffer mode** – reads and displays packets from network
- **Packet logger mode** – logs packets to disk
- **NIDS/NIPS mode** – analyses traffic against rules; alerts or blocks

**Snort rule structure:**
```
[action] [protocol] [src IP] [src port] -> [dst IP] [dst port] ([options])

alert tcp any any -> 192.168.1.0/24 80 (msg:"HTTP traffic to web server"; sid:1000001; rev:1;)
```

**Rule components:**
- **Action:** `alert`, `log`, `pass`, `drop`, `reject`, `sdrop`
- **Protocol:** `tcp`, `udp`, `icmp`, `ip`
- **Direction:** `->` (one-way) or `<>` (bidirectional)
- **Options:** message, signature ID (SID), content match, threshold, etc.

## 12.3 Configure Snort IPS

Snort IPS on Cisco ISR (IOS-XE) is integrated into the router as a virtual service container:

```
! Verify Snort virtual service
show virtual-service list

! Configure Snort IPS on an interface
interface GigabitEthernet0/0/0
 utd enable

! Configure UTD (Unified Threat Defence) with Snort
utd engine standard
 threat protection
  policy security                 ! Use security policy (can also use balanced/connectivity)

! Point to Snort signature update server
utd engine standard
 signature update server cisco    ! Use Cisco cloud for signature updates
 signature update occur-at daily 2 0    ! Update at 02:00 daily

! Verify
show utd engine standard status
show utd engine standard threat-inspection summary
```

## 12.4 IPS Operation and Implementation Summary

> Signatures are the core of IPS detection. Tuning is ongoing — start with the basic category, retire irrelevant signatures, and adjust thresholds over time. Snort powers Cisco's IPS platforms. The actions available (alert, drop, block, reset) let you control the response precisely. Always monitor IPS statistics to identify trends and optimise detection.

---

# Module 13: Endpoint Security

## 13.1 Endpoint Security Overview

The traditional network perimeter has dissolved. Endpoints (laptops, phones, tablets, servers) are the ultimate targets and increasingly the attack entry points.

**Why endpoint security matters:**
- Users work from untrusted networks (home, coffee shops)
- Malware often enters via email or web, not the network perimeter
- Encrypted traffic (HTTPS) bypasses network inspection
- Insiders are already inside the perimeter

**Endpoint protection layers:**

| Layer | Tool | Function |
|---|---|---|
| **Antivirus/Anti-malware** | Windows Defender, ESET | Signature + heuristic detection |
| **Host firewall** | Windows Firewall, iptables | Filter host-level traffic |
| **HIPS** | Cisco Secure Endpoint, Symantec | Block suspicious system behaviour |
| **EDR** | CrowdStrike, Cisco AMP, Carbon Black | Deep behavioural analysis + response |
| **DLP** | Forcepoint, Symantec DLP | Prevent data exfiltration |
| **Patching** | WSUS, SCCM, Ansible | Close known vulnerabilities |
| **Encryption** | BitLocker, FileVault | Protect data at rest on device |

**EDR vs Traditional AV:**

| Feature | Traditional AV | EDR |
|---|---|---|
| Detection method | Signatures | Signatures + AI/ML + behaviour |
| Zero-day detection | Limited | Yes |
| Visibility | File scan | Full activity timeline |
| Response | Quarantine file | Isolate host, kill process, rollback |
| Threat hunting | No | Yes |

## 13.2 802.1X Authentication

**802.1X** provides **port-based Network Access Control (NAC)** — devices must authenticate before gaining network access.

**Three roles:**

| Role | Device | Example |
|---|---|---|
| **Supplicant** | Device requesting access | PC, phone, printer |
| **Authenticator** | Enforces access control at the port | Switch, wireless AP |
| **Authentication Server** | Validates credentials | RADIUS / Cisco ISE |

**802.1X process:**
1. Device connects → switch port enters **Unauthorised** state (only 802.1X traffic allowed)
2. Switch sends **EAP-Request/Identity**
3. Device responds with **EAP-Response/Identity** (username)
4. Switch forwards to RADIUS server (via RADIUS Access-Request)
5. RADIUS sends EAP challenge to device
6. Device responds with credentials
7. RADIUS sends **Access-Accept** or **Access-Reject**
8. Port moves to **Authorised** (or remains blocked)

**EAP Methods:**

| Method | Authentication | Strength |
|---|---|---|
| **EAP-MD5** | Password hash challenge-response | Weak — no mutual auth |
| **EAP-TLS** | Client + server certificates | Very Strong |
| **PEAP** | Tunnels MS-CHAPv2 inside TLS | Strong — most common |
| **EAP-TTLS** | Tunnels various methods inside TLS | Strong |
| **EAP-FAST** | Cisco proprietary; uses PAC file | Strong |

**Switch 802.1X configuration:**
```
! Enable 802.1X globally
dot1x system-auth-control

! Configure RADIUS server for 802.1X
radius server ISE
 address ipv4 10.0.0.50 auth-port 1812 acct-port 1813
 key RadiusKey@123

aaa new-model
aaa authentication dot1x default group radius
aaa authorization network default group radius

! Configure interface
interface GigabitEthernet0/1
 switchport mode access
 dot1x pae authenticator
 authentication port-control auto    ! auto = require 802.1X auth
 spanning-tree portfast               ! Required for 802.1X on access ports

! Port control options:
! auto       = require authentication
! force-authorised   = always authorise (no 802.1X)
! force-unauthorised = always block
```

## 13.3 Endpoint Security Summary

> Endpoint security extends the security perimeter to individual devices. Layers include AV, host firewall, HIPS, EDR, DLP, and patching. 802.1X enforces authentication before granting any network access, using EAP between the supplicant (device), authenticator (switch), and authentication server (RADIUS/ISE). Modern EDR solutions go far beyond traditional AV with behavioural analysis, threat hunting, and automated response.

---

# Module 14: Layer 2 Security Considerations

## 14.1 Layer 2 Security Threats

Layer 2 is often overlooked in security design, yet if the Data Link layer is compromised, all higher-layer security can be undermined.

**Why Layer 2 is vulnerable:**
- Many Layer 2 protocols (ARP, STP, DHCP, DTP) have **no built-in authentication**
- Switches trust what they receive from connected devices by default
- Layer 2 attacks can enable Man-in-the-Middle, DoS, and privilege escalation

**Key Layer 2 threats:**
- MAC address table (CAM) overflow
- VLAN hopping (switch spoofing, double tagging)
- DHCP attacks (starvation, spoofing)
- ARP spoofing / ARP poisoning
- IP/MAC address spoofing
- STP manipulation

## 14.2 MAC Table Attacks

**How MAC tables work:** A switch learns the MAC address of devices by recording the source MAC and port of each received frame. The MAC address table (CAM table) maps MAC → port.

**MAC Flooding Attack:**
- Attacker floods the switch with Ethernet frames using **thousands of random fake source MACs**
- The MAC table fills up completely (CAM table overflow)
- Switch can no longer learn new entries
- Switch **fails open** — unknown destination frames are flooded to ALL ports (hub behaviour)
- Attacker can now sniff all traffic on the segment

## 14.3 Mitigate MAC Table Attacks

**Port Security** limits the number of MAC addresses learned on a port:

```
interface GigabitEthernet0/1
 switchport mode access
 switchport port-security                        ! Enable port security
 switchport port-security maximum 2              ! Max 2 MACs allowed
 switchport port-security mac-address sticky     ! Learn and save current MACs
 switchport port-security violation restrict     ! Violation action

! Violation modes:
! protect  – silently drop frames from unknown MACs; no log; no counter
! restrict – drop frames, log, increment violation counter (recommended)
! shutdown – (default) err-disable the port immediately

! Verify
show port-security
show port-security interface GigabitEthernet0/1
show port-security address

! Recover from err-disabled port
interface GigabitEthernet0/1
 shutdown
 no shutdown
! Or configure automatic recovery:
errdisable recovery cause psecure-violation
errdisable recovery interval 300
```

## 14.4 Mitigate VLAN Attacks

### VLAN Hopping

**Method 1 – Switch Spoofing:**
- Attacker configures their NIC to negotiate a **trunk link** using DTP (Dynamic Trunking Protocol)
- Once a trunk is established, attacker can send/receive traffic on any VLAN
- **Mitigation:** Disable DTP on all access ports

**Method 2 – Double Tagging:**
- Attacker sends a frame with **two 802.1Q VLAN tags**
- Switch 1 strips the outer tag (matching native VLAN) and forwards with inner tag intact
- Switch 2 sees the inner tag and delivers to the target VLAN
- Note: Only **one-directional** — responses are not returned to the attacker

```
! Disable DTP negotiation on access ports
interface range GigabitEthernet0/1 - 24
 switchport mode access
 switchport nonegotiate                   ! Disable DTP entirely

! Set native VLAN to an unused VLAN (not VLAN 1)
interface GigabitEthernet0/24            ! Trunk port
 switchport trunk native vlan 999        ! Use unused VLAN as native

! Optionally tag native VLAN (requires both ends to support)
vlan dot1q tag native

! Put all unused ports in an unused VLAN and disable them
interface range GigabitEthernet0/5 - 20
 switchport mode access
 switchport access vlan 999
 shutdown
```

## 14.5 Mitigate DHCP Attacks

### DHCP Starvation
- Attacker sends many DHCP Discover messages with spoofed random MAC addresses
- DHCP server allocates addresses to all of them, exhausting the pool
- Legitimate hosts cannot obtain an IP address → DoS

### DHCP Spoofing (Rogue DHCP Server)
- Attacker runs a rogue DHCP server on the segment
- Faster than legitimate server → wins the race to respond
- Assigns attacker-controlled **default gateway** and **DNS server**
- All client traffic routed through attacker → Man-in-the-Middle

**Mitigation: DHCP Snooping**

```
! Enable DHCP Snooping globally and on specific VLANs
ip dhcp snooping
ip dhcp snooping vlan 10,20,30

! Mark only legitimate uplinks (DHCP server side) as trusted
! Untrusted ports (client ports) will have DHCP Discover rate limited
interface GigabitEthernet0/24           ! Uplink to DHCP server or router
 ip dhcp snooping trust

! Rate limit DHCP messages on untrusted ports (prevent starvation)
interface range GigabitEthernet0/1 - 22
 ip dhcp snooping limit rate 15         ! Max 15 DHCP packets/second

! Verify
show ip dhcp snooping
show ip dhcp snooping binding           ! DHCP binding table (MAC, IP, port, VLAN)
```

> 💡 The **DHCP Snooping binding table** is used by both DAI (ARP inspection) and IP Source Guard. DHCP snooping must be enabled before those features work.

## 14.6 Mitigate ARP Attacks

### ARP Spoofing / ARP Poisoning

**ARP is stateless and unauthenticated** — any device can send a Gratuitous ARP claiming any IP-to-MAC mapping.

**Attack:**
1. Attacker broadcasts: "192.168.1.1 (the gateway) is at MY MAC address"
2. Victims update their ARP cache
3. All traffic destined for the gateway now goes to the attacker
4. Attacker forwards it (MitM) or drops it (DoS)

**Mitigation: Dynamic ARP Inspection (DAI)**

```
! Enable DAI on VLANs
ip arp inspection vlan 10,20,30

! Mark trusted interfaces (uplinks only; everything else untrusted by default)
interface GigabitEthernet0/24
 ip arp inspection trust

! Rate limit ARP on untrusted ports (prevent ARP flood)
interface range GigabitEthernet0/1 - 22
 ip arp inspection limit rate 100        ! Max 100 ARP packets/second

! Verify
show ip arp inspection
show ip arp inspection statistics vlan 10
```

> 💡 DAI validates ARP packets against the **DHCP Snooping binding table**. If an ARP reply claims a MAC-IP mapping not in the binding table, it is dropped. DHCP Snooping must be enabled first.

## 14.7 Mitigate Address Spoofing Attacks

**IP Source Guard (IPSG)** prevents IP spoofing on switch ports by only allowing traffic that matches the DHCP snooping binding table.

```
! Enable IP Source Guard on untrusted access ports
interface GigabitEthernet0/1
 ip verify source                        ! Filter based on source IP
 ! Or:
 ip verify source port-security          ! Filter based on source IP AND MAC

show ip verify source
show ip source binding
```

**Effect:** Only packets with the IP address assigned via DHCP (recorded in snooping table) are forwarded. All other source IPs are dropped.

## 14.8 Spanning Tree Protocol

**STP (Spanning Tree Protocol)** prevents Layer 2 loops by electing a **root bridge** and blocking redundant paths.

**STP process:**
1. **Root Bridge election** – switch with lowest Bridge ID (Priority + MAC) wins
2. **Root Port** – non-root switch port closest to root bridge
3. **Designated Port** – port that forwards traffic on each segment
4. **Blocked Port** – port put in blocking state to prevent loops

**STP port states (classic STP):**
- **Blocking** → **Listening** → **Learning** → **Forwarding** → **Disabled**
- PortFast skips Listening and Learning for access ports

**RSTP (Rapid STP / 802.1w)** – faster convergence; standard in modern networks.

## 14.9 Mitigate STP Attacks

**STP Attack:**
- Attacker sends **superior BPDUs** (lower Bridge ID) to become the root bridge
- Forces traffic through attacker → MitM, network disruption
- Network may recalculate STP topology causing temporary outages

**Mitigations:**

```
! PortFast — skip STP listening/learning on access ports (faster convergence)
interface GigabitEthernet0/1
 spanning-tree portfast

! BPDU Guard — if a BPDU arrives on a PortFast port, err-disable it immediately
interface GigabitEthernet0/1
 spanning-tree bpduguard enable

! Enable globally (applies to all PortFast-enabled ports)
spanning-tree portfast bpduguard default

! BPDU Filter — suppress sending and receiving BPDUs on a port
! (Use with caution — can cause loops if misconfigured)
interface GigabitEthernet0/1
 spanning-tree bpdufilter enable

! Root Guard — prevent this port from ever becoming a root port
! Apply to ports where you will NEVER receive superior BPDUs
interface GigabitEthernet0/24
 spanning-tree guard root

! Loop Guard — detect and prevent STP loops caused by unidirectional link failures
interface GigabitEthernet0/24
 spanning-tree guard loop

! Verify
show spanning-tree
show spanning-tree detail
show spanning-tree inconsistentports
```

**STP security feature summary:**

| Feature | Applied On | Purpose |
|---|---|---|
| **PortFast** | Access ports | Faster convergence; skip listen/learn |
| **BPDU Guard** | PortFast ports | Shut port if BPDU received |
| **BPDU Filter** | Specific ports | Stop sending/receiving BPDUs |
| **Root Guard** | Uplink/edge ports | Prevent port becoming root |
| **Loop Guard** | Non-designated ports | Detect unidirectional links |

## 14.10 Layer 2 Security Considerations Summary

> Layer 2 is the foundation all other layers rely on. Attacks on ARP, DHCP, STP, VLANs, and the MAC table can undermine all higher-layer security. DHCP Snooping, DAI, IP Source Guard, Port Security, BPDU Guard, and Root Guard together form a comprehensive Layer 2 defence strategy. These features are largely interdependent — DHCP Snooping provides the binding table that DAI and IPSG rely upon.

# Module 15: Cryptographic Services

## 15.1 Secure Communications

For communication to be considered secure, it must satisfy:

| Property | Question | Mechanism |
|---|---|---|
| **Confidentiality** | Can anyone else read this? | Encryption |
| **Integrity** | Has this been altered? | Hashing, HMAC |
| **Authentication** | Who really sent this? | Digital signatures, certificates |
| **Non-repudiation** | Can the sender deny sending it? | Digital signatures |

## 15.2 Cryptography

**Cryptography** is the science of using mathematics to transform data so that only authorised parties can read it.

**Basic terms:**

| Term | Definition |
|---|---|
| **Plaintext** | Original, readable data |
| **Ciphertext** | Encrypted, unreadable data |
| **Encryption** | Transforming plaintext → ciphertext |
| **Decryption** | Transforming ciphertext → plaintext |
| **Key** | Secret value that controls the cipher |
| **Algorithm / Cipher** | Mathematical function performing encryption |
| **Keyspace** | All possible key values — larger = stronger brute-force resistance |

### Symmetric Encryption
- **Same key** used for both encryption and decryption
- **Fast** — suitable for encrypting large amounts of data
- **Key distribution problem** — how do you securely share the key before you have a secure channel?

| Algorithm | Key Size | Notes |
|---|---|---|
| **DES** | 56-bit | Deprecated — broken 1999 |
| **3DES** | 112/168-bit | Three DES rounds — legacy, being phased out |
| **AES-128** | 128-bit | Current standard, secure |
| **AES-256** | 256-bit | Recommended for sensitive data |
| **RC4** | Variable | Stream cipher — deprecated (WEP, old TLS) |
| **ChaCha20** | 256-bit | Modern stream cipher — used in TLS 1.3 |

**Block cipher modes:**

| Mode | Notes |
|---|---|
| **ECB** | Same plaintext block → same ciphertext. Reveals patterns. **Insecure.** |
| **CBC** | XORs each block with previous ciphertext. Needs IV. Was standard. |
| **CTR** | Turns block cipher into stream cipher. Parallelisable. |
| **GCM** | CTR + authentication tag. Provides encryption AND integrity. **Recommended.** |

### Asymmetric Encryption
- **Key pair:** public key (shared freely) + private key (kept secret)
- What is encrypted with the **public key** → only decryptable with the **private key**
- What is signed with the **private key** → verifiable with the **public key**
- **Slow** — only used for key exchange and signatures, not bulk data

| Algorithm | Use Case | Notes |
|---|---|---|
| **RSA** | Encryption, signatures, key exchange | Most common; 2048+ bits required |
| **DH** | Key exchange only | Does not encrypt or sign |
| **ECDH** | Elliptic curve key exchange | Smaller keys, same security |
| **ECDSA** | Elliptic curve signatures | Smaller, faster than RSA |
| **DSA** | Signatures only | Older US government standard |

### Hybrid Encryption
Real-world systems combine both:
1. **Asymmetric** to securely exchange a **symmetric session key**
2. **Symmetric** to encrypt the bulk data (fast)

This is how **TLS, SSH, IPsec, and PGP** work.

## 15.3 Cryptanalysis

**Cryptanalysis** is the science of breaking cryptographic systems.

**Attack types:**

| Attack | Description |
|---|---|
| **Brute force** | Try every possible key; success guaranteed given enough time |
| **Dictionary attack** | Try common words and phrases as keys |
| **Birthday attack** | Exploit probability to find hash collisions faster |
| **Man-in-the-middle** | Intercept key exchange to insert attacker's keys |
| **Ciphertext-only** | Attacker has ciphertext only — tries to derive key |
| **Known-plaintext** | Attacker has some plaintext-ciphertext pairs |
| **Chosen-plaintext** | Attacker can choose plaintexts and observe ciphertexts |
| **Side-channel** | Exploit physical implementation (power usage, timing, EM emissions) |

**Cryptographic strength factors:**
- **Key length** — longer key = exponentially more brute-force effort
- **Algorithm strength** — no known mathematical shortcuts
- **Key secrecy** — even a perfect algorithm fails if the key is compromised
- **Kerckhoffs's principle** — a cryptosystem should be secure even if everything except the key is public knowledge

## 15.4 Cryptology

**Cryptology** is the combined field of **cryptography** (creating secure systems) and **cryptanalysis** (breaking them).

**Historical ciphers (for context):**
- **Caesar cipher** – shift letters by a fixed number (rot13 is Caesar shift-13)
- **Enigma** – rotor-based machine cipher (WW2); broken by Turing at Bletchley Park
- **Vigenère** – polyalphabetic substitution; harder to break than Caesar
- All historical ciphers are computationally trivial to break today

**One-Time Pad:**
- Theoretically **perfectly secure** — the only provably unbreakable cipher
- Key must be: random, same length as message, never reused
- Completely impractical at scale

## 15.5 Cryptographic Services Summary

> Cryptography provides confidentiality, integrity, authentication, and non-repudiation. Symmetric encryption is fast but has the key distribution problem. Asymmetric solves key distribution but is slow. Hybrid systems use asymmetric for key exchange and symmetric for data. Always use modern algorithms: AES-256 for symmetric, RSA-2048+/ECDSA for asymmetric.

---

# Module 16: Basic Integrity and Authenticity

## 16.1 Integrity and Authenticity

### Hashing

A **hash function** produces a fixed-size **digest** (fingerprint) from any size input.

**Properties of a good hash function:**
- **One-way** — computationally infeasible to reverse (find input from output)
- **Deterministic** — same input always produces same hash
- **Avalanche effect** — one bit change in input → completely different hash
- **Collision-resistant** — computationally infeasible to find two different inputs with same hash
- **Fixed output size** — regardless of input length

**Uses:** Integrity verification, password storage, digital signatures, file fingerprinting

| Algorithm | Output | Status |
|---|---|---|
| **MD5** | 128-bit (32 hex) | Deprecated — collisions known; used for checksums only |
| **SHA-1** | 160-bit | Deprecated — collision demonstrated 2017 |
| **SHA-256** | 256-bit | ✅ Current standard |
| **SHA-384** | 384-bit | ✅ Strong |
| **SHA-512** | 512-bit | ✅ Very strong |
| **SHA-3** | Variable | ✅ Modern; different internal design from SHA-2 |
| **bcrypt** | 60 chars | ✅ Best for passwords — intentionally slow |
| **Argon2** | Variable | ✅ Best for passwords — memory-hard |

### Salting
A **salt** is a random value appended to a password before hashing.

**Why salting matters:**
- Without salt: identical passwords produce identical hashes → **rainbow table** attacks work
- With salt: `hash("password" + "xK9$mN2p")` → unique hash per user
- Makes precomputed rainbow tables useless

```
! Without salt: two users with "password" have same hash
hash("password") = 5f4dcc3b5aa...   (same for all!)

! With salt: each user has a unique hash
hash("password" + "xK9$mN2p") = a7c8f3e...   (unique!)
hash("password" + "Qz7$wP4k") = 9f2b1a4...   (different!)
```

### HMAC (Hash-based Message Authentication Code)

`HMAC = Hash(message + secret_key)`

**HMAC provides:**
- ✅ **Integrity** — detects modification of the message
- ✅ **Authentication** — only someone with the secret key can produce a valid HMAC
- ❌ Does NOT provide non-repudiation (both parties share the same key)
- ❌ Does NOT provide confidentiality (HMAC does not encrypt)

**Common uses:** IPsec authentication, TLS record authentication, JWT tokens, SSH

```
HMAC-SHA256(message, secret_key) → 256-bit authentication tag
```

## 16.2 Key Management

**Key management** is one of the hardest problems in cryptography — the security of encrypted data depends entirely on keeping keys secret.

**Key management lifecycle:**

| Phase | Activities |
|---|---|
| **Generation** | Create keys using a cryptographically secure random number generator |
| **Storage** | Protect keys using HSMs, key vaults, or encrypted key stores |
| **Distribution** | Share keys securely (asymmetric key exchange, key wrapping) |
| **Use** | Apply to encryption/decryption; log usage |
| **Rotation** | Replace keys periodically or after compromise |
| **Revocation** | Invalidate compromised keys immediately |
| **Destruction** | Securely delete keys when no longer needed |

**Key storage options:**
- **HSM (Hardware Security Module)** – dedicated tamper-resistant hardware for key storage and crypto operations (highest security)
- **TPM (Trusted Platform Module)** – chip on motherboard; stores keys for BitLocker, Secure Boot
- **Software key store** – encrypted file; lower security than hardware

**Key length recommendations (2024+):**

| Use | Minimum | Recommended |
|---|---|---|
| AES (symmetric) | 128-bit | 256-bit |
| RSA (asymmetric) | 2048-bit | 3072+ bit |
| ECDSA/ECDH | 256-bit | 384-bit |
| DH key exchange | 2048-bit | Group 14 (2048) or Group 19 (ECDH 256) |
| Hash | SHA-256 | SHA-256 or SHA-3 |

## 16.3 Confidentiality

**Confidentiality** ensures that only authorised parties can read information.

**Achieved through encryption:**
- **Data in transit** — TLS (HTTPS, SMTPS), IPsec, SSH, SFTP
- **Data at rest** — AES-256 (BitLocker, FileVault, VeraCrypt, self-encrypting drives)
- **Data in use** — Confidential computing (Intel SGX, AMD SEV) — emerging technology

**Common encryption deployments:**

| Use Case | Protocol/Tool |
|---|---|
| Web browsing | TLS 1.2/1.3 (HTTPS) |
| Remote administration | SSH v2 |
| Site-to-site connectivity | IPsec |
| Remote user VPN | SSL/TLS (AnyConnect) |
| Email | S/MIME, PGP |
| File transfer | SFTP, FTPS |
| Disk encryption | BitLocker (Windows), FileVault (macOS) |
| Database | TDE (Transparent Data Encryption) |

## 16.4 Basic Integrity and Authenticity Summary

> Hashing provides one-way integrity verification. Salting prevents rainbow table attacks on password hashes. HMAC adds a secret key to a hash to provide both integrity and authentication. Key management is as critical as the algorithm itself — a strong cipher with a poorly managed key is worthless. Data must be protected whether in transit, at rest, or in use.

---

# Module 17: Public Key Cryptography

## 17.1 Public Key Cryptography With Digital Signatures

### How Digital Signatures Work

Digital signatures use **asymmetric cryptography** to provide **integrity, authentication, and non-repudiation**.

**Signing process:**
1. Sender creates a message
2. Sender computes hash of the message (e.g., SHA-256) → **message digest**
3. Sender encrypts the digest with their **private key** → **digital signature**
4. Message + signature sent to recipient

**Verification process:**
1. Recipient receives message + signature
2. Recipient decrypts signature using sender's **public key** → recovers digest
3. Recipient independently hashes the received message
4. **Compare:** if the two digests match → signature is valid

**Digital signatures provide:**
- ✅ **Integrity** — any modification to the message invalidates the signature
- ✅ **Authentication** — only the holder of the private key could have signed it
- ✅ **Non-repudiation** — sender cannot deny signing (private key is unique to them)
- ❌ **Does NOT provide confidentiality** — message is not encrypted

### Code Signing
- Software vendors digitally sign executable files and updates
- OS verifies the signature before installation/execution
- Protects against: tampered downloads, supply chain attacks, malware masquerading as legitimate software

## 17.2 Authorities and the PKI Trust System

### PKI (Public Key Infrastructure)
PKI is the complete framework for managing public keys and digital certificates.

**PKI components:**

| Component | Role |
|---|---|
| **CA (Certificate Authority)** | Issues and digitally signs certificates; the trust anchor |
| **RA (Registration Authority)** | Validates requester's identity before CA issues certificate |
| **Certificate** | Binds a public key to an entity's proven identity (X.509 format) |
| **CRL (Certificate Revocation List)** | Published list of certificates that have been revoked |
| **OCSP (Online Certificate Status Protocol)** | Real-time alternative to CRL for revocation checking |
| **Certificate Repository** | Directory (often LDAP) where certificates and CRLs are published |

### X.509 Certificate Contents
- **Subject** — the entity the cert belongs to (CN, O, OU, C)
- **Subject's Public Key** — the key being certified
- **Issuer** — the CA that signed this certificate
- **Validity period** — Not Before / Not After dates
- **Serial number** — unique identifier assigned by the CA
- **Signature algorithm** — e.g., SHA256WithRSAEncryption
- **CA's Digital Signature** — the CA's stamp of approval

### Certificate Trust Chain (Chain of Trust)

```
Root CA (self-signed — stored in OS/browser trust store; kept offline)
    └── Intermediate CA (signed by Root; does day-to-day signing)
           └── End-entity Certificate (signed by Intermediate)
                  e.g., www.company.com, VPN gateway cert
```

**Why Intermediate CAs?**
- Root CA is kept **offline** in a secure facility (protecting the ultimate trust anchor)
- Intermediate CA handles daily certificate issuance
- If Intermediate CA is compromised, it can be revoked without replacing the Root CA

### Types of Certificates

| Type | Issued to | Trust |
|---|---|---|
| **Self-signed** | Entity signs its own cert | No external trust; browser warns |
| **Internally-signed** | Internal CA signs | Trusted by org's devices only |
| **Publicly-signed** | Public CA (DigiCert, Let's Encrypt) | Trusted by everyone |

### PKI Certificate Enrolment

1. Entity generates a **key pair** (public + private key)
2. Entity creates a **CSR (Certificate Signing Request)** containing: public key + identity info
3. CSR submitted to RA/CA (sometimes via SCEP, EST, or web interface)
4. CA validates identity (out-of-band verification)
5. CA signs the certificate and returns it to the requester
6. Entity installs the certificate

## 17.3 Applications and Impacts of Cryptography

### Where Cryptography is Applied

| Application | Protocol | What it Protects |
|---|---|---|
| Web browsing | TLS (HTTPS) | Confidentiality + integrity of web traffic |
| Email | S/MIME, PGP | Email confidentiality + authenticity |
| VPN | IPsec, SSL/TLS | Tunnel confidentiality + integrity |
| Remote access | SSH | Admin session confidentiality |
| File transfer | SFTP, FTPS | File transfer confidentiality |
| Code distribution | Code signing | Software integrity + authenticity |
| 802.1X | EAP-TLS | Network access authentication |
| Wireless | WPA3, 802.11i | Wi-Fi confidentiality |

### Performance Impact of Cryptography
- Symmetric encryption (AES) – minimal overhead; hardware acceleration available
- Asymmetric operations (RSA) – CPU intensive; only used for session setup
- TLS handshake – several round trips + asymmetric operations → adds ~100ms latency
- AES-NI (CPU hardware acceleration) greatly reduces symmetric encryption overhead
- **Session resumption** in TLS avoids full handshake on reconnection

### Cryptographic Agility
The ability of a system to switch cryptographic algorithms without major redesign. Important because:
- Algorithms eventually become deprecated (MD5, SHA-1, DES, 3DES)
- Quantum computing threatens RSA and DH (not AES or SHA-2)
- Post-quantum cryptography (CRYSTALS-Kyber, CRYSTALS-Dilithium) is being standardised now

## 17.4 Public Key Cryptography Summary

> Digital signatures use asymmetric crypto to provide integrity, authentication, and non-repudiation. PKI is the trust framework that binds public keys to identities via certificates signed by CAs. The chain of trust runs from Root CA → Intermediate CA → end-entity certificate. Cryptography is embedded in virtually every secure protocol — understanding the underlying concepts allows you to evaluate, configure, and troubleshoot these systems.

---

# Module 18: VPNs

## 18.1 VPN Overview

A **VPN (Virtual Private Network)** creates an encrypted, authenticated tunnel over an untrusted network (the Internet).

**Security services:**
- **Confidentiality** — encryption prevents eavesdropping
- **Integrity** — HMAC detects any tampering
- **Authentication** — peers verify each other's identity
- **Anti-replay** — sequence numbers prevent replaying old packets

**Cost advantage:** VPNs replace expensive MPLS/leased lines by using commodity Internet connectivity while maintaining privacy.

## 18.2 VPN Topologies

### Site-to-Site VPN
- Connects entire networks (branch office ↔ headquarters)
- Configured on edge routers or firewalls
- **Always-on** — transparent to end users; no client software needed
- Typically uses IPsec

### Remote Access VPN
- Individual user connects from any location to the corporate network
- User must **initiate** with VPN client software
- Two main implementations:

**IPsec Remote Access:**
- Traditional; uses UDP 500/4500 (IKE + NAT-T)
- Requires dedicated VPN client

**SSL VPN (Cisco AnyConnect):**
- Uses TLS (TCP 443) or DTLS (UDP 443 — better performance for real-time traffic)
- Works through any firewall/NAT — port 443 is almost never blocked
- Two modes:
  - **Clientless** — browser-only; web proxied access to internal web apps
  - **Full tunnel** — AnyConnect client; full Layer 3 network access

### MPLS VPN (Service Provider)
- Provider-managed Layer 3 VPN
- Traffic separated using MPLS labels (not encryption)
- Not encrypted by default — relies on provider infrastructure trust
- Provides QoS guarantees; predictable performance

### GRE over IPsec
- **GRE (Generic Routing Encapsulation)** — can carry multicast and routing protocol traffic
- **IPsec alone** cannot encrypt multicast/broadcast
- **GRE over IPsec** = GRE for routing protocols + IPsec for encryption

## 18.3 IPsec Overview

**IPsec** is a suite of open standards providing **Layer 3 security** for IP packets.

**Four key services:**

| Service | Options (Weak → Strong) |
|---|---|
| **Confidentiality** | DES → 3DES → AES-128 → AES-256 |
| **Integrity** | MD5 → SHA-1 → SHA-256 → SHA-384 |
| **Authentication** | Pre-Shared Key (PSK) → RSA signatures → certificates |
| **Key Exchange** | DH Group 1/2/5 (deprecated) → Group 14 → Group 19/20 |

## 18.4 IPsec Protocols

### AH (Authentication Header) — Protocol 51
- Provides: Integrity + Authentication + Anti-replay
- Does **NOT** encrypt data — no confidentiality
- Authenticates the **entire IP packet** including the outer IP header
- **Incompatible with NAT** — NAT modifies the IP header, breaking AH authentication
- Rarely used in practice

### ESP (Encapsulating Security Payload) — Protocol 50
- Provides: **Confidentiality + Integrity + Authentication + Anti-replay**
- Encrypts the payload
- Only authenticates from the ESP header inward (not outer IP header)
- **NAT-compatible** with NAT-T (NAT Traversal — wraps ESP in UDP port 4500)
- **Almost always preferred** over AH

### IPsec Modes

**Transport Mode:**
- Protects the **payload** — original IP header unchanged
- Used for **host-to-host** communication (e.g., remote management encryption)
- Smaller overhead

**Tunnel Mode:**
- Encapsulates the **entire original IP packet** inside a new IP packet
- New outer IP header added with the gateway addresses
- Used for **site-to-site VPNs** (gateway-to-gateway)
- Standard mode for site-to-site deployments

```
Transport: [IP Hdr][ESP Hdr][Original Payload][ESP Trailer][ESP Auth]
Tunnel:    [New IP Hdr][ESP Hdr][Original IP Hdr][Payload][ESP Trailer][ESP Auth]
```

## 18.5 Internet Key Exchange (IKE)

**IKE** is the protocol that negotiates and manages the IPsec Security Associations (SAs).

### IKEv1 — Two Phases

**Phase 1 (ISAKMP SA) — Establish Secure Channel:**

Negotiates: encryption algorithm, hash algorithm, authentication method, DH group, SA lifetime.

Two modes:
- **Main Mode** (6 messages) — full negotiation; identity protected; preferred
- **Aggressive Mode** (3 messages) — faster; identity NOT protected; used for legacy remote access

**Phase 2 (IPsec SA) — Negotiate IPsec Parameters:**
- Runs over the Phase 1 secure channel
- Mode: **Quick Mode** (3 messages)
- Creates **two unidirectional SAs** (one for each traffic direction)
- Optional: **PFS (Perfect Forward Secrecy)** — performs a new DH exchange for each Phase 2 SA

**Why PFS?**
If the long-term key is ever compromised, without PFS an attacker could decrypt all past sessions. With PFS, each session has its own independently generated key.

### IKEv2 (Recommended over IKEv1)
- More efficient: 4 messages instead of 9 (IKEv1 main mode)
- Built-in **NAT traversal**
- Better DoS attack resistance (cookies)
- Supports **EAP** for remote access authentication
- Supports **MOBIKE** — handles IP address changes (roaming mobile users)
- Simpler configuration

## 18.6 VPNs Summary

> VPNs extend private networks over public infrastructure using encryption and authentication. Site-to-site VPNs connect networks; remote access VPNs connect individual users. IPsec uses AH or ESP (ESP strongly preferred) in transport or tunnel mode. IKE negotiates the Security Associations in two phases. IKEv2 is more efficient and secure than IKEv1.

---

# Module 19: Implement Site-to-Site IPsec VPNs

## 19.1 Configure a Site-to-Site IPsec VPN

A site-to-site IPsec VPN has five configuration elements:

1. **ISAKMP policy** (Phase 1 parameters)
2. **ISAKMP key** (Pre-shared key for peer authentication)
3. **IPsec transform set** (Phase 2 encryption/integrity algorithms)
4. **Crypto ACL** (Defines which traffic should be encrypted)
5. **Crypto map** (Ties all the pieces together and applies to interface)

## 19.2 ISAKMP Policy

The ISAKMP policy defines the **Phase 1 (IKE SA)** parameters:

```
! ISAKMP policy — lower number = higher priority (tried first)
crypto isakmp policy 10
 encryption aes 256          ! Encryption algorithm
 hash sha256                 ! Integrity/hash algorithm
 authentication pre-share    ! Authentication method (PSK)
 group 14                    ! DH group for key exchange (2048-bit)
 lifetime 86400              ! SA lifetime in seconds (24 hours)

! Pre-shared key for the remote peer
crypto isakmp key VPNSecret@Key123 address 203.0.113.2
```

> 💡 Both VPN peers must have **identical** ISAKMP policy parameters. If they don't match, Phase 1 will fail. Debug with `debug crypto isakmp`.

## 19.3 IPsec Policy

The transform set defines **Phase 2 (IPsec SA)** parameters — the actual encryption used on the data:

```
! Define the transform set (encryption + integrity for ESP)
crypto ipsec transform-set TS-AES esp-aes 256 esp-sha256-hmac
 mode tunnel                  ! Tunnel mode for site-to-site (default)

! Optional: set SA lifetime for Phase 2
crypto ipsec security-association lifetime seconds 3600
```

**Common transform set combinations:**

| Transform | Notes |
|---|---|
| `esp-aes esp-sha-hmac` | AES-128 + SHA-1 — acceptable |
| `esp-aes 256 esp-sha256-hmac` | AES-256 + SHA-256 — **recommended** |
| `esp-aes 256 esp-sha512-hmac` | AES-256 + SHA-512 — very strong |

## 19.4 Crypto Map

The crypto map binds together: the remote peer, transform set, ACL (interesting traffic), and optional PFS:

```
! ACL defining "interesting traffic" — traffic that should be encrypted
ip access-list extended VPN-INTERESTING-TRAFFIC
 permit ip 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255
 ! Source = local network; Destination = remote network

! Crypto map — ties everything together
crypto map VPN-MAP 10 ipsec-isakmp
 set peer 203.0.113.2                   ! Remote peer IP address
 set transform-set TS-AES               ! Phase 2 transform set
 set pfs group14                        ! Enable PFS with DH group 14
 match address VPN-INTERESTING-TRAFFIC  ! Which traffic to encrypt

! Apply crypto map to the WAN interface
interface GigabitEthernet0/1
 crypto map VPN-MAP
```

## 19.5 IPsec VPN

### NAT Exemption
If NAT is configured on the router, VPN traffic must be exempted — NAT should NOT translate VPN source IPs before encryption.

```
! Route map or extended ACL approach:
ip access-list extended NAT-EXEMPT
 deny ip 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255   ! VPN traffic — don't NAT
 permit ip 192.168.1.0 0.0.0.255 any                 ! Everything else — NAT normally

ip nat inside source list NAT-EXEMPT interface GigabitEthernet0/1 overload
```

### Complete Site-to-Site Configuration (Router R1)

```
! ── Prerequisites ──────────────────────────
hostname R1
ip domain-name company.com

! ── Phase 1 (ISAKMP) ────────────────────────
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400

crypto isakmp key VPNSecret@Key123 address 203.0.113.2

! ── Phase 2 (IPsec Transform Set) ───────────
crypto ipsec transform-set TS-AES esp-aes 256 esp-sha256-hmac
 mode tunnel

! ── Interesting Traffic ACL ──────────────────
ip access-list extended VPN-INTERESTING-TRAFFIC
 permit ip 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255

! ── NAT Exemption ────────────────────────────
ip access-list extended NAT-EXEMPT
 deny ip 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255
 permit ip 192.168.1.0 0.0.0.255 any
ip nat inside source list NAT-EXEMPT interface GigabitEthernet0/1 overload

! ── Crypto Map ───────────────────────────────
crypto map VPN-MAP 10 ipsec-isakmp
 set peer 203.0.113.2
 set transform-set TS-AES
 set pfs group14
 match address VPN-INTERESTING-TRAFFIC

! ── Apply to Interface ────────────────────────
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside

interface GigabitEthernet0/1
 ip address 203.0.113.1 255.255.255.0
 ip nat outside
 crypto map VPN-MAP
```

## 19.6 Verify and Troubleshoot

```
! Verify Phase 1 SA (should show QM_IDLE when working)
show crypto isakmp sa

! Verify Phase 2 SA (check pkts encaps/decaps counts are incrementing)
show crypto ipsec sa

! View current crypto map configuration
show crypto map

! View transform sets
show crypto ipsec transform-set

! Troubleshooting debug commands (use with caution in production)
debug crypto isakmp           ! Phase 1 negotiation
debug crypto ipsec            ! Phase 2 negotiation
no debug all                  ! Stop all debugging

! Common issues:
! Phase 1 fails → mismatched ISAKMP policy (encryption, hash, auth, DH group)
! Phase 2 fails → mismatched transform sets
! Tunnel up but no traffic → incorrect/mismatched crypto ACL, or NAT not exempted
! Tunnel not forming → routing issue, ACL blocking UDP 500/4500
```

## 19.6 Implement Site-to-Site IPsec VPNs Summary

> A site-to-site IPsec VPN requires consistent configuration on both peers. Phase 1 (ISAKMP policy) establishes a secure channel; Phase 2 (transform set + crypto map) encrypts the actual data. The crypto ACL defines which traffic gets encrypted — it must be a mirror image on both sides. NAT exemption prevents NAT from mangling VPN traffic before it's encrypted.

# Module 20: Introduction to the ASA

## 20.1 ASA Solutions

**Cisco ASA (Adaptive Security Appliance)** is Cisco's purpose-built security platform combining:
- **Stateful inspection firewall** (primary function)
- **VPN concentrator** (IPsec site-to-site and remote access, SSL/AnyConnect)
- **NAT/PAT**
- **Optional IPS** (via integrated Firepower module)
- **Optional antimalware** (via CSC module)

**ASA product lines:**
- **ASA 5505/5506-X/5508-X/5516-X** – SOHO/SMB
- **ASA 5525-X/5545-X/5555-X** – enterprise
- **ASA 5585-X** – high-end enterprise/data centre
- **Firepower 1000/2100/4100/9300** – modern NGFW (replaced classic ASA)

**Management options:**
- **CLI** (console/SSH) — most granular control
- **ASDM (Adaptive Security Device Manager)** — Java-based web GUI
- **Cisco Defense Orchestrator (CDO)** — cloud-based management for multiple ASAs
- **Cisco Firepower Management Centre (FMC)** — for ASAs with Firepower module

## 20.2 The ASA 5506-X with FirePOWER Services

The **ASA 5506-X** is a common SMB/SOHO device used in labs and real environments.

**Hardware:**
- 8x GigabitEthernet ports
- 1x Management port
- USB port for recovery
- Optional Wi-Fi (5506W-X)
- Integrated FirePOWER (IPS, AVC, URL filtering, AMP) — managed by FMC or locally via ASDM

**Security levels — the defining ASA concept:**

Every ASA interface has a **security level** from 0 (untrusted) to 100 (most trusted).

**Default traffic rules based on security level:**

| Traffic Direction | Behaviour |
|---|---|
| **High → Low** (e.g., Inside 100 → Outside 0) | Permitted by default (stateful return traffic auto-allowed) |
| **Low → High** (e.g., Outside 0 → Inside 100) | **Denied by default** — requires explicit ACL to permit |
| **Equal level** (e.g., DMZ 50 → DMZ 50) | **Denied by default** — requires `same-security-traffic permit inter-interface` |

```
! Basic interface configuration
interface GigabitEthernet1/1
 nameif outside                    ! Named interface — required for ASA operation
 security-level 0
 ip address 203.0.113.1 255.255.255.0
 no shutdown

interface GigabitEthernet1/2
 nameif inside
 security-level 100
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface GigabitEthernet1/3
 nameif dmz
 security-level 50
 ip address 172.16.1.1 255.255.255.0
 no shutdown
```

**Why `nameif` matters:**
- All subsequent ASA configuration references interfaces by their **name** (inside, outside, dmz) not the interface ID
- `nameif` must be configured before most other ASA features work

## 20.3 Introduction to the ASA Summary

> The ASA's security level model is fundamentally different from router ACLs. Higher security levels automatically trust lower ones — traffic from trusted (high) to untrusted (low) is permitted by default. Traffic from untrusted to trusted requires explicit permission. This default-deny model for inbound traffic is what makes the ASA secure out of the box.

---

# Module 21: ASA Firewall Configuration

## 21.1 Basic ASA Firewall Configuration

### Routing on ASA
```
! Default route (toward Internet)
route outside 0.0.0.0 0.0.0.0 203.0.113.254

! Static route for internal network
route inside 10.0.0.0 255.255.0.0 192.168.1.254
```

### Default vs Explicit Traffic Rules
- Inside (100) → Outside (0): **Permitted automatically**
- Outside (0) → Inside (100): **Denied automatically** — need ACL to permit
- Outside (0) → DMZ (50): **Denied automatically** — need ACL to permit specific services
- Inside (100) → DMZ (50): **Permitted automatically** (higher to lower)

## 21.2 Configure Management Settings and Services

```
! Set hostname and domain
hostname ASA-FW
domain-name company.com

! Enable SSH access from inside network
crypto key generate rsa modulus 2048
ssh 192.168.1.0 255.255.255.0 inside     ! Permit SSH from inside subnet
ssh version 2
ssh timeout 5

! Local admin user
username admin password Admin@P@ss privilege 15

! Console timeout
console timeout 5

! Disable Telnet (it should be disabled by default)
! telnet timeout 5   -- only if needed (avoid using Telnet)

! Configure management interface (separate physical port)
interface Management1/1
 nameif management
 security-level 100
 ip address 10.0.0.1 255.255.255.0
 management-only               ! Only management traffic; no transit traffic

! DNS
dns server-group DefaultDNS
 name-server 8.8.8.8
 name-server 1.1.1.1

! NTP
ntp server 10.0.0.50 source inside
```

## 21.3 Object Groups

**Objects** and **object groups** simplify and organise ASA configuration:

**Network Objects/Groups:**
```
! Single host object
object network WEB-SERVER
 host 172.16.1.10
 description Primary web server

! Network object  
object network INTERNAL-NETWORK
 subnet 192.168.1.0 255.255.255.0

! Object group (multiple hosts/networks)
object-group network ALL-WEB-SERVERS
 network-object host 172.16.1.10
 network-object host 172.16.1.11
 network-object host 172.16.1.12
```

**Service Objects/Groups:**
```
! Service object group
object-group service WEB-SERVICES tcp
 port-object eq 80
 port-object eq 443
 port-object eq 8080

object-group service MANAGEMENT-SERVICES tcp
 port-object eq 22      ! SSH
 port-object eq 3389    ! RDP

! Protocol object group
object-group protocol ROUTING-PROTOCOLS
 protocol-object ospf
 protocol-object eigrp
```

**Using objects in ACLs:**
```
access-list OUTSIDE-IN extended permit tcp any object-group ALL-WEB-SERVERS object-group WEB-SERVICES
access-list OUTSIDE-IN extended deny ip any any log
access-group OUTSIDE-IN in interface outside
```

## 21.4 ASA ACLs

**ASA ACL differences from IOS:**
- Named ACLs only (no numbered ACLs)
- Use **subnet masks** (not wildcard masks) — opposite of IOS!
- Applied with `access-group` command to an interface in a direction
- ASA uses `eq`, `gt`, `lt`, `neq`, `range` for port matching

```
! Permit HTTP/HTTPS from internet to DMZ web server
access-list OUTSIDE-IN extended permit tcp any host 172.16.1.10 eq 80
access-list OUTSIDE-IN extended permit tcp any host 172.16.1.10 eq 443
access-list OUTSIDE-IN extended deny ip any any log

! Permit SMTP from internet to mail server in DMZ
access-list OUTSIDE-IN extended permit tcp any host 172.16.1.20 eq 25

! Apply to outside interface (inbound direction)
access-group OUTSIDE-IN in interface outside

! Permit from DMZ to inside for specific services only
access-list DMZ-TO-INSIDE extended permit tcp 172.16.1.0 255.255.255.0 192.168.1.0 255.255.255.0 eq 1433   ! SQL Server
access-list DMZ-TO-INSIDE extended deny ip any any log
access-group DMZ-TO-INSIDE in interface dmz
```

> ⚠️ **ASA subnet masks are NOT wildcard masks.** Use `255.255.255.0` (not `0.0.0.255`) in ASA ACLs.

## 21.5 NAT Services on an ASA

ASA NAT has two types: **Object NAT** (simpler) and **Twice NAT** (more complex/flexible).

### Object NAT (Auto NAT)

```
! Dynamic PAT — translate inside hosts to outside interface address (many-to-one)
object network INSIDE-HOSTS
 subnet 192.168.1.0 255.255.255.0
 nat (inside,outside) dynamic interface

! Static NAT — fixed translation for DMZ web server
object network DMZ-WEB-SERVER
 host 172.16.1.10
 nat (dmz,outside) static 203.0.113.10

! Static PAT — map external port 443 to internal server port 443
object network DMZ-HTTPS
 host 172.16.1.10
 nat (dmz,outside) static 203.0.113.10 service tcp 443 443

! Verify NAT
show nat
show nat detail
show xlate
```

### Twice NAT (Manual NAT)
Used for complex scenarios (translating both source and destination, policy NAT):

```
! Dynamic PAT with twice NAT
nat (inside,outside) source dynamic INSIDE-HOSTS interface

! NAT exemption (no-NAT for VPN traffic) using twice NAT
nat (inside,outside) source static INSIDE-HOSTS INSIDE-HOSTS destination static REMOTE-VPN-NETWORK REMOTE-VPN-NETWORK
```

**NAT order of operations:**
1. Twice NAT (manual NAT) is evaluated first
2. Object NAT (auto NAT) is evaluated second
3. Within the same type, more specific matches win

## 21.6 AAA on ASA

```
! Enable AAA and define TACACS+ server
aaa-server TACACS-GROUP protocol tacacs+
aaa-server TACACS-GROUP (inside) host 10.0.0.50
 key TacacsKey@123

! Authentication for admin access (console, SSH, ASDM)
aaa authentication ssh console TACACS-GROUP LOCAL
aaa authentication serial console LOCAL
aaa authentication http console TACACS-GROUP LOCAL

! Authorisation for commands
aaa authorization command TACACS-GROUP LOCAL

! Accounting
aaa accounting ssh console TACACS-GROUP

! Enable command authorisation
aaa authorization exec authentication-server
```

## 21.7 Service Policies on an ASA

**ASA Service Policies** use a **Modular Policy Framework (MPF)** similar to IOS QoS:
- **class-map** → match traffic
- **policy-map** → define actions
- **service-policy** → apply to interface or globally

**Uses:** IPS inspection, application inspection, rate limiting, QoS

```
! Match all traffic (default class)
class-map inspection_default
 match default-inspection-traffic

! Policy to enable inspection engines
policy-map global_policy
 class inspection_default
  inspect dns
  inspect ftp
  inspect http
  inspect icmp
  inspect smtp
  inspect tftp

! Apply globally (all interfaces)
service-policy global_policy global

! Or apply to specific interface
service-policy global_policy interface outside
```

**HTTP application inspection example:**
```
! Inspect HTTP and block specific content
regex URL-BLOCK ".*\.exe$"

class-map type regex match-any BLOCK-EXE
 match regex URL-BLOCK

policy-map type inspect http HTTP-POLICY
 parameters
  body-match-maximum 1000
 class BLOCK-EXE
  drop-connection log

policy-map global_policy
 class inspection_default
  inspect http HTTP-POLICY
```

## 21.8 ASA Firewall Configuration Summary

> ASA configuration revolves around named interfaces with security levels, object-based ACLs using subnet masks, NAT with Object and Twice NAT, AAA for admin access, and MPF service policies for deep inspection. Always verify with `show` commands after configuring. The ASA differs from IOS routers in important ways — particularly the subnet mask vs wildcard mask distinction in ACLs.

## 21.9 Introduction to ASDM (Optional)

**ASDM (Adaptive Security Device Manager)** is the graphical management interface for the ASA.

**Enabling ASDM:**
```
! Enable HTTPS server (required for ASDM)
http server enable
http 192.168.1.0 255.255.255.0 inside     ! Permit HTTPS from inside network

! Create admin user
username admin password Admin@P@ss privilege 15
aaa authentication http console LOCAL

! Access ASDM via browser
! https://192.168.1.1  (accept self-signed cert; launch ASDM Java app)
```

**ASDM features:**
- **Dashboard** — real-time graphs of connections, CPU, memory, interface stats
- **Configuration wizard** — step-by-step startup configuration
- **ACL Manager** — visual editor for access lists
- **VPN Wizard** — guided site-to-site and AnyConnect VPN setup
- **Monitoring** — firewall events, NAT translations, VPN sessions, routing tables
- **ASDM Syslog Viewer** — real-time log viewing without separate syslog server

**ASDM limitations:**
- Requires Java on the client machine
- Some advanced configurations require CLI
- Not available for all ASA features (e.g., some FirePOWER settings require FMC)

---

# Module 22: Network Security Testing

## 22.1 Network Security Testing Techniques

**Why test?** Finding vulnerabilities before attackers do is far cheaper than dealing with a breach. Security testing validates that controls are functioning as intended.

### Types of Security Testing

| Type | Description | Who performs |
|---|---|---|
| **Vulnerability Assessment** | Identify and catalogue vulnerabilities — does not exploit | Security team / automated scanner |
| **Penetration Testing** | Actively exploit vulnerabilities to prove impact | Authorised ethical hacker |
| **Security Audit** | Compare configuration against policy/standards | Internal audit / compliance team |
| **Red Team Exercise** | Full adversary simulation — test people, processes, technology | External red team |
| **Blue Team / SOC** | Defensive team that detects and responds to red team | Internal security ops |

### Penetration Testing Phases

```
1. Planning & Scoping
   ↓ Define targets, rules of engagement, written authorisation
2. Reconnaissance
   ↓ Passive OSINT + active scanning (Nmap, Shodan, Google dorking)
3. Scanning & Enumeration
   ↓ Port/service discovery, OS fingerprinting, vulnerability scanning
4. Gaining Access (Exploitation)
   ↓ Exploit vulnerabilities; gain initial foothold
5. Maintaining Access
   ↓ Establish persistence; lateral movement; privilege escalation
6. Covering Tracks
   ↓ Remove logs/artefacts (simulated in authorised testing)
7. Reporting
   ↓ Document findings, evidence, risk ratings, and remediation steps
```

### Penetration Test Types (by knowledge)

| Type | Tester Knowledge | Simulates |
|---|---|---|
| **Black box** | None — just target IP/domain | External attacker |
| **White box** | Full — source code, credentials, network diagrams | Thorough internal audit |
| **Grey box** | Partial — some credentials, network overview | Insider or authenticated attacker |

> ⚠️ **CRITICAL:** Always obtain **written authorisation** before any penetration test. Testing without explicit permission is illegal under computer fraud laws (e.g., Computer Fraud and Abuse Act) regardless of intent.

### Security Audit Process

1. **Define scope** — which systems, networks, and policies are included
2. **Gather documentation** — network diagrams, policies, current configurations
3. **Compare against standards** — CIS Benchmarks, vendor hardening guides
4. **Test controls** — verify firewalls, ACLs, IPS, patching are functioning
5. **Analyse logs** — look for anomalies, policy violations, missed alerts
6. **Report** — document findings with risk ratings (CVSS); prioritise by severity
7. **Remediate** — fix issues; implement compensating controls
8. **Verify** — retest to confirm remediation was effective

### CVSS (Common Vulnerability Scoring System)

**CVSS** provides a standardised 0.0–10.0 score for vulnerability severity:

| Score | Severity |
|---|---|
| 0.0 | None |
| 0.1–3.9 | Low |
| 4.0–6.9 | Medium |
| 7.0–8.9 | High |
| 9.0–10.0 | Critical |

**CVSS Base Metrics:**
- **Attack Vector:** Network / Adjacent / Local / Physical
- **Attack Complexity:** Low / High
- **Privileges Required:** None / Low / High
- **User Interaction:** None / Required
- **Scope:** Unchanged / Changed
- **CIA Impact:** None / Low / High (assessed for each of C, I, A)

## 22.2 Network Security Testing Tools

### Network Scanning & Enumeration

| Tool | Purpose |
|---|---|
| **Nmap** | Port scanning, OS detection, service version detection, NSE scripts |
| **Zenmap** | Graphical front-end for Nmap |
| **Angry IP Scanner** | Lightweight IP/port scanner |
| **Netdiscover** | ARP-based host discovery |

**Useful Nmap commands:**
```bash
nmap -sn 192.168.1.0/24          # Ping sweep — find live hosts
nmap -sS 192.168.1.10            # TCP SYN scan (stealth)
nmap -sV 192.168.1.10            # Service version detection
nmap -O 192.168.1.10             # OS fingerprinting
nmap -A 192.168.1.10             # Aggressive scan (OS+version+scripts+traceroute)
nmap -p 1-65535 192.168.1.10     # Full port scan (all 65535 ports)
nmap --script vuln 192.168.1.10  # Run vulnerability detection scripts
```

### Vulnerability Scanners

| Tool | Notes |
|---|---|
| **Nessus** | Industry standard; commercial; comprehensive plugin library |
| **OpenVAS** | Open-source vulnerability scanner; free |
| **Qualys** | Cloud-based; enterprise scanner |
| **Retina** | Enterprise vulnerability scanner |

### Packet Analysis

| Tool | Purpose |
|---|---|
| **Wireshark** | GUI packet capture and deep protocol analysis |
| **tcpdump** | CLI packet capture; lightweight; great for remote sessions |
| **NetworkMiner** | Passive network sniffer; file carving from captures |

### Exploitation Frameworks

| Tool | Purpose |
|---|---|
| **Metasploit Framework** | Exploitation framework; thousands of modules; post-exploitation |
| **Core Impact** | Commercial exploitation framework |
| **Cobalt Strike** | Advanced red team C2 framework |

### Password Testing

| Tool | Purpose |
|---|---|
| **Hashcat** | GPU-accelerated hash cracking (dictionary, brute, rules) |
| **John the Ripper** | Classic password cracker; many hash types |
| **Hydra** | Online password brute-forcer (SSH, FTP, HTTP, etc.) |
| **Medusa** | Similar to Hydra — parallel online password attacks |

### Wireless Testing

| Tool | Purpose |
|---|---|
| **Aircrack-ng** | WEP/WPA key cracking; packet injection |
| **Kismet** | Wireless network detection and sniffing |
| **Wireshark** | Capture 802.11 frames (with monitor-mode adapter) |

### Cisco-Specific Security Tools

| Tool | Purpose |
|---|---|
| **Cisco CCP (Configuration Professional)** | GUI device management and security audit |
| **Cisco Secure Network Analytics (Stealthwatch)** | NetFlow-based behaviour analytics |
| **Cisco Vulnerability Manager** | CVE tracking for Cisco IOS/ASA |
| **Cisco IOS AutoSecure** | Automated hardening baseline |

### Web Application Testing

| Tool | Purpose |
|---|---|
| **Burp Suite** | Web proxy for intercepting and modifying HTTP/S traffic |
| **OWASP ZAP** | Free web application scanner |
| **SQLmap** | Automated SQL injection detection and exploitation |
| **Nikto** | Web server scanner (misconfiguration, vulnerabilities) |

## 22.3 Network Security Testing Summary

> Security testing must be authorised, scoped, and documented. Vulnerability assessments identify weaknesses; penetration tests prove their impact. CVSS provides standardised severity scoring to prioritise remediation. The right tool depends on the task — Nmap for discovery, Nessus/OpenVAS for vulnerability scanning, Metasploit for exploitation, Wireshark for traffic analysis. Always report findings clearly with risk context and remediation recommendations.

---

# 📌 Comprehensive Quick-Reference

## Attack → Defence Mapping

| Attack | Layer | Defence |
|---|---|---|
| Phishing / Social engineering | Human | Awareness training, email filtering, MFA |
| Password brute force | Access | Account lockout, MFA, strong policy, `login block-for` |
| Buffer overflow | Application | ASLR, DEP, stack canaries, patching |
| IP spoofing | Network | uRPF (`ip verify unicast source`), BCP38 ingress filtering |
| Packet sniffing | Network/Data | Encryption (TLS, IPsec, SSH), switch segmentation |
| DoS / DDoS | Network | Rate limiting, upstream filtering, scrubbing centres |
| ARP spoofing | Data Link | Dynamic ARP Inspection (DAI) |
| DHCP starvation/spoofing | Data Link | DHCP Snooping |
| MAC flooding | Data Link | Port Security |
| VLAN hopping | Data Link | Disable DTP (`nonegotiate`), unused native VLAN |
| STP manipulation | Data Link | BPDU Guard, Root Guard, PortFast |
| DNS poisoning | Application | DNSSEC, trusted resolvers |
| SQL injection | Application | Input validation, parameterised queries, WAF |
| Reconnaissance | Pre-attack | Close unused ports, firewall ICMP, IDS/IPS |
| Man-in-the-Middle | Network | Encryption, PKI certificates, HTTPS |
| Session hijacking | Application | HTTPS, Secure cookie flags, session timeouts |

## Protocol Security Reference

| Protocol | Port | Secure? | Secure Alternative |
|---|---|---|---|
| Telnet | TCP 23 | ❌ Plaintext | SSH v2 (TCP 22) |
| FTP | TCP 20/21 | ❌ Plaintext | SFTP/SCP (TCP 22) or FTPS (TCP 990) |
| HTTP | TCP 80 | ❌ Plaintext | HTTPS (TCP 443) |
| SNMP v1/v2c | UDP 161 | ❌ Community string | SNMPv3 (auth+priv) |
| LDAP | TCP 389 | ❌ Plaintext | LDAPS (TCP 636) |
| SMTP | TCP 25 | ❌ (base) | SMTPS (465) / STARTTLS (587) |
| HTTP management | TCP 80 | ❌ | HTTPS (443) or SSH |
| SSH v2 | TCP 22 | ✅ | — |
| HTTPS/TLS | TCP 443 | ✅ | — |
| SNMPv3 | UDP 161 | ✅ | — |
| TACACS+ | TCP 49 | ✅ (full encryption) | — |
| RADIUS | UDP 1812/1813 | ⚠️ Password only | — |
| IPsec ESP | Protocol 50 | ✅ | — |
| IPsec AH | Protocol 51 | ⚠️ No encryption | Use ESP instead |

## Key Number Reference

| Number | Meaning |
|---|---|
| **0–99, 1300–1999** | Standard ACL range |
| **100–199, 2000–2699** | Extended ACL range |
| **0–100** | ASA security level range |
| **0–15** | IOS privilege level range |
| **0–7** | Syslog severity level range |
| **TCP 22** | SSH |
| **TCP 23** | Telnet |
| **TCP 49** | TACACS+ |
| **UDP 161/162** | SNMP (queries/traps) |
| **UDP 500** | IKE Phase 1 |
| **UDP 4500** | IKE NAT-T |
| **Protocol 50** | IPsec ESP |
| **Protocol 51** | IPsec AH |
| **UDP 1812/1813** | RADIUS auth/accounting |
| **DH Group 14** | 2048-bit — minimum recommended |
| **DH Group 19** | 256-bit ECDH — recommended |

## IOS Security Command Quick Reference

```
! ── Access & Passwords ─────────────────────────────────
enable secret <password>
service password-encryption
security passwords min-length 10
login block-for 120 attempts 3 within 60
login on-failure log

! ── SSH ───────────────────────────────────────────────
ip domain-name <name>
crypto key generate rsa modulus 2048
ip ssh version 2
ip ssh time-out 60
line vty 0 4
 login local
 transport input ssh
 exec-timeout 5 0

! ── Banners ────────────────────────────────────────────
banner motd # Authorised access only. Monitored. #

! ── Syslog ─────────────────────────────────────────────
logging host 10.0.0.100
logging trap warnings
service timestamps log datetime msec

! ── NTP ────────────────────────────────────────────────
ntp authenticate
ntp authentication-key 1 md5 <key>
ntp trusted-key 1
ntp server 10.0.0.1 key 1

! ── AutoSecure ─────────────────────────────────────────
auto secure

! ── OSPF Authentication ────────────────────────────────
interface Gi0/0
 ip ospf message-digest-key 1 md5 <key>
 ip ospf authentication message-digest

! ── AAA ────────────────────────────────────────────────
aaa new-model
aaa authentication login default group tacacs+ local
aaa authorization exec default group tacacs+ local
aaa accounting exec default start-stop group tacacs+

! ── DHCP Snooping ──────────────────────────────────────
ip dhcp snooping
ip dhcp snooping vlan 10
interface Gi0/24
 ip dhcp snooping trust

! ── Dynamic ARP Inspection ─────────────────────────────
ip arp inspection vlan 10
interface Gi0/24
 ip arp inspection trust

! ── Port Security ──────────────────────────────────────
interface Gi0/1
 switchport mode access
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict

! ── STP Protection ─────────────────────────────────────
interface Gi0/1
 spanning-tree portfast
 spanning-tree bpduguard enable
interface Gi0/24
 spanning-tree guard root

! ── IP Source Guard ────────────────────────────────────
interface Gi0/1
 ip verify source

! ── ZPF (key steps) ────────────────────────────────────
zone security INSIDE
zone security OUTSIDE
interface Gi0/0
 zone-member security INSIDE
zone-pair security IN-OUT source INSIDE destination OUTSIDE
 service-policy type inspect MY-POLICY
```

## Cryptography Quick Reference

| Need | Use |
|---|---|
| Bulk encryption | AES-256-GCM |
| Key exchange | ECDH Group 19/20, or DH Group 14 minimum |
| Digital signatures | ECDSA or RSA 2048+ |
| Integrity only | HMAC-SHA256 |
| Password hashing | Argon2, bcrypt, PBKDF2 |
| Avoid | MD5, SHA-1, DES, 3DES, RC4, DH Groups 1/2/5 |

## IPsec Quick Reference

| Phase | What | Parameters |
|---|---|---|
| **Phase 1 (ISAKMP SA)** | Secure channel | Encryption: AES-256 / Hash: SHA-256 / Auth: PSK or cert / DH: Group 14+ / Lifetime: 86400s |
| **Phase 2 (IPsec SA)** | Data encryption | Transform: ESP-AES256-SHA256 / Mode: Tunnel / PFS: Group 14+ / Lifetime: 3600s |

## RADIUS vs TACACS+ — Exam Favourite

| | RADIUS | TACACS+ |
|---|---|---|
| **Protocol** | UDP | TCP |
| **Port** | 1812/1813 | 49 |
| **Encryption** | Password only | **Entire packet** |
| **AAA separation** | Combined | **Fully separated** |
| **Command authorisation** | Limited | **Granular** |
| **Best for** | Network access | **Device admin** |

---

*🔐 Cisco NetAcad Network Security 1.0 — All 22 Modules | Good luck!*
