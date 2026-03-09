# 🔐 Cisco NetAcad – Network Security 1.0
### Comprehensive Study Notes — All 22 Modules

---

> **Course:** Cisco Networking Academy – Network Security 1.0  
> **Modules:** 22 | **Labs:** 23 | **Packet Tracer Activities:** 22  
> **Recommended Next:** CyberOps Associate, IoT Security

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
| 16 | [Basic Integrity and Authentication](#module-16-basic-integrity-and-authentication) |
| 17 | [Public Key Cryptography](#module-17-public-key-cryptography) |
| 18 | [VPNs](#module-18-vpns) |
| 19 | [Implement Site-to-Site IPsec VPNs](#module-19-implement-site-to-site-ipsec-vpns) |
| 20 | [Introduction to the ASA](#module-20-introduction-to-the-asa) |
| 21 | [ASA Firewall Configuration](#module-21-asa-firewall-configuration) |
| 22 | [Network Security Testing](#module-22-network-security-testing) |

---

# Module 1: Securing Networks

## 1.1 Why Network Security?

Network security breaches can:
- Disrupt e-commerce and business operations
- Cause **loss of business data**
- **Threaten people's privacy**
- Compromise the **integrity of information**
- Damage company reputation and shareholder value

## 1.2 Network Topologies and Their Security Concerns

| Network Type | Description | Security Concern |
|---|---|---|
| **SOHO** | Small Office/Home Office; consumer-grade router | Weak default configs, limited security features |
| **Campus Area Network (CAN)** | Interconnected networks in nearby buildings | Insider threats, physical access |
| **Wide Area Network (WAN)** | Spans large geographic areas (state/country) | Transit traffic exposure |
| **Data Centre Network** | Off-site facility; VPN-connected to corporate | Physical security, VM sprawl |
| **Cloud** | Third-party hosted infrastructure | Shared responsibility, data sovereignty |

## 1.3 The Network Security Architecture

A **secure network architecture** has three layers:

**Access Layer (Layer 2 Switches)**
- Connect user-facing ports to the network
- Enforce port security, 802.1X, DHCP snooping

**Distribution Layer (Layer 3 Switches)**
- Provide secure redundant trunk connections to Layer 2 switches
- Enforce routing policies, ACLs

**Core Layer**
- High-speed backbone
- Minimal security policy enforcement here (speed priority)

## 1.4 Attack Vectors

An **attack vector** is a path by which a threat actor can gain access to a server or network.

Common attack vectors:
- **Email/Web** – phishing, malicious links, drive-by downloads
- **Removable media** – infected USB drives
- **Cloud services** – misconfigured S3 buckets, credential theft
- **Wireless** – rogue APs, evil twin attacks
- **Supply chain** – compromised hardware or software vendors
- **Physical access** – tailgating, theft of devices

## 1.5 Physical Security

**Outside Perimeter Security:**
- Security officers, fences, gates, CCTV, breach alarms

**Inside Perimeter Security:**
- CCTV, motion detectors, security traps, biometric access

## 1.6 Virtualisation Security Concerns

| Threat | Description |
|---|---|
| **Hyperjacking** | Attacker hijacks the VM hypervisor and launches attacks from it |
| **Instant-on activation** | A long-dormant VM is brought back online with outdated security policies |
| **VM escape** | Malware breaks out of a VM to affect the host |
| **VM sprawl** | Unmanaged/forgotten VMs increase the attack surface |

## 1.7 Cisco Network Security Tools

| Tool | Purpose |
|---|---|
| **ESA (Email Security Appliance)** | Filters spam, phishing, malware in email |
| **WSA (Web Security Appliance)** | URL filtering, malware scanning for web traffic |
| **ASA (Adaptive Security Appliance)** | Stateful firewall, VPN concentrator |
| **ISE (Identity Services Engine)** | AAA, 802.1X, posture assessment |
| **Firepower / NGIPS** | Next-gen IPS with application awareness |

---

# Module 2: Network Threats

## 2.1 Types of Threat Actors

| Actor | Motivation | Skill |
|---|---|---|
| **Script kiddies** | Curiosity, reputation | Low – use pre-built tools |
| **Hacktivists** | Political/social agenda | Moderate |
| **Cybercriminals** | Financial gain | Moderate–High |
| **State-sponsored / APT** | Espionage, sabotage | Very High |
| **Insiders** | Revenge, financial gain, accidental | Varies |

## 2.2 Hacker Terminology

| Term | Meaning |
|---|---|
| **White hat** | Ethical hacker — authorised penetration tester |
| **Black hat** | Malicious hacker — criminal intent |
| **Grey hat** | Breaks rules but without malicious intent; may disclose vulnerabilities |
| **Cracker** | Breaks into systems to steal data |
| **Hacktivist** | Hacks for political/social causes |
| **Script kiddie** | Uses others' tools without understanding them |

## 2.3 Malware Types

### Viruses
- Attach to legitimate programs/files; require user action to spread
- **Types:** File infector, Boot-sector, Macro, Polymorphic, Metamorphic
- Polymorphic virus changes its code to avoid signature detection

### Worms
- **Self-replicating** — no user action needed
- Exploit network vulnerabilities to spread autonomously
- Can consume massive bandwidth (e.g., Code Red, WannaCry)

### Trojans
- Disguised as legitimate software
- Do NOT self-replicate
- Subtypes:
  - **RAT** – Remote Access Trojan; gives attacker full remote control
  - **Backdoor** – persistent hidden access
  - **Downloader** – fetches additional malware
  - **Banking Trojan** – steals financial credentials

### Ransomware
- Encrypts victim files; demands payment (usually cryptocurrency) for decryption
- Delivered via: phishing, exploit kits, RDP brute force
- Notable examples: WannaCry (2017), CryptoLocker, REvil

### Rootkits
- Operate at kernel/firmware level
- Conceal the presence of other malware
- Extremely difficult to detect/remove
- May survive OS reinstall (bootkits)

### Spyware & Adware
- **Spyware:** Captures keystrokes, credentials, browsing activity — sent to attacker
- **Adware:** Displays unwanted ads; often bundled with "free" software
- **Grayware:** Potentially unwanted programs that aren't clearly malicious

### Botnets
- **Bot:** Compromised host under attacker control ("zombie")
- **Botnet:** Network of thousands/millions of bots
- **C2 (Command & Control) server:** Issues commands to the botnet
- Uses: DDoS, spam campaigns, credential stuffing, crypto-mining

## 2.4 Social Engineering

Manipulating people rather than technology.

| Technique | Description |
|---|---|
| **Phishing** | Fraudulent email mimicking a trusted entity |
| **Spear phishing** | Targeted phishing using personal details |
| **Whaling** | Phishing targeting executives (CEO, CFO) |
| **Vishing** | Phone-based phishing |
| **Smishing** | SMS-based phishing |
| **Pretexting** | Fabricated scenario to gain trust ("I'm from IT support") |
| **Baiting** | Leaving infected USB in a car park/lobby |
| **Tailgating/Piggybacking** | Following an authorised person through a secured door |
| **Quid pro quo** | Offering something in exchange for information or access |
| **Watering hole** | Infecting websites frequently visited by targets |

> 💡 **Key Insight:** Social engineering exploits human psychology (urgency, authority, fear, helpfulness) — not technical vulnerabilities.

## 2.5 Common Network Attacks Overview

### Reconnaissance Attacks
Goal: Gather information before attacking.
- **Passive:** Packet sniffing, OSINT, traffic analysis (no packets sent)
- **Active:** Ping sweeps, port scans, OS fingerprinting, vulnerability scanning

### Access Attacks
Goal: Gain unauthorised access to systems.
- Password attacks (brute force, dictionary, credential stuffing)
- Trust exploitation (forging trusted relationships between systems)
- Port redirection / man-in-the-middle
- Social engineering

### DoS / DDoS Attacks
Goal: Make a resource unavailable to legitimate users.

### Reconnaissance → Access → DoS is the typical attack progression.

---

# Module 3: Mitigating Threats

## 3.1 Defence-in-Depth Strategy

No single security control is sufficient. **Defence in depth** means layering multiple overlapping controls so that the failure of one does not compromise the entire system.

```
[Internet]
    ↓
[Perimeter Firewall / IPS]
    ↓
[DMZ – Public-facing servers]
    ↓
[Internal Firewall]
    ↓
[Core Network – Routing / Switching Controls]
    ↓
[Endpoints – Host AV, HIPS, Encryption]
    ↓
[Data – Encryption, DLP, Access Control]
```

## 3.2 Security Policy

A **security policy** is a set of objectives and rules that defines how an organisation protects its information assets.

Components:
- **Acceptable Use Policy (AUP)** – rules for how employees may use systems
- **Data Classification Policy** – sensitivity levels (Public / Internal / Confidential / Restricted)
- **Incident Response Policy** – steps to take when a breach occurs
- **Password Policy** – minimum length, complexity, expiry
- **Remote Access Policy** – VPN requirements, device standards

## 3.3 The CIA Triad

| Pillar | Definition | Threatened by | Control |
|---|---|---|---|
| **Confidentiality** | Only authorised parties can read data | Sniffing, data theft | Encryption, access control |
| **Integrity** | Data is accurate and unmodified | MitM, corruption | Hashing, digital signatures |
| **Availability** | Services accessible when needed | DoS/DDoS, hardware failure | Redundancy, rate limiting |

## 3.4 Security Frameworks

| Framework | Description |
|---|---|
| **NIST CSF** | Identify → Protect → Detect → Respond → Recover |
| **ISO/IEC 27001** | International standard for information security management |
| **CIS Controls** | Prioritised set of 18 defensive actions |

## 3.5 Threat Intelligence & IoCs

**Indicators of Compromise (IoCs)** are evidence that an attack has occurred:
- Malware file hashes
- IP addresses of C2 servers
- Suspicious domain names
- Unusual file paths or registry keys
- Characteristic changes to end-system software

Organisations share IoCs via platforms like **MISP**, **STIX/TAXII**, and **Cisco Threat Intelligence Director (TID)**.

## 3.6 Types of Security Controls

| Category | Examples |
|---|---|
| **Preventive** | Firewall, encryption, access control, patching |
| **Detective** | IDS, SIEM, audit logs, anomaly detection |
| **Corrective** | Incident response, backups, patch management |
| **Deterrent** | Warning banners, security cameras, policies |
| **Compensating** | Controls used when primary controls are not feasible |

## 3.7 Risk Management

**Risk = Likelihood × Impact**

Steps:
1. **Identify assets** – hardware, software, data, people
2. **Identify threats and vulnerabilities**
3. **Assess risk** – likelihood and impact
4. **Select controls** – reduce, accept, transfer, or avoid risk
5. **Monitor** – continuously reassess

---

# Module 4: Secure Device Access

## 4.1 Securing Cisco IOS – Fundamentals

Every network device is a potential attack target. Hardening eliminates unnecessary services and secures all access points.

**Principle of Least Privilege:** Users and processes should only have the minimum access required for their function.

## 4.2 Passwords and Local Authentication

```
! Always use enable secret (MD5 hash), never enable password
enable secret StrongP@ssw0rd!

! Encrypt all plaintext passwords
service password-encryption

! Enforce minimum password length
security passwords min-length 10

! Create local user with privilege level
username admin privilege 15 secret Admin@P@ss!

! Block logins after repeated failures
login block-for 120 attempts 3 within 60
```

> 💡 `enable password` stores the password in weak reversible encryption. **Always** use `enable secret` instead.

## 4.3 Securing Console and AUX Access

```
line console 0
 password C0ns0leP@ss
 login
 exec-timeout 5 0        ! Auto-logout after 5 minutes of inactivity
 logging synchronous     ! Prevent log messages interrupting typing

line aux 0
 no exec                 ! Disable exec on AUX (rarely needed)
 transport input none
```

## 4.4 Securing VTY (Remote Access) Lines – SSH Only

```
! Step 1: Set hostname and domain (required for SSH key generation)
hostname R1
ip domain-name company.com

! Step 2: Create local user
username admin privilege 15 secret SSH@Admin1

! Step 3: Generate RSA key pair (minimum 1024 bits, recommend 2048)
crypto key generate rsa modulus 2048

! Step 4: Set SSH version 2
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 3

! Step 5: Restrict VTY to SSH only
line vty 0 4
 login local
 transport input ssh
 exec-timeout 5 0
```

> 💡 **Never use Telnet** – it transmits everything including passwords in plaintext. Telnet can be verified and sniffed with Wireshark.

## 4.5 Disabling Unused Services

```
! Disable HTTP/HTTPS management server
no ip http server
no ip http secure-server

! Disable Cisco Discovery Protocol (CDP) globally (re-enable per interface if needed)
no cdp run

! Disable other unnecessary services
no service finger
no service tcp-small-servers
no service udp-small-servers
no ip bootp server
no ip source-route

! Disable proxy ARP on interfaces facing untrusted networks
interface GigabitEthernet0/1
 no ip proxy-arp
 no ip directed-broadcast
```

## 4.6 Login Banners

```
banner motd ^
=======================================================
 AUTHORISED ACCESS ONLY
 All activity on this device is monitored and logged.
 Unauthorised access is prohibited and will be
 reported to law enforcement.
=======================================================
^
```

> 💡 **Never** use welcoming language ("Welcome to...") in banners. This may negate legal protections. Always warn that the system is monitored and access is restricted.

## 4.7 Securing SNMP

| Version | Security | Notes |
|---|---|---|
| SNMPv1 | None | Community string in cleartext — never use |
| SNMPv2c | None | Still community string in cleartext — avoid |
| SNMPv3 | Auth + Encryption | Only acceptable version for production |

```
! SNMPv3 configuration
snmp-server group SECUREGROUP v3 priv
snmp-server user SNMPUSER SECUREGROUP v3 auth sha AuthPass123 priv aes 128 PrivPass123
snmp-server host 10.0.0.50 version 3 priv SNMPUSER

! Restrict SNMP access with ACL
access-list 10 permit host 10.0.0.50
snmp-server community DEPRECATED ro 10   ! Avoid community strings entirely if possible
```

---

# Module 5: Assigning Administrative Roles

## 5.1 Privilege Levels in Cisco IOS

Cisco IOS has 16 privilege levels (0–15):

| Level | Default Access | Description |
|---|---|---|
| 0 | Very limited | Only: `disable`, `enable`, `exit`, `help`, `logout` |
| 1 | User EXEC | Default `>` prompt; read-only, limited commands |
| 2–14 | Custom | Administrator-defined |
| 15 | Privileged EXEC | Full access `#` prompt; all commands |

```
! Set privilege level for a user
username netadmin privilege 10 secret NetAdmin@P@ss

! Assign specific commands to a custom privilege level
privilege exec level 10 show running-config
privilege exec level 10 debug ip ospf
privilege exec level 10 ping
privilege exec level 10 traceroute

! Apply privilege level to a VTY line
line vty 0 4
 login local
 privilege level 10
```

## 5.2 Role-Based CLI Access (CLI Views)

**CLI Views** are a more granular alternative to privilege levels, allowing precise command-level access control.

**Types:**
- **Root view** – Has access to all commands; required to create/manage other views
- **CLI view** – Custom set of commands
- **Superview** – Group of CLI views (user can access commands from multiple views)

```
! Step 1: Enable AAA (required for views)
aaa new-model

! Step 2: Enter root view (from privilege level 15)
enable view

! Step 3: Create a CLI view
parser view SHOWONLY
 secret ShowOnly@P@ss
 commands exec include show
 commands exec include ping
 commands exec include exit

! Step 4: Create a superview
parser view SUPPORT superview
 secret Support@P@ss
 view SHOWONLY

! Verify views
show parser view all
```

## 5.3 Accessing a CLI View

```
! Switch to a specific view
enable view SHOWONLY
! (prompted for the view password)

! Check current view
show parser view
```

## 5.4 Why Use Views Over Privilege Levels?

| Feature | Privilege Levels | CLI Views |
|---|---|---|
| Command granularity | Coarse (all or most commands) | Fine (exact commands) |
| Applies to show subcommands separately | No | Yes |
| Grouping | No superview concept | Superview groups views |
| Complexity | Simple | More complex to configure |

---

# Module 6: Device Monitoring and Management

## 6.1 Syslog

**Syslog** sends log messages from network devices to a centralised log server for storage, analysis, and alerting.

### Syslog Severity Levels

| Level | Name | Meaning |
|---|---|---|
| 0 | **Emergency** | System is unusable |
| 1 | **Alert** | Immediate action required |
| 2 | **Critical** | Critical conditions |
| 3 | **Error** | Error conditions |
| 4 | **Warning** | Warning conditions |
| 5 | **Notice** | Normal but significant event |
| 6 | **Informational** | Informational messages |
| 7 | **Debug** | Debug-level messages (very verbose) |

> 💡 **Memory Aid:** "**E**very **A**wful **C**isco **E**ngineer **W**ill **N**eed **I**ce **D**aily" (Emergency, Alert, Critical, Error, Warning, Notice, Informational, Debug)

```
! Configure syslog
logging host 10.0.0.100           ! Syslog server IP
logging trap warnings              ! Log levels 0–4 (Warning and above)
logging source-interface Loopback0 ! Use consistent source IP
logging on

! Add timestamps to log messages (important for correlation)
service timestamps log datetime msec
service timestamps debug datetime msec
```

## 6.2 NTP (Network Time Protocol)

**Why NTP matters for security:**
- Log correlation across devices requires synchronised clocks
- Certificate validity checks depend on accurate time
- Forensic investigations require reliable timestamps

```
! Configure NTP client
ntp server 10.0.0.1

! NTP Authentication (prevents rogue NTP servers)
ntp authenticate
ntp authentication-key 1 md5 NTPSecret@Key
ntp trusted-key 1
ntp server 10.0.0.1 key 1

! Verify NTP
show ntp status
show ntp associations
```

## 6.3 SNMP (Simple Network Management Protocol)

Used to **monitor and manage** network devices.

**Components:**
- **NMS (Network Management Station)** – collects and analyses SNMP data
- **Agent** – runs on managed devices; stores MIB data
- **MIB (Management Information Base)** – database of device attributes
- **OID (Object Identifier)** – unique ID for each MIB variable

**SNMP Operations:**
- `GET` – NMS requests data from agent
- `SET` – NMS changes a value on agent
- `TRAP` – Agent sends unsolicited alert to NMS
- `INFORM` – Like TRAP but requires acknowledgement (SNMPv2c/v3)

> 💡 Always use **SNMPv3** in production. v1/v2c send community strings in cleartext.

## 6.4 NetFlow

**NetFlow** collects metadata about IP traffic flows (not packet content).

A **flow** is defined by 5-tuple: source IP, destination IP, source port, destination port, protocol.

**Uses:**
- Detect anomalies (unusual traffic patterns)
- Capacity planning
- Security investigations (who talked to whom, when, how much)
- Billing

```
! Enable NetFlow on interfaces
interface GigabitEthernet0/0
 ip flow ingress
 ip flow egress

! Export to NetFlow collector
ip flow-export destination 10.0.0.200 9996
ip flow-export version 9
ip flow-export source GigabitEthernet0/0

! View active flows
show ip cache flow
```

## 6.5 Secure Copy Protocol (SCP)

**SCP** allows secure file transfer to/from Cisco devices (uses SSH as transport).

```
! Enable SCP server on Cisco device (requires AAA and SSH)
ip scp server enable

! Copy config to SCP server from IOS
copy running-config scp://admin@10.0.0.50/backup-r1.cfg
```

## 6.6 SIEM (Security Information and Event Management)

A **SIEM** aggregates logs from all sources and correlates events to detect incidents.

| SIEM Function | Description |
|---|---|
| **Aggregation** | Collects logs from firewalls, IDS, servers, endpoints |
| **Normalisation** | Converts different log formats to a common schema |
| **Correlation** | Links related events across sources and time |
| **Alerting** | Notifies analysts of suspicious patterns |
| **Forensics** | Historical search for investigation |
| **Reporting** | Dashboards, compliance reports |

Examples: Splunk, IBM QRadar, Microsoft Sentinel, Elastic SIEM, Cisco SecureX

---

# Module 7: Authentication, Authorization, and Accounting (AAA)

## 7.1 AAA Overview

| Component | Question | Purpose |
|---|---|---|
| **Authentication** | Who are you? | Verify identity |
| **Authorization** | What can you do? | Control resource access |
| **Accounting** | What did you do? | Audit trail, compliance |

**Without AAA:** Local authentication only — difficult to manage at scale, no central audit.

**With AAA:** Centralised policy, scalable, detailed logging.

```
! Enable AAA globally
aaa new-model
```

## 7.2 RADIUS vs TACACS+

| Feature | RADIUS | TACACS+ |
|---|---|---|
| **Standard** | Open RFC (2865/2866) | Cisco proprietary |
| **Transport** | UDP | TCP |
| **Ports** | 1812 (auth), 1813 (acct) | 49 |
| **Encryption** | Password only | Entire packet body |
| **AAA separation** | Combined auth + authz | Fully separated |
| **Command authorisation** | Limited | Granular per-command |
| **Best for** | Network access (802.1X, VPN) | Device administration |

> 💡 **Exam Tip:** TACACS+ → TCP, port 49, encrypts entire packet, separates AAA, best for device admin. RADIUS → UDP, ports 1812/1813, encrypts password only, best for network access.

## 7.3 Configuring Server-Based AAA

```
! TACACS+ Server Configuration
aaa new-model

tacacs server ISE-SERVER
 address ipv4 10.0.0.50
 key TacacsSecretKey123

! Authentication: try TACACS+ first, fall back to local
aaa authentication login default group tacacs+ local

! Authorization: use TACACS+ for exec sessions
aaa authorization exec default group tacacs+ local if-authenticated

! Accounting: log start/stop of exec sessions
aaa accounting exec default start-stop group tacacs+

! Apply to lines
line vty 0 4
 login authentication default
```

## 7.4 Local AAA (Fallback)

```
! Local database for fallback when server is unreachable
username admin privilege 15 secret LocalAdmin@P@ss
aaa authentication login default group tacacs+ local
```

> 💡 Always include `local` as the last method so admins can still log in if the AAA server is unavailable.

## 7.5 802.1X – Port-Based Network Access Control

**802.1X** prevents unauthorised devices from connecting to the network.

**Three roles:**
| Role | Device | Example |
|---|---|---|
| **Supplicant** | Device requesting access | PC, phone |
| **Authenticator** | Enforces access control | Switch, WAP |
| **Authentication Server** | Validates credentials | RADIUS / Cisco ISE |

**Process:**
1. Device connects to switch port → port is in **Unauthorised** state
2. Switch sends EAP-Request/Identity
3. Device responds with credentials
4. Switch forwards to RADIUS server
5. RADIUS returns Accept/Reject
6. Port moves to **Authorised** or stays blocked

```
! Enable 802.1X on a switch
dot1x system-auth-control

interface GigabitEthernet0/1
 switchport mode access
 dot1x pae authenticator
 authentication port-control auto
 spanning-tree portfast
```

## 7.6 EAP Methods

| Method | Security | Notes |
|---|---|---|
| **EAP-MD5** | Weak | No mutual auth, vulnerable to dictionary attacks |
| **EAP-TLS** | Very Strong | Requires certificates on both client and server |
| **PEAP** | Strong | Tunnels MS-CHAPv2 inside TLS — most common |
| **EAP-TTLS** | Strong | Tunnels various auth methods inside TLS |
| **EAP-FAST** | Strong | Cisco proprietary; uses PAC file instead of certificates |

---

# Module 8: Access Control Lists

## 8.1 ACL Fundamentals

An **ACL** is an ordered set of rules (ACEs – Access Control Entries) that permits or denies packets.

**Key rules:**
- Processed **top-down** — first match wins
- **Implicit deny all** at the end of every ACL (not visible in config)
- Applied to an interface in a **direction** (in = inbound, out = outbound)
- **Inbound ACL:** applied before routing decision
- **Outbound ACL:** applied after routing decision

## 8.2 Standard ACLs

Filter on **source IP address only**.

- Numbered: **1–99** and **1300–1999**
- Should be placed **close to the destination** (since they only match source IP, placing them near the source would block too broadly)

```
! Numbered standard ACL
access-list 10 remark Permit Sales network
access-list 10 permit 192.168.10.0 0.0.0.255
access-list 10 deny any

! Apply outbound on interface
interface GigabitEthernet0/0
 ip access-group 10 out

! Named standard ACL
ip access-list standard PERMIT-MGMT
 remark Allow only management subnet
 permit 10.10.10.0 0.0.0.255
 deny any log
```

## 8.3 Extended ACLs

Filter on **source IP, destination IP, protocol, and port**.

- Numbered: **100–199** and **2000–2699**
- Should be placed **close to the source** (to stop unwanted traffic early)

```
! Permit HTTP/HTTPS to web server; deny everything else
access-list 110 remark Web server access
access-list 110 permit tcp any host 10.0.1.10 eq 80
access-list 110 permit tcp any host 10.0.1.10 eq 443
access-list 110 deny ip any any log

! Named extended ACL
ip access-list extended OUTBOUND-FILTER
 remark Block Telnet
 deny tcp any any eq 23 log
 remark Block unencrypted FTP
 deny tcp any any eq 21 log
 remark Permit everything else
 permit ip any any

interface GigabitEthernet0/1
 ip access-group OUTBOUND-FILTER out
```

## 8.4 Wildcard Masks

Wildcard mask = inverse of subnet mask:
- **0 bit** = must match this bit
- **1 bit** = ignore (any value)

| To Match | Wildcard Mask | IOS Keyword |
|---|---|---|
| One specific host | 0.0.0.0 | `host` |
| /24 network | 0.0.0.255 | — |
| /16 network | 0.0.255.255 | — |
| Any address | 255.255.255.255 | `any` |

> 💡 `permit host 192.168.1.5` = `permit 192.168.1.5 0.0.0.0`
> 💡 `permit any` = `permit 0.0.0.0 255.255.255.255`

## 8.5 ACL Placement Guidelines

| ACL Type | Place Near | Why |
|---|---|---|
| **Standard** | Destination | Avoids blocking traffic from source unnecessarily |
| **Extended** | Source | Stops traffic as early as possible, saves bandwidth |

## 8.6 Editing Named ACLs (Sequence Numbers)

```
ip access-list extended OUTBOUND-FILTER
 no 30                              ! Delete ACE at sequence 30
 15 permit tcp 10.0.0.0 0.0.0.255 any eq 443  ! Insert at sequence 15

show ip access-lists OUTBOUND-FILTER
show access-lists
```

## 8.7 IPv6 ACLs

- Named only (no numbered IPv6 ACLs)
- Must explicitly permit **ICMPv6 NDP** (Neighbor Discovery Protocol) messages — otherwise IPv6 will break

```
ipv6 access-list IPV6-FILTER
 permit icmp any any nd-na
 permit icmp any any nd-ns
 permit tcp any any eq 443
 deny ipv6 any any log

interface GigabitEthernet0/0
 ipv6 traffic-filter IPV6-FILTER in
```

## 8.8 Verifying ACLs

```
show ip access-lists                    ! All IPv4 ACLs with hit counters
show ip access-lists OUTBOUND-FILTER    ! Specific named ACL
show ip interface GigabitEthernet0/0    ! Shows ACLs applied to an interface
clear ip access-list counters           ! Reset hit counters
```

---

# Module 9: Firewall Technologies

## 9.1 Firewall Overview

A **firewall** is a network security device that monitors and controls traffic based on security rules.

**Core functions:**
- Enforce access policy between network segments
- Track connection state
- Perform NAT/PAT
- Log allowed and denied traffic

## 9.2 Types of Firewalls

### Packet Filtering (Stateless)
- Inspects Layer 3–4 headers only (IP addresses, ports, protocols)
- **No connection tracking** — each packet evaluated independently
- Fast, but cannot distinguish return traffic from new traffic
- Implemented via **ACLs on routers**
- Susceptible to IP spoofing and fragmentation attacks

### Stateful Inspection Firewall
- Maintains a **state table** tracking active connections
- Automatically allows return traffic matching established sessions
- Default for most modern firewall products
- Cannot inspect **application-layer content**

### Application Layer Gateway (Proxy Firewall)
- Operates at **Layer 7**
- Terminates and re-originates connections (full proxy)
- Deep content inspection — can filter commands within protocols (e.g., block FTP PUT)
- Higher latency due to processing
- Protocol-aware (HTTP, FTP, DNS, SMTP)

### Next-Generation Firewall (NGFW)
All of the above, plus:
- **Application identification** (regardless of port — stops port evasion)
- **User identity awareness** (integrates with AD, ISE)
- **SSL/TLS inspection** (decrypt → inspect → re-encrypt)
- **Integrated IPS**
- **Threat intelligence** feeds
- Examples: **Cisco Firepower**, Palo Alto, Fortinet

### Unified Threat Management (UTM)
- All-in-one appliance for SMBs
- Combines: Firewall + IPS + AV + URL filtering + VPN + email filtering
- Simpler to manage, but single point of failure

## 9.3 Firewall Deployment Concepts

### DMZ (Demilitarised Zone)
A **DMZ** is a semi-trusted network segment between the Internet and the internal LAN.

```
Internet ──→ [External Firewall] ──→ DMZ ──→ [Internal Firewall] ──→ LAN
                                     │
                              Web / Mail / DNS
                               (Public servers)
```

**Rules:**
- Internet → DMZ: Permitted for specific services (HTTP 80, HTTPS 443)
- DMZ → LAN: Denied by default (a compromised DMZ server shouldn't access the LAN)
- LAN → DMZ: Permitted (internal users access DMZ resources)

### Single vs Dual Firewall DMZ

| Setup | Description | Security Level |
|---|---|---|
| **Single firewall** | 3-interface firewall (outside/DMZ/inside) | Moderate |
| **Dual firewall** | Separate firewalls for outside→DMZ and DMZ→inside | High |

### Transparent Firewall (L2 Mode)
- Acts as a Layer 2 bridge — invisible to routing
- No IP address needed on interfaces
- Useful when you can't change the IP addressing scheme

---

# Module 10: Zone-Based Policy Firewalls

## 10.1 Overview

**Zone-Based Policy Firewall (ZPF)** is an IOS feature that turns a Cisco router into a full stateful firewall.

**Advantages over classic IOS firewall (CBAC):**
- More intuitive policy model
- Traffic controlled between **zones**, not just interfaces
- One interface can only belong to **one zone**
- **Self zone** represents the router itself

## 10.2 ZPF Concepts

**Zone:** A logical grouping of interfaces with the same trust level.

**Zone-pair:** Defines traffic direction between two zones. A zone-pair is **unidirectional** — you need two zone-pairs for bidirectional traffic (unless using `inspect`, which creates return traffic automatically).

**Policy flow:**
1. Traffic arrives on interface → look up zone membership
2. Find the zone-pair for source zone → destination zone
3. Apply the service policy (class-map → policy-map)

**Actions:**
| Action | Description |
|---|---|
| **Inspect** | Stateful tracking — automatically permits return traffic |
| **Pass** | Permit traffic (no state tracking) |
| **Drop** | Deny traffic (default for zone-pairs with no policy) |
| **Log** | Log matched traffic |

## 10.3 ZPF Configuration Steps

```
! Step 1: Create zones
zone security INSIDE
zone security OUTSIDE
zone security DMZ

! Step 2: Assign interfaces to zones
interface GigabitEthernet0/0
 zone-member security INSIDE

interface GigabitEthernet0/1
 zone-member security OUTSIDE

interface GigabitEthernet0/2
 zone-member security DMZ

! Step 3: Define class maps (match interesting traffic)
class-map type inspect match-any INSIDE-ALLOWED
 match protocol http
 match protocol https
 match protocol dns
 match protocol icmp

class-map type inspect match-any DMZ-WEB
 match protocol http
 match protocol https

! Step 4: Define policy maps (what to do with matched traffic)
policy-map type inspect INSIDE-TO-OUTSIDE-POLICY
 class type inspect INSIDE-ALLOWED
  inspect
 class class-default
  drop log

policy-map type inspect OUTSIDE-TO-DMZ-POLICY
 class type inspect DMZ-WEB
  inspect
 class class-default
  drop log

! Step 5: Create zone-pairs and apply policies
zone-pair security INSIDE-TO-OUTSIDE source INSIDE destination OUTSIDE
 service-policy type inspect INSIDE-TO-OUTSIDE-POLICY

zone-pair security OUTSIDE-TO-DMZ source OUTSIDE destination DMZ
 service-policy type inspect OUTSIDE-TO-DMZ-POLICY
```

## 10.4 The Self Zone

Traffic **to or from the router itself** involves the **self zone**.

```
! Permit SSH from management network to router
class-map type inspect match-any MGMT-TRAFFIC
 match protocol ssh

policy-map type inspect INSIDE-TO-SELF
 class type inspect MGMT-TRAFFIC
  inspect
 class class-default
  drop

zone-pair security INSIDE-TO-SELF source INSIDE destination self
 service-policy type inspect INSIDE-TO-SELF
```

## 10.5 Verifying ZPF

```
show zone security
show zone-pair security
show policy-map type inspect zone-pair
show class-map type inspect
```

---

# Module 11: IPS Technologies

## 11.1 IDS vs IPS

| Feature | IDS | IPS |
|---|---|---|
| **Deployment** | Out-of-band (passive, copy of traffic) | Inline (traffic passes through) |
| **Response** | Alerts only — cannot stop attack | Blocks/drops malicious traffic |
| **Impact on traffic** | None | Adds some latency |
| **False positive consequence** | Alert flood | Blocks legitimate traffic |
| **False negative consequence** | Missed alert | Attack succeeds undetected |

> 💡 **IDS is a burglar alarm; IPS is a security guard.**  
> Both use signatures to detect patterns.

## 11.2 Detection Methods

### Signature-Based Detection
- Compares traffic against a database of known attack signatures
- **Pros:** Low false positives, fast, reliable for known attacks
- **Cons:** Blind to zero-days; requires constant signature updates
- Like antivirus — only knows what it's been told to look for

### Anomaly-Based (Behaviour-Based) Detection
- Profiles **normal baseline** behaviour
- Alerts when traffic deviates significantly from baseline
- **Pros:** Can detect novel attacks and zero-days
- **Cons:** Higher false positive rate; requires time to build a good baseline
- Can detect things like: unusual port usage, traffic spikes, new protocols

### Policy-Based Detection
- Alerts when defined security policy is violated
- Example: "Alert when any Telnet traffic is seen on the network"
- Simpler but less flexible than anomaly-based

## 11.3 IPS Signatures

A **signature** is a set of rules describing a known attack pattern.

**Signature components:**
- **Type:** Atomic (single packet) or Composite (multi-packet sequence)
- **Trigger:** Condition that activates the signature
- **Action:** Alert, drop, reset TCP, block attacker IP

**Signature Actions:**
| Action | Description |
|---|---|
| **Alert** | Generate log entry / alert |
| **Drop** | Silently discard the packet |
| **Deny attacker** | Block all future traffic from the source IP |
| **Reset TCP** | Send RST to both parties to terminate the session |
| **Log** | Log the packet for forensics |

## 11.4 False Positives and False Negatives

| Result | Meaning | Consequence |
|---|---|---|
| **True Positive** | Real attack — correctly detected | Desired outcome |
| **True Negative** | Legitimate traffic — correctly allowed | Desired outcome |
| **False Positive** | Legitimate traffic — incorrectly flagged as attack | Wasted analyst time; may block legitimate users |
| **False Negative** | Real attack — not detected | Attack succeeds undetected |

> 💡 The goal is to maximise True Positives and True Negatives while minimising both types of errors.

## 11.5 NIPS vs HIPS

| Type | Deployment | Monitors |
|---|---|---|
| **NIPS (Network-Based IPS)** | Network chokepoint | All network traffic |
| **HIPS (Host-Based IPS)** | Agent on individual host | System calls, files, processes, registry |

**HIPS advantages:**
- Can see encrypted traffic (after decryption on host)
- Can monitor local system behaviour (file modifications, process injection)

**NIPS advantages:**
- Single sensor covers many hosts
- No agent installation required

---

# Module 12: IPS Operation and Implementation

## 12.1 Cisco IPS Implementation Options

| Platform | Description |
|---|---|
| **Cisco IOS IPS** | Built-in IPS feature in IOS routers (basic) |
| **Cisco Firepower** | Dedicated NGIPS appliance or module (recommended) |
| **Firepower Module in ASA** | AIP-SSM / AIP-SSC module adds IPS to ASA |
| **Firepower Management Centre (FMC)** | Centralised management for multiple Firepower sensors |

## 12.2 IPS Sensor Placement

**Key principle:** Place sensors where they can see the most relevant traffic.

**Recommended locations:**
- Outside the perimeter firewall → sees all external threats
- Inside the perimeter firewall → sees threats that bypassed the firewall
- In front of critical servers in the DMZ
- On internal segments containing sensitive data

## 12.3 Cisco IOS IPS Configuration

```
! Step 1: Create IPS rules directory
mkdir flash:/ipsdir

! Step 2: Define IPS config location
ip ips config location flash:/ipsdir

! Step 3: Create IPS rule (name it)
ip ips name IOSIPS

! Step 4: Enable IPS on an interface
interface GigabitEthernet0/1
 ip ips IOSIPS in

! Step 5: Load signatures (from Cisco SigUpgrade)
ip ips signature-category
 category all
  retired true         ! Retire all sigs first
 category ios_ips basic
  retired false        ! Enable only the basic category

! Verify
show ip ips all
show ip ips signature count
show ip ips statistics
```

## 12.4 Tuning IPS

**Why tuning is essential:**
- Out-of-box signatures generate many false positives
- Not all signatures are relevant to your environment
- An untuned IPS creates alert fatigue → analysts ignore alerts

**Tuning steps:**
1. **Retire** signatures not relevant to your OS/applications
2. **Enable** only categories relevant to your environment
3. **Adjust severity** of low-risk signatures
4. **Add exclusions** for known-good traffic patterns
5. **Monitor** and iterate

## 12.5 Cisco Firepower

**Cisco Firepower** is Cisco's modern NGIPS/NGFW platform.

**Features:**
- Powered by the **Snort** detection engine
- Application visibility and control (AVC)
- URL filtering
- Advanced Malware Protection (AMP) integration
- File trajectory — track where a file went across the network
- Network behaviour analysis
- Managed by **FMC (Firepower Management Centre)** or **FDM (Firepower Device Manager)**

---

# Module 13: Endpoint Security

## 13.1 Why Endpoint Security?

The traditional network perimeter has dissolved. Users work from home, coffee shops, and on personal devices. **Endpoints are the new perimeter.**

An endpoint can be:
- Laptop / Desktop
- Smartphone / Tablet
- IoT device
- Server
- Virtual machine

## 13.2 Endpoint Protection Tools

### Antivirus / Anti-malware
- Signature + heuristic + behaviour-based detection
- Must be kept updated — new signatures released daily
- Not sufficient alone — only catches known threats

### Host-Based Firewall
- Filters traffic at the individual host
- Complements network firewall
- Examples: Windows Defender Firewall, iptables (Linux)

### HIPS (Host Intrusion Prevention System)
- Monitors system calls, file system, registry, network connections
- Blocks suspicious activity in real time

### EDR (Endpoint Detection and Response)
- Advanced evolution of HIPS
- Continuous monitoring with cloud-based intelligence
- Forensic capabilities — full activity timeline
- Examples: Cisco Secure Endpoint (formerly AMP), CrowdStrike, Carbon Black

### DLP (Data Loss Prevention)
- Prevents unauthorised transfer of sensitive data
- Monitors: email, USB, web uploads, printing, cloud sync
- Uses content inspection: regex patterns, document fingerprinting, ML

## 13.3 OS Hardening

**Hardening** reduces the attack surface by:
- Removing unnecessary software and features
- Applying all security patches promptly
- Disabling unused services (e.g., SMBv1, Remote Registry)
- Enabling the host-based firewall
- Using strong account policies (password complexity, lockout)
- Enabling audit logging
- Implementing application whitelisting (only approved software runs)

## 13.4 Patch Management

- Keep firmware, OS, and applications up to date
- **Critical patches** (RCE) should be applied within **days**
- Test in lab before production deployment
- Maintain asset inventory — you can't patch what you don't know about
- Use centralised tools: WSUS, SCCM, Ansible, Puppet

## 13.5 Mobile Device Security (MDM)

**MDM (Mobile Device Management)** tools manage and secure mobile devices.

Controls include:
- Enforce PIN, password, or biometric lock
- Enable **remote wipe** on lost/stolen devices
- Enforce **device encryption**
- Control which apps can be installed
- Enforce **VPN** for corporate access
- Separate work and personal profiles (**containerisation**)

Examples: Cisco Meraki MDM, Microsoft Intune, VMware Workspace ONE

## 13.6 BYOD (Bring Your Own Device)

BYOD policies must balance employee privacy vs corporate security.

Best practices:
- Register all BYOD devices with MDM before granting access
- Apply minimum security requirements (OS version, patching, AV)
- Use **containerisation** — work data isolated from personal data
- Restrict access to sensitive systems from unmanaged devices
- Define clear acceptable use policy

---

# Module 14: Layer 2 Security Considerations

## 14.1 Why Layer 2 Security Matters

Layer 2 attacks target the **Data Link layer** — switches and the protocols they rely on. If Layer 2 is compromised, all higher layers are at risk.

> 💡 "Security is only as strong as its weakest link — and Layer 2 is often that link."

## 14.2 VLAN Attacks

### VLAN Hopping
**Goal:** Access VLANs the attacker is not authorised to reach.

**Method 1 – Switch Spoofing:**
- Attacker configures their NIC to negotiate a **trunk link** (using DTP – Dynamic Trunking Protocol)
- Once trunked, attacker can send/receive traffic for any VLAN

**Method 2 – Double Tagging:**
- Attacker sends a double-tagged 802.1Q frame
- First tag matches native VLAN → stripped by first switch
- Second tag targets the victim VLAN → forwarded by second switch
- Note: Only works **one-way** (responses don't return)

**Mitigations:**
```
! Disable DTP on all access ports
interface GigabitEthernet0/1
 switchport mode access
 switchport nonegotiate

! Set native VLAN to an unused VLAN ID (not VLAN 1)
interface GigabitEthernet0/24
 switchport trunk native vlan 999

! Explicitly tag native VLAN on trunk (Cisco proprietary)
vlan dot1q tag native
```

## 14.3 STP (Spanning Tree Protocol) Attacks

**Attack:** Attacker sends superior BPDUs (Bridge Protocol Data Units) with a lower Bridge ID to become the **root bridge**. This forces all traffic to route through the attacker → MitM attack.

**Mitigations:**

```
! PortFast – skip STP states on access ports (instant forwarding)
interface GigabitEthernet0/1
 spanning-tree portfast

! BPDU Guard – if a BPDU is received on a PortFast port, shut it down
interface GigabitEthernet0/1
 spanning-tree bpduguard enable

! BPDU Filter – silently discard BPDUs (use with caution)
interface GigabitEthernet0/1
 spanning-tree bpdufilter enable

! Root Guard – prevents a port from becoming root port
interface GigabitEthernet0/24
 spanning-tree guard root
```

## 14.4 DHCP Attacks

### DHCP Starvation
- Attacker sends many DHCP Discover messages with **spoofed MAC addresses**
- Exhausts the DHCP pool → legitimate hosts can't get IP addresses
- DoS attack

### DHCP Spoofing (Rogue DHCP Server)
- Attacker runs their own DHCP server
- Assigns attacker-controlled **default gateway** and **DNS** → MitM attack
- Clients unknowingly send all traffic through attacker

**Mitigation: DHCP Snooping**
```
! Enable globally and on VLANs
ip dhcp snooping
ip dhcp snooping vlan 10,20

! Mark uplinks as trusted (all others are untrusted by default)
interface GigabitEthernet0/24
 ip dhcp snooping trust

! Verify binding table
show ip dhcp snooping binding
```

> 💡 DHCP Snooping creates a **binding table** (MAC → IP → port → VLAN) that is also used by DAI.

## 14.5 ARP Attacks

### ARP Spoofing / ARP Poisoning
- ARP has **no authentication** — anyone can send a Gratuitous ARP
- Attacker sends fake ARP replies: "The IP 192.168.1.1 is at MY MAC"
- Victim updates their ARP cache → traffic redirected to attacker
- Enables: **MitM, session hijacking, eavesdropping**

**Mitigation: Dynamic ARP Inspection (DAI)**
```
! Enable DAI on VLANs
ip arp inspection vlan 10,20

! Mark trusted interfaces (uplinks only)
interface GigabitEthernet0/24
 ip arp inspection trust

! Verify
show ip arp inspection
show ip arp inspection statistics
```

> 💡 DAI uses the **DHCP Snooping binding table** to validate ARP packets. DHCP Snooping must be enabled first.

## 14.6 MAC Address Table Attacks

### MAC Flooding
- Attacker floods switch with frames using **random fake source MACs**
- Switch MAC address table fills up (CAM table overflow)
- Switch fails open → broadcasts all frames to all ports (hub behaviour)
- Attacker can sniff all traffic

**Mitigation: Port Security**
```
interface GigabitEthernet0/1
 switchport mode access
 switchport port-security
 switchport port-security maximum 2          ! Max 2 MACs on this port
 switchport port-security mac-address sticky ! Learn and save current MACs
 switchport port-security violation restrict  ! Log violation, don't shut port

! Violation modes:
! protect  – drop frames from unknown MACs, no log
! restrict – drop frames, log, increment violation counter
! shutdown – (default) err-disable the port

show port-security
show port-security interface GigabitEthernet0/1
```

---

# Module 15: Cryptographic Services

## 15.1 Why Cryptography?

Cryptography protects:
- **Confidentiality** – only intended recipients can read the data
- **Integrity** – detect unauthorised modification
- **Authentication** – verify identity
- **Non-repudiation** – sender cannot deny sending

## 15.2 Cryptographic Terms

| Term | Definition |
|---|---|
| **Plaintext** | Original readable data |
| **Ciphertext** | Encrypted, unreadable data |
| **Encryption** | Converting plaintext → ciphertext |
| **Decryption** | Converting ciphertext → plaintext |
| **Key** | Secret value used in the cipher |
| **Algorithm / Cipher** | Mathematical function performing encryption |
| **Keyspace** | The set of all possible values used to generate a key |

> 💡 The **keyspace** determines how resistant an algorithm is to brute force. A 256-bit key has 2^256 possible values.

## 15.3 Symmetric Encryption

**Same key** used for both encryption and decryption.

**Advantages:** Very fast, suitable for large amounts of data
**Disadvantage:** Key distribution problem — how do you securely share the key?

| Algorithm | Key Size | Notes |
|---|---|---|
| **DES** | 56-bit | Deprecated — broken in 1999 |
| **3DES** | 112/168-bit | Three rounds of DES — legacy, being phased out |
| **AES** | 128/192/256-bit | **Current standard** — recommended |
| **RC4** | Variable | Stream cipher — deprecated (used in WEP, old SSL) |
| **ChaCha20** | 256-bit | Modern stream cipher, used in TLS 1.3 |

### Block Cipher Modes

| Mode | Notes |
|---|---|
| **ECB (Electronic Codebook)** | Same plaintext block → same ciphertext. Insecure — reveals patterns |
| **CBC (Cipher Block Chaining)** | XORs each block with previous ciphertext. Uses IV. Was the standard |
| **CTR (Counter)** | Converts block cipher to stream cipher. Parallelisable |
| **GCM (Galois/Counter Mode)** | CTR + authentication tag. Provides encryption AND integrity. Recommended |

> 💡 **AES-256-GCM** is the gold standard for symmetric encryption in modern protocols (TLS 1.3, IPsec).

## 15.4 Asymmetric Encryption

A **key pair**: **public key** (shared with everyone) + **private key** (kept secret).

- What is encrypted with the **public key** can only be decrypted with the **private key**
- What is signed with the **private key** can be verified with the **public key**
- Much **slower** than symmetric — used only for key exchange and signatures

| Algorithm | Use Case | Notes |
|---|---|---|
| **RSA** | Encryption, key exchange, digital signatures | Most common; use 2048+ bit keys |
| **Diffie-Hellman (DH)** | Key exchange only | Cannot encrypt or sign; establishes shared secret |
| **ECDH** | Elliptic curve key exchange | Smaller keys, same security as DH |
| **ECDSA** | Elliptic curve digital signatures | Smaller, faster than RSA |
| **DSA** | Digital signatures only | Older standard |

## 15.5 Hybrid Encryption

Real-world protocols use **both** symmetric and asymmetric:

1. **Asymmetric** → securely exchange a temporary **symmetric session key**
2. **Symmetric** → encrypt the actual data (fast)

This is exactly how **TLS/SSL, IPsec, and SSH** work.

---

# Module 16: Basic Integrity and Authentication

## 16.1 Hashing

A **hash function** produces a fixed-size output (digest) from any input.

**Properties:**
- **One-way** — computationally infeasible to reverse
- **Deterministic** — same input always gives same output
- **Avalanche effect** — tiny input change → completely different hash
- **Collision resistance** — very hard to find two inputs with the same hash

**Uses:** Integrity verification, password storage, digital signatures

| Algorithm | Output Size | Status |
|---|---|---|
| **MD5** | 128-bit (32 hex chars) | Deprecated — collision-prone |
| **SHA-1** | 160-bit | Deprecated — collisions demonstrated |
| **SHA-256** | 256-bit | Current standard |
| **SHA-384** | 384-bit | Strong |
| **SHA-512** | 512-bit | Very strong |
| **SHA-3** | Variable | Modern design, not SHA-2 based |

> 💡 For **password storage**, use bcrypt, Argon2, or PBKDF2 — they include a **salt** and are deliberately slow to resist brute force.

## 16.2 Salting Passwords

A **salt** is a random value appended to a password before hashing.

**Why salting matters:**
- Without salt: all users with password "password" have the same hash
- Attacker can use **rainbow tables** (pre-computed hash lookups) to crack quickly
- With salt: `hash("password" + "xK9$mN2p")` — unique hash per user
- Rainbow tables are useless against salted hashes

## 16.3 HMAC (Hash-based Message Authentication Code)

`HMAC = Hash(message + secret_key)`

**HMAC provides:**
- **Integrity** – detects modification
- **Authentication** – only someone with the secret key could produce this HMAC

HMAC does NOT provide encryption or non-repudiation.

Used in: IPsec, TLS, SSH, JWT tokens.

```
HMAC-SHA256(message, secret_key) → authentication tag
```

## 16.4 Digital Signatures

**Process:**
1. Sender hashes the message → **message digest**
2. Sender encrypts the digest with their **private key** → **digital signature**
3. Recipient decrypts signature with sender's **public key** → recovers digest
4. Recipient independently hashes the message
5. Compare the two digests — if they match, signature is valid

**Digital signatures provide:**
- **Integrity** – any change invalidates the signature
- **Authentication** – only the private key owner could sign it
- **Non-repudiation** – sender cannot deny signing

**Does NOT provide confidentiality** (the message itself is not encrypted).

## 16.5 Code Signing

- Software vendors **digitally sign** executables and updates
- Operating systems verify the signature before execution
- Ensures the software has not been tampered with since publication
- Protects against: supply chain attacks, malware masquerading as legitimate software

---

# Module 17: Public Key Cryptography

## 17.1 Public Key Infrastructure (PKI)

**PKI** is the framework for managing digital certificates and public keys.

### PKI Components

| Component | Role |
|---|---|
| **CA (Certificate Authority)** | Issues and digitally signs certificates |
| **RA (Registration Authority)** | Validates identity before CA issues certificate |
| **Certificate** | Binds a public key to an entity's identity (X.509 standard) |
| **CRL (Certificate Revocation List)** | List of revoked certificates published by CA |
| **OCSP** | Online Certificate Status Protocol — real-time revocation check |
| **Repository** | Directory where certificates and CRLs are published (LDAP) |

### X.509 Certificate Contents
- **Subject** – entity name (CN, O, OU, C)
- **Subject's public key** – the key being certified
- **Issuer** – CA name
- **Validity period** – Not Before / Not After
- **Serial number** – unique identifier
- **Signature algorithm** – e.g., SHA256withRSA
- **CA's digital signature** – proves the CA vouches for this cert

## 17.2 Certificate Trust Chain

```
Root CA (self-signed — highest trust, stored in browser/OS trust store)
    └── Intermediate CA (signed by Root CA)
           └── End-entity certificate (signed by Intermediate CA)
                  e.g., www.example.com
```

**Why intermediate CAs?**
- Root CA is kept **offline** for security
- Intermediate CA does the day-to-day signing
- Compromise of Intermediate CA doesn't require replacing the Root CA

## 17.3 Self-Signed vs CA-Signed Certificates

| Type | Trust | Use Case |
|---|---|---|
| **Self-signed** | No external trust; triggers browser warnings | Lab, internal testing |
| **CA-signed (Internal CA)** | Trusted by devices in the organisation | Corporate internal services |
| **CA-signed (Public CA)** | Trusted by everyone (browser trust store) | Public-facing websites |

## 17.4 Diffie-Hellman Key Exchange

**DH** allows two parties to establish a **shared secret over an insecure channel** without the secret ever being transmitted.

**How it works (simplified):**
1. Alice and Bob agree on public parameters: large prime **p** and generator **g**
2. Alice picks private `a`; Bob picks private `b`
3. Alice sends `g^a mod p`; Bob sends `g^b mod p` (exchanged publicly)
4. Alice computes `(g^b)^a mod p`; Bob computes `(g^a)^b mod p`
5. Both arrive at the same shared secret `g^(ab) mod p`
6. Eavesdropper can see `g^a` and `g^b` but cannot compute `g^(ab)` — this is the **Discrete Logarithm Problem**

**DH Groups:**

| Group | Algorithm | Strength | Status |
|---|---|---|---|
| Group 1/2/5 | 768/1024/1536-bit DH | Weak | Deprecated |
| **Group 14** | 2048-bit DH | Acceptable | Minimum for production |
| **Group 19** | 256-bit ECDH | Strong | Recommended |
| **Group 20** | 384-bit ECDH | Very Strong | Recommended |
| **Group 21** | 521-bit ECDH | Very Strong | Recommended |

> 💡 **ECDH (Elliptic Curve DH)** provides equivalent security to DH with much smaller key sizes, which is faster and more efficient.

## 17.5 PKI Enrolment Process

1. Entity generates a **key pair**
2. Creates a **CSR (Certificate Signing Request)** containing: public key + identity info
3. Submits CSR to **RA/CA**
4. CA validates identity (out-of-band)
5. CA signs the certificate and returns it
6. Entity installs certificate

---

# Module 18: VPNs

## 18.1 VPN Overview

A **VPN (Virtual Private Network)** creates an encrypted, authenticated tunnel over an untrusted network (the Internet).

**Security services provided:**
- **Confidentiality** – encryption prevents eavesdropping
- **Integrity** – hashing detects tampering
- **Authentication** – peers verify each other's identity
- **Anti-replay** – sequence numbers prevent replay attacks

**Cost benefit:** VPNs replace expensive leased lines by using the Internet.

## 18.2 VPN Types

### Site-to-Site VPN
- Connects two entire networks (branch office → HQ)
- Configured on routers or firewalls
- **Always-on** — transparent to end users
- Users don't need VPN client software

### Remote Access VPN
- Individual user connects to corporate network
- User must **initiate** the connection with VPN client software
- Two main types: **IPsec VPN** and **SSL VPN**

### SSL VPN (Cisco AnyConnect)
- Uses **TLS** (TCP 443) or **DTLS** (UDP 443)
- Works through almost any firewall/NAT without special configuration
- Two modes:
  - **Clientless SSL VPN:** Web browser only (limited access to web-based apps)
  - **Full tunnel SSL VPN:** AnyConnect client, full network access

### MPLS VPN
- Service provider managed Layer 3 VPN
- Uses MPLS labels to separate customer traffic
- **Not encrypted by default** — relies on trusted provider infrastructure
- Provides QoS guarantees (unlike Internet-based VPN)

## 18.3 IPsec Framework

**IPsec** is a suite of protocols providing security at the **IP layer (Layer 3)**.

**Four security functions of IPsec:**

| Function | Options |
|---|---|
| **Confidentiality** | DES (deprecated), 3DES (legacy), **AES-128/256** (recommended) |
| **Integrity** | MD5 (deprecated), SHA-1 (deprecated), **SHA-256/384/512** (recommended) |
| **Authentication** | Pre-Shared Key (PSK), RSA digital signatures, certificates |
| **Key Exchange** | DH Groups (14, 19, 20 recommended) |

### IPsec Protocols

**AH (Authentication Header) – Protocol 51**
- Provides: Integrity + Authentication + Anti-replay
- Does **NOT** encrypt (no confidentiality)
- Authenticates the entire IP packet including the outer header
- **Incompatible with NAT** (NAT changes IP header, breaks authentication)
- Rarely used in practice

**ESP (Encapsulating Security Payload) – Protocol 50**
- Provides: **Confidentiality + Integrity + Authentication + Anti-replay**
- Encrypts the payload
- NAT-compatible (with **NAT-T** using UDP 4500)
- **Almost always preferred over AH**

### IPsec Modes

| Mode | What is Protected | Used For |
|---|---|---|
| **Transport Mode** | Payload only — original IP header unchanged | Host-to-host |
| **Tunnel Mode** | Entire original packet encapsulated in new IP packet | **Site-to-site VPN** (gateway-to-gateway) |

```
Transport Mode:  [IP Hdr | ESP Hdr | Original Payload | ESP Auth]
Tunnel Mode:     [New IP Hdr | ESP Hdr | Original IP Hdr | Payload | ESP Auth]
```

---

# Module 19: Implement Site-to-Site IPsec VPNs

## 19.1 IKE – Internet Key Exchange

**IKE** negotiates the security associations (SAs) used by IPsec.

### IKEv1 – Two Phases

**Phase 1 (ISAKMP SA) — Establish Secure Channel**

Negotiates: encryption, hash, authentication method, DH group, lifetime.

Two modes:
- **Main Mode** (6 messages) — Protects identity of both peers (more secure)
- **Aggressive Mode** (3 messages) — Faster but identity not protected (legacy use)

**Phase 2 (IPsec SA) — Negotiate Actual IPsec Parameters**

- Uses the Phase 1 secure channel
- Only mode: **Quick Mode** (3 messages)
- Negotiates: transform set (ESP algorithms)
- Creates **two unidirectional SAs** (one for each direction of traffic)
- Optional: **PFS (Perfect Forward Secrecy)** — new DH exchange for each SA

### IKEv2 (Recommended)
- More efficient — fewer messages (4 instead of 9 for IKEv1 main mode)
- Built-in **NAT traversal**
- Better DoS resistance
- Supports **EAP** for remote access authentication
- Supports **MOBIKE** — handles IP address changes (mobile users)

## 19.2 Full IPsec Site-to-Site Configuration

```
! ─────────────────────────────────────────
! Step 1: ISAKMP Policy (Phase 1 / IKE SA)
! ─────────────────────────────────────────
crypto isakmp policy 10
 encryption aes 256
 hash sha256
 authentication pre-share
 group 14
 lifetime 86400              ! 24 hours

! Pre-shared key for the remote peer
crypto isakmp key VPNSecret@Key123 address 203.0.113.2

! ─────────────────────────────────────────
! Step 2: IPsec Transform Set (Phase 2)
! ─────────────────────────────────────────
crypto ipsec transform-set TRANSFORM-AES esp-aes 256 esp-sha256-hmac
 mode tunnel                 ! Tunnel mode (default for site-to-site)

! ─────────────────────────────────────────
! Step 3: ACL - Define "Interesting Traffic"
! ─────────────────────────────────────────
ip access-list extended VPN-INTERESTING-TRAFFIC
 permit ip 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255

! ─────────────────────────────────────────
! Step 4: Crypto Map
! ─────────────────────────────────────────
crypto map VPN-MAP 10 ipsec-isakmp
 set peer 203.0.113.2
 set transform-set TRANSFORM-AES
 set pfs group14              ! Perfect Forward Secrecy
 match address VPN-INTERESTING-TRAFFIC

! ─────────────────────────────────────────
! Step 5: Apply to Outside Interface
! ─────────────────────────────────────────
interface GigabitEthernet0/1
 crypto map VPN-MAP
```

## 19.3 Verify IPsec VPN

```
show crypto isakmp sa           ! Phase 1 SA status (should show QM_IDLE)
show crypto ipsec sa            ! Phase 2 SA status (packets encrypted/decrypted)
show crypto map                 ! Crypto map configuration
debug crypto isakmp             ! Troubleshoot Phase 1 issues
debug crypto ipsec              ! Troubleshoot Phase 2 issues
```

**Common Issues:**
- Mismatched ISAKMP policy parameters → Phase 1 fails
- Mismatched transform sets → Phase 2 fails
- ACL not matching interesting traffic → tunnel comes up but no traffic passes
- NAT translating IPsec traffic before encryption → enable NAT exemption

## 19.4 NAT Exemption (Avoid NAT on VPN Traffic)

```
! Exclude VPN traffic from NAT
ip access-list extended NAT-EXEMPTION
 deny ip 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255   ! VPN traffic - no NAT
 permit ip 192.168.1.0 0.0.0.255 any                  ! Other traffic - NAT normally

ip nat inside source list NAT-EXEMPTION interface GigabitEthernet0/1 overload
```

---

# Module 20: Introduction to the ASA

## 20.1 ASA Overview

**Cisco ASA (Adaptive Security Appliance)** is Cisco's dedicated stateful firewall platform. It combines:
- Stateful inspection firewall
- VPN concentrator (IPsec, SSL/AnyConnect)
- NAT/PAT
- Optional IPS module (AIP-SSM)
- Optional Antimalware module (CSC-SSM)

**ASA Hardware Modules:**
| Module | Function |
|---|---|
| **AIP-SSM / AIP-SSC** | Advanced IPS capabilities (tens of thousands of exploit signatures) |
| **CSC module** | Content Security and Control — antimalware capabilities |

## 20.2 ASA Security Levels

Every ASA interface has a **security level 0–100**.

| Level | Direction | Default Rule |
|---|---|---|
| **Higher → Lower** | Outbound (trusted → untrusted) | Permitted by default |
| **Lower → Higher** | Inbound (untrusted → trusted) | Denied by default |
| **Equal levels** | Between equal-trust zones | Denied by default |

**Defaults:**
- **Inside = 100** (most trusted)
- **Outside = 0** (least trusted)
- **DMZ = 50** (semi-trusted)

```
interface GigabitEthernet0/0
 nameif outside
 security-level 0
 ip address 203.0.113.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 nameif inside
 security-level 100
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/2
 nameif dmz
 security-level 50
 ip address 172.16.1.1 255.255.255.0
 no shutdown
```

## 20.3 ASA vs IOS Router Firewall

| Feature | Cisco ASA | IOS ZPF |
|---|---|---|
| **Primary purpose** | Dedicated firewall | Router + firewall |
| **Security levels** | Yes (0–100) | No (zone-based model) |
| **Performance** | Dedicated hardware | Shared with routing |
| **VPN** | Extensive (IPsec + AnyConnect) | Moderate |
| **Management** | ASDM (GUI) or CLI | CLI only |

## 20.4 ASA ASDM (GUI Management)

**ASDM (Adaptive Security Device Manager)** is a Java-based web GUI for managing ASA.

```
! Enable ASDM access
http server enable
http 192.168.1.0 255.255.255.0 inside   ! Permit ASDM from inside network
username admin password Admin@P@ss privilege 15
aaa authentication http console LOCAL
```

---

# Module 21: ASA Firewall Configuration

## 21.1 ASA Access Control Lists

**ASA ACLs differ from IOS ACLs:**
- ASA uses **named ACLs only**
- Applied with `access-group` command
- By default, higher-security → lower-security is permitted; all other traffic is denied
- ASA ACLs use **subnet masks** (not wildcard masks) — opposite of IOS!

**Standard ACLs on ASA:** Match destination IP only (used for route maps, not interface ACLs)
**Extended ACLs on ASA:** Match source and destination — most common

```
! Allow HTTP from outside to DMZ web server
access-list OUTSIDE-IN extended permit tcp any host 172.16.1.10 eq 80
access-list OUTSIDE-IN extended permit tcp any host 172.16.1.10 eq 443
access-list OUTSIDE-IN extended deny ip any any log

! Apply to interface
access-group OUTSIDE-IN in interface outside
```

## 21.2 ASA Object Groups

**Object groups** simplify ACLs by grouping related items.

```
! Network object group
object-group network WEB-SERVERS
 network-object host 172.16.1.10
 network-object host 172.16.1.11

! Service object group
object-group service WEB-PORTS tcp
 port-object eq 80
 port-object eq 443
 port-object eq 8080

! Use in ACL
access-list OUTSIDE-IN extended permit tcp any object-group WEB-SERVERS object-group WEB-PORTS
```

## 21.3 ASA NAT

**Two types of NAT on ASA:**

### Object NAT (Auto NAT)
Defined within a **network object** — simpler, good for common cases.

```
! Dynamic PAT — translate inside hosts to outside interface IP
object network INSIDE-NETWORK
 subnet 192.168.1.0 255.255.255.0
 nat (inside,outside) dynamic interface

! Static NAT — fixed translation for DMZ server
object network DMZ-WEB
 host 172.16.1.10
 nat (dmz,outside) static 203.0.113.10
```

### Twice NAT (Manual NAT)
Defined separately from objects — translates both source and destination. Used for complex scenarios.

```
nat (inside,outside) source dynamic INSIDE-NETWORK interface
```

## 21.4 ASA VPN (AnyConnect SSL VPN)

```
webvpn
 enable outside
 anyconnect image disk0:/anyconnect-win-4.10.pkg 1
 anyconnect enable
 tunnel-group-list enable

! IP address pool for VPN clients
ip local pool VPN-POOL 10.10.10.1-10.10.10.50 mask 255.255.255.0

! Group policy
group-policy ANYCONNECT-POLICY internal
group-policy ANYCONNECT-POLICY attributes
 vpn-tunnel-protocol ssl-client
 dns-server value 192.168.1.10
 default-domain value company.com

! Tunnel group (connection profile)
tunnel-group REMOTE-VPN type remote-access
tunnel-group REMOTE-VPN general-attributes
 address-pool VPN-POOL
 default-group-policy ANYCONNECT-POLICY
tunnel-group REMOTE-VPN webvpn-attributes
 group-alias "Corporate VPN" enable
```

## 21.5 ASA Site-to-Site IPsec VPN

```
! Phase 1
crypto ikev1 policy 10
 authentication pre-share
 encryption aes-256
 hash sha
 group 14
 lifetime 86400

! Pre-shared key
tunnel-group 203.0.113.2 type ipsec-l2l
tunnel-group 203.0.113.2 ipsec-attributes
 ikev1 pre-shared-key VPNSecret@123

! Phase 2 transform set
crypto ipsec ikev1 transform-set TS-AES esp-aes-256 esp-sha-hmac

! ACL for interesting traffic
access-list VPN-ACL extended permit ip 192.168.1.0 255.255.255.0 10.0.0.0 255.255.255.0

! Crypto map
crypto map VPN-MAP 10 match address VPN-ACL
crypto map VPN-MAP 10 set peer 203.0.113.2
crypto map VPN-MAP 10 set ikev1 transform-set TS-AES
crypto map VPN-MAP interface outside
crypto ikev1 enable outside
```

---

# Module 22: Network Security Testing

## 22.1 Why Test Network Security?

Security testing validates that controls are working as intended. Finding vulnerabilities before attackers do is far cheaper than dealing with a breach.

**Types of security testing:**
- **Vulnerability assessment** — identify weaknesses (passive)
- **Penetration testing** — actively exploit weaknesses (authorised attacks)
- **Security audit** — compare configuration against policy/standards
- **Red team / Blue team** — offensive vs defensive simulation

## 22.2 Penetration Testing Phases

```
1. Planning & Reconnaissance
       ↓
2. Scanning & Enumeration  
       ↓
3. Gaining Access (Exploitation)
       ↓
4. Maintaining Access
       ↓
5. Covering Tracks
       ↓
6. Reporting
```

**Types by knowledge:**
| Type | Tester Knowledge | Description |
|---|---|---|
| **Black box** | None | Simulates external attacker |
| **White box** | Full | Tests with full network/code knowledge |
| **Grey box** | Partial | Simulates insider or authenticated user |

> 💡 **CRITICAL:** Always obtain **written authorisation** before any penetration test. Unauthorised testing is illegal regardless of intent.

## 22.3 Security Testing Tools

### Network Scanners
| Tool | Purpose |
|---|---|
| **Nmap** | Port scanning, OS detection, service enumeration |
| **Nessus** | Vulnerability scanning |
| **OpenVAS** | Open-source vulnerability scanner |
| **Metasploit** | Exploitation framework |
| **Wireshark** | Packet capture and analysis |
| **Netcat** | Network Swiss army knife — banners, connections, file transfer |

### Cisco-Specific Tools
| Tool | Purpose |
|---|---|
| **Cisco Security Manager (CSM)** | Centralised policy management |
| **Cisco Prime Infrastructure** | Network monitoring and management |
| **Cisco Secure Network Analytics (Stealthwatch)** | Network behaviour analysis |
| **Cisco SecureX** | Security platform with threat intelligence |

## 22.4 Vulnerability Assessment vs Penetration Testing

| Feature | Vulnerability Assessment | Penetration Testing |
|---|---|---|
| **Goal** | Identify vulnerabilities | Exploit vulnerabilities |
| **Risk** | Low | Moderate (may cause outages) |
| **Depth** | Broad | Deep |
| **Authorisation** | Required | **Absolutely required** |
| **Output** | List of vulnerabilities | Exploits + evidence + business impact |

## 22.5 CVSS (Common Vulnerability Scoring System)

**CVSS** provides a standardised severity score for vulnerabilities (0.0–10.0).

| Score | Severity |
|---|---|
| 0.0 | None |
| 0.1–3.9 | Low |
| 4.0–6.9 | Medium |
| 7.0–8.9 | High |
| 9.0–10.0 | Critical |

**CVSS Factors:**
- **Attack Vector:** Network / Adjacent / Local / Physical
- **Attack Complexity:** Low / High
- **Privileges Required:** None / Low / High
- **User Interaction:** None / Required
- **Scope:** Unchanged / Changed
- **Confidentiality/Integrity/Availability Impact:** None / Low / High

## 22.6 Security Audit Process

1. **Define scope** — which systems, networks, and policies to audit
2. **Gather documentation** — network diagrams, policies, configurations
3. **Review configurations** — compare against hardening benchmarks (CIS)
4. **Analyse logs** — look for anomalies and policy violations
5. **Test controls** — verify firewalls, ACLs, IPS are functioning
6. **Report findings** — prioritise by risk; include remediation steps
7. **Remediate** — fix identified issues
8. **Verify** — confirm remediation was effective

---

# 📌 Quick-Reference Cheat Sheets

## Attack → Countermeasure

| Attack | Countermeasure |
|---|---|
| Password brute force | Account lockout, MFA, strong passwords |
| Phishing | User training, email filtering, MFA |
| ARP spoofing | Dynamic ARP Inspection (DAI) |
| DHCP starvation / spoofing | DHCP Snooping |
| VLAN hopping | Disable DTP (`nonegotiate`), unused VLAN for native |
| STP manipulation | PortFast + BPDU Guard + Root Guard |
| IP spoofing | uRPF (ip verify unicast source), BCP38 |
| MAC flooding | Port Security |
| DoS / DDoS | Rate limiting, upstream filtering, scrubbing |
| Sniffing | Encryption (TLS/IPsec), switched networks |
| DNS poisoning | DNSSEC, trusted recursive resolvers |
| SQL injection | Input validation, parameterised queries, WAF |
| Zero-day exploit | Behaviour-based IPS, EDR, rapid patching |

## Protocol Security Summary

| Protocol | Port | Secure? | Replace With |
|---|---|---|---|
| Telnet | TCP 23 | ❌ | SSH (TCP 22) |
| FTP | TCP 20/21 | ❌ | SFTP/SCP (TCP 22) |
| HTTP | TCP 80 | ❌ | HTTPS (TCP 443) |
| SNMPv1/v2c | UDP 161 | ❌ | SNMPv3 |
| LDAP | TCP 389 | ❌ | LDAPS (TCP 636) |
| SSH v2 | TCP 22 | ✅ | — |
| HTTPS/TLS | TCP 443 | ✅ | — |
| SNMPv3 | UDP 161 | ✅ | — |
| TACACS+ | TCP 49 | ✅ | — |
| IPsec ESP | Protocol 50 | ✅ | — |

## RADIUS vs TACACS+ (Exam Favourite)

| | RADIUS | TACACS+ |
|---|---|---|
| Protocol | UDP | TCP |
| Port | 1812 / 1813 | 49 |
| Encryption | Password only | Full packet |
| AAA separation | No | Yes |
| Command authorisation | Limited | Granular |
| Best for | Network access | Device admin |

## IPsec Summary

| | Phase 1 (IKE) | Phase 2 (IPsec) |
|---|---|---|
| Creates | ISAKMP SA | IPsec SAs (×2) |
| Modes (IKEv1) | Main / Aggressive | Quick |
| Encryption | AES-256 | AES-256 |
| Hash | SHA-256+ | SHA-256+ |
| Authentication | PSK or certs | Via Phase 1 |
| DH Group | 14 / 19 / 20 | PFS group |
| Lifetime | 86400 sec (24hr) | 3600 sec (1hr) |

## Cisco IOS Security Command Reference

```
! Passwords
enable secret <pass>                     ! Privileged EXEC password (hashed)
service password-encryption              ! Encrypt all plaintext passwords
security passwords min-length 10         ! Minimum password length
login block-for 120 attempts 3 within 60 ! Anti-brute-force lockout

! SSH
crypto key generate rsa modulus 2048
ip ssh version 2
line vty 0 4
 transport input ssh
 login local
 exec-timeout 5 0

! Syslog
logging host 10.0.0.100
logging trap warnings

! DHCP Snooping
ip dhcp snooping
ip dhcp snooping vlan 10
interface Gi0/24
 ip dhcp snooping trust

! Dynamic ARP Inspection
ip arp inspection vlan 10
interface Gi0/24
 ip arp inspection trust

! Port Security
interface Gi0/1
 switchport mode access
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict

! STP Protection
interface Gi0/1
 spanning-tree portfast
 spanning-tree bpduguard enable

! ACL Verification
show ip access-lists
show ip interface Gi0/1   ! Shows applied ACLs
```

## Key Term Glossary

| Term | Definition |
|---|---|
| **CIA Triad** | Confidentiality, Integrity, Availability |
| **Attack vector** | Path used to gain unauthorised access |
| **AAA** | Authentication, Authorisation, Accounting |
| **DMZ** | Semi-trusted zone between inside and outside networks |
| **IDS** | Passive – detects and alerts only |
| **IPS** | Inline – detects and blocks |
| **False positive** | Legitimate traffic incorrectly flagged as malicious |
| **False negative** | Malicious traffic not detected |
| **NGFW** | Application-aware stateful firewall with integrated IPS |
| **PKI** | Framework for managing digital certificates |
| **CA** | Certificate Authority – signs certificates |
| **DH** | Key exchange algorithm (not encryption) |
| **HMAC** | Hash + shared key = integrity + authentication |
| **AH** | IPsec protocol: integrity only, no encryption, no NAT |
| **ESP** | IPsec protocol: encryption + integrity, NAT-compatible |
| **Tunnel mode** | IPsec encapsulates entire original IP packet (site-to-site) |
| **Transport mode** | IPsec protects payload only (host-to-host) |
| **RADIUS** | UDP, encrypts password only, combined auth+authz |
| **TACACS+** | TCP 49, encrypts all, separates AAA, device admin |
| **802.1X** | Port-based NAC using EAP between supplicant/authenticator/server |
| **BPDU Guard** | Disables port if BPDU received – protects STP |
| **DHCP Snooping** | Validates DHCP messages; builds binding table |
| **DAI** | Dynamic ARP Inspection – validates ARP using DHCP binding table |
| **Port security** | Limits MACs per port; mitigates MAC flooding |
| **Privilege level** | 0–15 access tiers in Cisco IOS (15 = full access) |
| **CLI view** | Custom set of allowed commands for a user role |
| **Self zone** | ZPF zone representing the router itself |
| **Keyspace** | All possible values for generating an encryption key |
| **Salt** | Random value added to password before hashing |
| **CVSS** | Standardised vulnerability severity scoring (0–10) |

---

*🔐 Cisco NetAcad Network Security 1.0 – All 22 Modules | Good luck with your studies!*
