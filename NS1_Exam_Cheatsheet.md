# 🔐 Network Security 1.0 — Final Exam Cheatsheet
> All modules · Key facts · Exam traps · Cisco NetAcad

---

## 🦠 Threats & Malware

### Malware Quick-Reference

| Type | Key Trait |
|------|-----------|
| **Virus** | Attaches to file; needs **user action** to spread |
| **Worm** | Self-replicates **without user action**; exploits network vulnerabilities |
| **Trojan** | Disguised as legit software; does **NOT** self-replicate |
| **Rootkit** | Hides at kernel level; conceals other malware |
| **Ransomware** | Encrypts files; demands payment |
| **Botnet/Bot** | Compromised host controlled via C2 server |
| **Spyware** | Captures keystrokes/data; sends to attacker |

> ⚠️ **Exam trap:** Virus = needs user activation. Worm = no user action needed. Worm **can** launch DDoS; virus can only DoS.

### Attack Types

| Attack | Description |
|--------|-------------|
| Recon (passive) | Sniffing, OSINT — no packets sent to target |
| Recon (active) | Ping sweep, port scan — packets sent to target |
| Brute force | Try every key/password combination |
| Dictionary | Try words from a wordlist |
| SYN flood | Half-open TCP connections; exhaust connection table |
| Smurf | ICMP to broadcast; spoofed source = victim IP |
| Buffer overflow | Overwrite memory to execute arbitrary code |
| Zero-day | Unknown/undisclosed vulnerability exploit |

> 💡 **Port scan limit:** Only **IPS** and **firewall** limit port scanning. Passwords and encryption do **NOT**.

### Social Engineering

| Technique | Method |
|-----------|--------|
| Phishing | Fake email from a trusted entity |
| Spear phishing | Personalised phishing using researched details about the target |
| Whaling | Spear phishing targeting senior executives (CEO/CFO) |
| Vishing | Voice phishing (phone) |
| Pretexting | Fabricated scenario ("I'm from IT support…") |
| Baiting | Infected USB left in a parking lot/lobby |
| Tailgating | Follow authorised person through a secure door |
| Watering hole | Infect a website known to be visited by the target |

---

## 🔑 AAA & Device Access

### TACACS+ vs RADIUS — Most-Tested Comparison

| Feature | TACACS+ | RADIUS |
|---------|---------|--------|
| Protocol | TCP | UDP |
| Port | **49** | **1812** (auth) / **1813** (acct) |
| Encryption | **Entire packet** | Password only |
| AAA separation | **Fully separated** | Auth + authz combined |
| Command authorisation | **Granular per-command** | Limited |
| Standard | Cisco proprietary | Open RFC standard |
| Best for | **Device administration** | Network access (VPN, 802.1X, Wi-Fi) |
| 802.1X / SIP support | No | **Yes** |

> 💡 **Both share:** password encryption + use of transport layer protocols (Layer 4).
> **Only TACACS+** separates auth/authz.
> **Only RADIUS** supports 802.1X and SIP.
> ⚠️ RADIUS provides **separate ports for authorization and accounting** — that's the trick answer, not "separate AAA services" (that's TACACS+).

### Privilege Levels & CLI Views

**Privilege Levels:**

| Level | Access |
|-------|--------|
| 0 | Minimal: help, exit, logout only |
| 1 | User EXEC `>` — default, read-only |
| 2–14 | Custom admin-defined levels |
| 15 | Privileged EXEC `#` — full access |

**CLI Views:**

| View Type | Description |
|-----------|-------------|
| Root view | Full access; required to create/manage other views |
| CLI view | Named set of permitted commands |
| Superview | Collection of views; inherits all commands from included views |

> 💡 **Key facts:** `aaa new-model` must be enabled before CLI views work. Enter root view with `enable view`. A CLI view **can be shared** within multiple superviews.

### SSH — Required Setup Steps

1. Set a **hostname** (not the default "Router")
2. Set `ip domain-name`
3. `crypto key generate rsa modulus 2048`
4. `ip ssh version 2`
5. Configure `login local` + `transport input ssh` on VTY lines

> ⚠️ **Exam question:** "Which three additional steps are required?" → hostname, domain-name, crypto key generate. **NOT** DNS, **NOT** pre-shared keys.

### login block-for — Read This Carefully

```
login block-for 150 attempts 4 within 90
```

- Block for **150 seconds** (first number = duration)
- If **4 attempts** fail (middle number)
- Within **90 seconds** (last number = window)

> ⚠️ People commonly confuse the numbers. `block-for` = duration of lockout. `within` = observation window. They are independent values.

---

## 📋 Access Control Lists

### Standard vs Extended

| Feature | Standard | Extended |
|---------|----------|----------|
| Numbers | 1–99, 1300–1999 | 100–199, 2000–2699 |
| Matches | **Source IP only** | Src + Dst + Protocol + Port |
| Place near | **Destination** | **Source** |
| IOS masks | Wildcard masks | Wildcard masks |
| ASA masks | **Subnet masks** | **Subnet masks** (NOT wildcard!) |

> ⚠️ **Implicit deny:** Every ACL ends with invisible `deny any any`. Unmatched traffic is always dropped.
> ⚠️ **Router-generated packets** cannot be filtered by an **outbound** ACL — they bypass it entirely.

### Wildcard Mask Quick Reference

| Wildcard | Matches | IOS Shortcut |
|----------|---------|--------------|
| `0.0.0.0` | Exact host (/32) | `host x.x.x.x` |
| `0.0.0.255` | Entire /24 network | — |
| `0.0.255.255` | Entire /16 network | — |
| `255.255.255.255` | Any address | `any` |

**Formula:** Wildcard = `255.255.255.255 − subnet mask`
e.g. /24 mask = `255.255.255.0` → wildcard = `0.0.0.255`

### ACL Exam Traps

- **To change ACL direction:** Remove the inbound association, reapply outbound — do **NOT** delete and recreate the ACL
- **`established` keyword:** Allows return TCP traffic (packets with ACK/RST bit set)
- `access-list 100 deny icmp 192.168.10.0 0.0.0.255 any echo reply` applied outbound → **NO traffic** outbound (implicit deny drops everything else)
- **VTY restriction:** Use `access-class` on lines (not `access-group`, which is for interfaces)
- **ASA standard ACLs** identify **destination** IP — opposite of IOS where standard ACLs match source. Only used for OSPF routes, **cannot** be applied to interfaces to control traffic.

### IPv6 ACLs

- Named ACLs **only** (no numbered IPv6 ACLs)
- Must **explicitly permit NDP** or IPv6 breaks entirely:
  - `permit icmp any any nd-na` (Neighbor Advertisement)
  - `permit icmp any any nd-ns` (Neighbor Solicitation)
- Applied with `ipv6 traffic-filter` (not `access-group`)

---

## 🔥 Firewalls

### Firewall Types

| Type | OSI Layers | Key Trait |
|------|-----------|-----------|
| Packet filter | L3–L4 | Stateless; simplest; most routers support this |
| Stateful | L3–L4 | Tracks connection state; allows return traffic automatically |
| Application/Proxy | **L3–L7** | Full proxy; inspects content; slowest |
| NGFW | L3–L7+ | App awareness + user identity + IPS + SSL inspection |

> ⚠️ **Stateful firewalls CANNOT** prevent application layer attacks — they don't inspect HTTP content. Need NGFW or IPS.
> 💡 **Proxy firewall** inspects Layers 3, 4, 5, and 7 (two additional layers beyond packet filter).

### ASA Security Levels

| Interface | Level | Default Behaviour |
|-----------|-------|-------------------|
| inside | 100 | → lower levels: **permitted** |
| dmz | 50 | → inside: **denied**; → outside: permitted |
| outside | 0 | → any higher: **denied** (requires explicit ACL) |

**Key ASA facts:**
- To allow outside → inside traffic: must configure an **ACL**
- ASA ACLs use **subnet masks**, not wildcard masks
- Transparent mode = "bump in the wire"; invisible to attackers; does **not** support VPNs, QoS, or DHCP Relay
- ASA `show` commands work in any mode — no `do` keyword needed (unlike IOS)
- Object NAT: defined inside network objects; evaluated **second**
- Twice NAT: standalone; evaluated **first**; more flexible

---

## 🛡️ Zone-Based Policy Firewall (ZPF)

### ZPF Key Concepts

| Concept | Description |
|---------|-------------|
| **self zone** | System-defined; applies to traffic **to/from the router itself** — no interface assignment needed |
| Zone-pair | Directional relationship between two zones; has a policy applied |
| class-map | Defines what traffic to match |
| policy-map | Defines what action to take |

### ZPF Actions

| Action | Effect |
|--------|--------|
| `inspect` | Stateful — return traffic automatically permitted ✅ |
| `pass` | One-way permit; no state tracking |
| `drop` | Block traffic (default for all unmatched traffic) |

**Configuration order:** zones → assign interfaces → class-maps → policy-maps → zone-pairs → service-policy

**ZPF benefits over Classic Firewall:**
- Not dependent on ACLs
- Policies are easy to read and troubleshoot
- Default deny between zones (more secure)
- One interface can only be in **one** zone

> ⚠️ Traffic between the **same zone** = permitted by default. An interface **not in any zone** cannot communicate with zoned interfaces.

---

## 🔍 IDS / IPS

### IDS vs IPS

| Feature | IDS | IPS |
|---------|-----|-----|
| Deployment | Out-of-band (passive) | **Inline** (active) |
| Response | Alert only | **Block + alert** |
| Traffic impact | None | Adds latency |
| False positive effect | Alert noise only | Blocks legitimate traffic |
| Uses copy of traffic | Yes (via SPAN/TAP) | No — processes live traffic |

> 💡 **Main difference:** IDS would allow malicious traffic to pass before it is addressed; IPS stops it immediately. IDS has no impact on traffic flow and requires other devices to respond to attacks.

### IPS Detection Methods

| Method | How it Works |
|--------|-------------|
| **Signature/pattern** | Matches pre-defined patterns; simplest; cannot detect zero-days |
| **Anomaly/behaviour** | Deviation from baseline; can detect zero-days; higher false positives |
| **Policy-based** | Admin manually defines suspicious behaviours based on historical analysis |
| **Honey pot** | Decoy server diverts attacks away from production devices |

### Snort IPS

**Rule actions:** `alert` · `log` · `pass` · `drop` (block+log) · `reject` (block+log+TCP RST) · `sdrop` (block, no log)

**Snort IPS on ISR 4000 Series — three policy levels:** `security` · `balanced` · `connectivity`

**Three signature attributes:** action · trigger · type

> 💡 **RSPAN** = uses VLANs to monitor traffic on **remote** switches. SPAN = local switch only.

---

## 🔒 Cryptography

### Symmetric vs Asymmetric

| Feature | Symmetric | Asymmetric |
|---------|-----------|------------|
| Keys | Same key for both ends | Public + private key pair |
| Speed | **Fast** — used for bulk data | Slow — used for key exchange/signatures |
| Problem | Key distribution challenge | CPU intensive |
| Examples | AES, DES, 3DES, RC4 | RSA, DH, ECDH, ECDSA |
| VPN use | Bulk data encryption | Session key exchange |

> 💡 **Site-to-site VPN key type** = **symmetric** key (both routers share the secret).
> 💡 **Security lies in key secrecy**, not the algorithm (Kerckhoffs's principle).

### Hashing & Integrity

| Algorithm | Output | Status |
|-----------|--------|--------|
| MD5 | 128-bit | ❌ Deprecated — collisions known |
| SHA-1 | 160-bit | ❌ Deprecated — collision demonstrated 2017 |
| SHA-256 | 256-bit | ✅ Current standard |
| SHA-512 | 512-bit | ✅ Very strong |

**HMAC** = hash + secret key → provides **integrity AND authentication** (not confidentiality)

**Digital signature** = hash encrypted with **private key** → integrity + authentication + **non-repudiation**

**Salt** = random value appended to password before hashing → defeats rainbow table attacks

> 💡 **IPsec exam mapping:** MD5/SHA = data integrity. AES = confidentiality. DH = key exchange. RSA = authentication.

### PKI & Certificates

| Component | Role |
|-----------|------|
| **CA** | Issues and digitally signs certificates (trust anchor) |
| **RA** | Validates identity before CA issues certificate |
| **CRL** | Published list of revoked certificates |
| **OCSP** | Real-time revocation checking (alternative to CRL) |
| **X.509** | Standard certificate format |

**Certificate classes 0–5:** Higher number = more trusted.
- Class 5 = private organisations or government security (most trusted)
- Class 0 = no checks performed (least trusted)

> 💡 **S/MIME** uses X.509 certificates to support mail protection by mail agents.
> 💡 **PKI** = trusted third-party technology to issue credentials accepted as authoritative identity.

---

## 🌐 VPNs & IPsec

### IPsec — AH vs ESP

| Feature | AH (Protocol 51) | ESP (Protocol 50) |
|---------|-----------------|------------------|
| Confidentiality (encryption) | ❌ No | ✅ Yes |
| Integrity | ✅ Yes | ✅ Yes |
| Authentication | ✅ Yes | ✅ Yes |
| Anti-replay | ✅ Yes | ✅ Yes |
| NAT compatible | ❌ No | ✅ Yes (NAT-T on UDP 4500) |
| Hashing algorithms | MD5, SHA | MD5, SHA |

> 💡 **IPsec VPN setup order:** interesting traffic (ACL) → IKE Phase 1 → IKE Phase 2 → IPsec tunnel → termination.
> 💡 **IKE Phase 1:** Negotiates ISAKMP SA (secure channel between peers).
> 💡 **IKE Phase 2 action = negotiation of IPsec policy** (not IKE policy — that's Phase 1).

### VPN Types

| VPN Type | Key Trait |
|----------|-----------|
| Site-to-site | Always-on; transparent to users; **must be statically set up** |
| Remote access | Client software required; user initiates the connection |
| SSL/AnyConnect | Uses TCP/UDP 443; works through any NAT/firewall |
| GRE over IPsec | GRE handles multicast/routing protocols; IPsec provides encryption |

**Additional IPsec facts:**
- **Crypto ACL** = defines "interesting traffic" to encrypt; must be a **mirror image** on both peers
- **Multiple crypto ACLs** = define different traffic types with different IPsec protection levels
- **PFS** (Perfect Forward Secrecy) = new DH exchange per Phase 2 SA; protects past sessions if long-term key is compromised
- **IKE Phase 1 modes:** Main Mode (6 msgs, secure, identity protected), Aggressive Mode (3 msgs, faster, identity exposed), Quick Mode = Phase 2 only

---

## 🔗 Layer 2 Security

### Attacks & Mitigations

| Attack | Mitigation |
|--------|-----------|
| MAC flooding | Port Security (limit MACs per port) |
| VLAN hopping — switch spoofing | Disable DTP; `switchport mode access`; `switchport nonegotiate` |
| VLAN hopping — double tagging | Change native VLAN to an unused VLAN (not VLAN 1) |
| DHCP starvation | DHCP Snooping + rate limit on untrusted ports |
| Rogue DHCP server | DHCP Snooping (trust only uplink ports toward DHCP server) |
| ARP spoofing/poisoning | Dynamic ARP Inspection (DAI) — requires DHCP Snooping table |
| IP/MAC address spoofing | IP Source Guard (`ip verify source`) — requires DHCP Snooping table |
| STP manipulation | BPDU Guard + Root Guard + PortFast |

> 💡 **VLAN hopping best practice:** Disable trunk negotiation for trunk ports AND statically set nontrunk ports as access ports.
> ⚠️ **MAC overflow attacker goal:** See frames destined for **other hosts** (switch fails open → acts like a hub).

### Port Security Violation Modes

| Mode | Drops? | Logs? | Shuts Port? |
|------|--------|-------|------------|
| `protect` | ✅ | ❌ | ❌ |
| `restrict` | ✅ | ✅ | ❌ |
| `shutdown` | ✅ | ✅ | ✅ **(default)** |

### STP Security Features

| Feature | Applied On | Does |
|---------|-----------|------|
| **PortFast** | Access ports only | Skips listen/learn states for faster host connection |
| **BPDU Guard** | PortFast ports | Err-disables port if any BPDU is received |
| **Root Guard** | Uplink/edge ports | Prevents port from becoming root port |
| **Loop Guard** | Non-designated ports | Detects unidirectional link failures |

### 802.1X — Port-Based NAC

| Role | Device |
|------|--------|
| **Supplicant** | End device (PC, phone) requesting access |
| **Authenticator** | Switch or wireless AP enforcing access control |
| **Authentication Server** | RADIUS / Cisco ISE validating credentials |

- Command to set authenticator role: `dot1x pae authenticator`
- Port control modes: `auto` (require auth) · `force-authorized` (always allow) · `force-unauthorized` (always block)

**EAP Methods:**

| Method | Auth Type | Strength |
|--------|-----------|----------|
| EAP-MD5 | Password hash | Weak — no mutual auth |
| EAP-TLS | Client + server certs | Strongest |
| PEAP | Server cert + tunneled MS-CHAPv2 | Strong — most common |

---

## 📊 Device Monitoring

### Syslog Severity Levels

| Level | Name | Mnemonic |
|-------|------|----------|
| **0** | Emergency | **E**very |
| **1** | Alert | **A**wful |
| **2** | Critical | **C**isco |
| **3** | Error | **E**ngineer |
| **4** | Warning | **W**ill |
| **5** | Notice | **N**eed |
| **6** | Informational | **I**ce |
| **7** | Debug | **D**aily |

> 💡 `logging trap warnings` = sends levels 0–4. Lower number = **more** severe.

### NTP, SNMP & Monitoring Facts

- **NTP Stratum 0** = atomic clock / GPS (reference). Each hop adds 1. Higher stratum = less accurate.
- **Stratum 2 device** = synced from stratum 1; can provide NTP service to other devices
- **SNMPv3** = only version with authentication + encryption. v1/v2c send community strings in cleartext — never use.
- **SNMP TRAP** = unsolicited notification from agent. **INFORM** = trap with acknowledgement (v2c/v3)
- **SIEM** = centralised log correlation, alerting, and forensic analysis
- **NetFlow** = collects IP flow metadata (not payload); used for anomaly detection and capacity planning
- **OOB management limitation** on large networks: all devices appear attached to a **single management network**

---

## 📌 Critical Numbers & Ports

### Protocol Ports

| Protocol | Port | Secure? |
|----------|------|---------|
| SSH | TCP 22 | ✅ |
| Telnet | TCP 23 | ❌ Use SSH |
| TACACS+ | TCP 49 | ✅ |
| HTTP | TCP 80 | ❌ Use HTTPS |
| HTTPS | TCP 443 | ✅ |
| RADIUS auth | UDP 1812 | ⚠️ Password only |
| RADIUS acct | UDP 1813 | ⚠️ |
| SNMP | UDP 161/162 | ⚠️ Use SNMPv3 |
| Syslog | UDP 514 | — |
| IKE (VPN) | UDP 500 | — |
| IKE NAT-T | UDP 4500 | — |
| IPsec ESP | Protocol 50 | ✅ |
| IPsec AH | Protocol 51 | ⚠️ No encryption |

### IOS & ASA Numbers

| Item | Range/Value |
|------|-------------|
| Standard ACL | 1–99, 1300–1999 |
| Extended ACL | 100–199, 2000–2699 |
| IOS privilege levels | 0–15 |
| ASA security levels | 0–100 |
| Syslog severity levels | 0–7 |
| DH minimum group | **14** (2048-bit) |
| Certificate class (best) | **5** (government/private org) |
| Certificate class (worst) | **0** (no checks performed) |

### Insecure → Secure Protocol Replacements

| Insecure | Use Instead |
|----------|------------|
| Telnet | SSH v2 |
| FTP | SFTP / SCP |
| HTTP | HTTPS |
| SNMPv1 / v2c | SNMPv3 |
| MD5 | SHA-256 |
| DES / 3DES | AES-256 |
| RC4 | AES-GCM |
| DH Groups 1/2/5 | DH Group 14+ |
| IPsec AH only | IPsec ESP |

---

## 🏗️ NFP — Network Foundations Protection

Three planes of the router, each requiring separate security:

| Plane | Traffic Type | Security Controls |
|-------|-------------|-------------------|
| **Management** | Traffic to/from the router (SSH, SNMP, syslog) | Role-based access control |
| **Control** | Routing protocols (OSPF, BGP, EIGRP) | Routing protocol authentication, CoPP |
| **Data** | User transit traffic | ACLs, firewall, IPS, uRPF |

> 💡 Management plane = role-based access. Control plane = routing authentication. Data plane = ACLs + antispoofing.

---

## 🧪 Security Testing

| Test Type | What It Does |
|-----------|-------------|
| **Penetration test** | Simulated attacks; determines **feasibility** of attack and possible consequences |
| **Vulnerability scan** | Detects potential weaknesses; does **NOT** exploit them |
| **Network scan** | Identifies open TCP ports and live hosts (Nmap) |
| **Integrity check** | Detects changes that have occurred in a system |
| **Security audit** | Compares configuration against policy or standard |

> 💡 **Nmap** = identifies network layer protocols running on a host (Layer 3 scanning).
> 💡 **BYOD security best practices:** Keep OS/software updated + only turn on Wi-Fi when actively using wireless.

---

## 🚨 Classic Exam Traps — Know These Cold

1. **ASA standard ACL** identifies **destination** IP — not source like IOS. Only used for OSPF routes, cannot be applied to interfaces to control traffic.

2. **ASA ACLs use subnet masks** — NOT wildcard masks (opposite of IOS router ACLs).

3. **ZPF self zone** = system-defined; no interface assignment needed; applies to traffic destined for or originating from the router itself.

4. **`single-connection` keyword** in TACACS+ config = keeps **one TCP connection** open for the life of the session, enhancing performance. Without it, a new TCP connection is opened per session.

5. **IDS uses copies** of traffic via SPAN/TAP; IPS sits **inline** with live traffic.

6. **Stateful firewall CANNOT** stop application layer attacks — it doesn't inspect HTTP content.

7. **Router-generated packets bypass outbound ACLs** — they cannot be filtered by an outbound ACL on that router.

8. **RADIUS function answer:** provides **separate ports for authorization and accounting** — NOT "separate AAA services" (that's TACACS+).

9. **Worm vs Virus:** Virus attaches to a file and needs user action. Worm self-replicates independently by exploiting network vulnerabilities.

10. **SSL Appliance** is needed to decrypt SSL traffic so an IPS can inspect it for hidden threats.

11. **OSPF MD5 authentication** does **NOT** encrypt routing updates — it only authenticates them.

12. **`local-case` keyword** in AAA local auth = authentication is **case-sensitive** for usernames/passwords.

13. **Symmetric encryption algorithms** are used to **encrypt data** (fast). Asymmetric are used for **key exchange and signatures** (slow).

14. **DES weak keys** = four keys for which encryption is identical to decryption (not about key length).

15. **Traffic from private network → DMZ** = selectively permitted and inspected (not "little or no restrictions" — that's internal LAN to LAN).

16. **SCP → SSH** = SCP uses SSH to securely transfer files

---

*Cisco NetAcad · Network Security 1.0 · Final Exam Cheatsheet*
