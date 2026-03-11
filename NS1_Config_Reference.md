# 🖥️ Cisco NetAcad Network Security 1.0
## Complete Configuration Reference

---

> **Purpose:** Ready-to-use configuration templates for every device and technology in the NS 1.0 course.  
> **Format:** Annotated IOS/ASA CLI — copy, adapt, and apply.  
> **Devices covered:** Routers (IOS), Switches (IOS), ASA Firewall, with AAA, VPN, IPS, and Layer 2 security.

---

## 📋 Table of Contents

| Section | Topic |
|---------|-------|
| 1 | [Router — Baseline Hardening](#1-router--baseline-hardening) |
| 2 | [Router — SSH & Secure Remote Access](#2-router--ssh--secure-remote-access) |
| 3 | [Router — AAA Local Authentication](#3-router--aaa-local-authentication) |
| 4 | [Router — AAA Server-Based (TACACS+ & RADIUS)](#4-router--aaa-server-based-tacacs--radius) |
| 5 | [Router — Privilege Levels & CLI Views](#5-router--privilege-levels--cli-views) |
| 6 | [Router — Device Monitoring (Syslog, NTP, SNMP, NetFlow)](#6-router--device-monitoring-syslog-ntp-snmp-netflow) |
| 7 | [Router — Routing Protocol Authentication](#7-router--routing-protocol-authentication) |
| 8 | [Router — AutoSecure & NFP / CoPP](#8-router--autosecure--nfp--copp) |
| 9 | [Router — Standard & Extended ACLs](#9-router--standard--extended-acls) |
| 10 | [Router — IPv6 ACLs](#10-router--ipv6-acls) |
| 11 | [Router — Zone-Based Policy Firewall (ZPF)](#11-router--zone-based-policy-firewall-zpf) |
| 12 | [Router — IOS IPS (Snort/UTD)](#12-router--ios-ips-snorutd) |
| 13 | [Router — Site-to-Site IPsec VPN](#13-router--site-to-site-ipsec-vpn) |
| 14 | [Switch — Baseline Hardening](#14-switch--baseline-hardening) |
| 15 | [Switch — Port Security](#15-switch--port-security) |
| 16 | [Switch — VLAN Security](#16-switch--vlan-security) |
| 17 | [Switch — DHCP Snooping](#17-switch--dhcp-snooping) |
| 18 | [Switch — Dynamic ARP Inspection (DAI)](#18-switch--dynamic-arp-inspection-dai) |
| 19 | [Switch — IP Source Guard](#19-switch--ip-source-guard) |
| 20 | [Switch — STP Security (PortFast, BPDU Guard, Root Guard)](#20-switch--stp-security) |
| 21 | [Switch — 802.1X Port-Based NAC](#21-switch--8021x-port-based-nac) |
| 22 | [Switch — SPAN / RSPAN (Traffic Mirroring)](#22-switch--span--rspan) |
| 23 | [ASA — Baseline & Interface Configuration](#23-asa--baseline--interface-configuration) |
| 24 | [ASA — Management & SSH/ASDM Access](#24-asa--management--sshasdm-access) |
| 25 | [ASA — AAA Configuration](#25-asa--aaa-configuration) |
| 26 | [ASA — Object Groups & ACLs](#26-asa--object-groups--acls) |
| 27 | [ASA — NAT (Object NAT & Twice NAT)](#27-asa--nat-object-nat--twice-nat) |
| 28 | [ASA — Service Policies (MPF)](#28-asa--service-policies-mpf) |
| 29 | [ASA — Site-to-Site IPsec VPN](#29-asa--site-to-site-ipsec-vpn) |
| 30 | [ASA — AnyConnect SSL VPN](#30-asa--anyconnect-ssl-vpn) |
| 31 | [Verification & Troubleshooting Commands](#31-verification--troubleshooting-commands) |

---

# 1. Router — Baseline Hardening

Apply to **every** router before connecting to any network.

```
! ════════════════════════════════════════════════════════════
! STEP 1 — Identity
! ════════════════════════════════════════════════════════════
hostname R1
ip domain-name company.com

! ════════════════════════════════════════════════════════════
! STEP 2 — Privileged EXEC Password
! Always use 'enable secret' (MD5/Scrypt hashed)
! NEVER use 'enable password' (weak reversible encryption)
! ════════════════════════════════════════════════════════════
enable secret Str0ng!Enable@P@ss

! ════════════════════════════════════════════════════════════
! STEP 3 — Encrypt all plaintext passwords in running-config
! ════════════════════════════════════════════════════════════
service password-encryption

! ════════════════════════════════════════════════════════════
! STEP 4 — Password Policy
! ════════════════════════════════════════════════════════════
security passwords min-length 10

! ════════════════════════════════════════════════════════════
! STEP 5 — Local User Database
! privilege 15 = full access; lower levels = restricted
! ════════════════════════════════════════════════════════════
username admin privilege 15 secret Admin@Secure!1
username netops privilege 7 secret NetOps@Secure!1

! ════════════════════════════════════════════════════════════
! STEP 6 — Console Port Security
! ════════════════════════════════════════════════════════════
line console 0
 login local                   ! Use local user database
 exec-timeout 5 0              ! Logout after 5 min idle (5 min 0 sec)
 logging synchronous           ! Prevent syslog interrupting typed commands

! ════════════════════════════════════════════════════════════
! STEP 7 — Auxiliary Port (disable — rarely needed)
! ════════════════════════════════════════════════════════════
line aux 0
 no exec
 transport input none
 exec-timeout 0 1

! ════════════════════════════════════════════════════════════
! STEP 8 — VTY Lines (SSH only — see Section 2 for SSH setup)
! ════════════════════════════════════════════════════════════
line vty 0 4
 login local
 transport input ssh            ! SSH only; Telnet disabled
 exec-timeout 5 0

! ════════════════════════════════════════════════════════════
! STEP 9 — Login Attack Mitigation
! Block logins for 120s after 3 failures within 60s
! ════════════════════════════════════════════════════════════
login block-for 120 attempts 3 within 60
login on-failure log            ! Log failed login attempts
login on-success log            ! Log successful logins

! ════════════════════════════════════════════════════════════
! STEP 10 — Legal Warning Banner
! NEVER use "Welcome" — use a warning that access is restricted
! ════════════════════════════════════════════════════════════
banner motd ^
========================================================
  AUTHORISED ACCESS ONLY
  All activity on this device is monitored and logged.
  Unauthorised access is prohibited and will be
  reported to the appropriate authorities.
========================================================
^

! ════════════════════════════════════════════════════════════
! STEP 11 — Disable Unused & Insecure Services
! ════════════════════════════════════════════════════════════
no ip http server                ! Disable HTTP management server
no ip http secure-server         ! Disable HTTPS server (unless needed for ASDM)
no cdp run                       ! Disable CDP globally (re-enable per interface if needed for VoIP)
no ip finger                     ! Disable finger service
no service finger
no service tcp-small-servers     ! Disable echo, chargen, etc. on TCP
no service udp-small-servers     ! Disable echo, chargen, etc. on UDP
no ip bootp server               ! Disable BOOTP
no ip source-route               ! Disable loose/strict source routing (used in attacks)
no ip gratuitous-arps            ! Disable gratuitous ARP on boot

! ════════════════════════════════════════════════════════════
! STEP 12 — Disable Proxy ARP & Directed Broadcasts on interfaces
! ════════════════════════════════════════════════════════════
interface GigabitEthernet0/0
 no ip proxy-arp                 ! Prevent router answering ARP on behalf of other hosts
 no ip directed-broadcast        ! Block directed broadcast (used in Smurf attacks)

! ════════════════════════════════════════════════════════════
! STEP 13 — Save Configuration
! ════════════════════════════════════════════════════════════
copy running-config startup-config
```

---

# 2. Router — SSH & Secure Remote Access

```
! ════════════════════════════════════════════════════════════
! Prerequisites: hostname and domain must be set (Section 1)
! ════════════════════════════════════════════════════════════
hostname R1
ip domain-name company.com

! ════════════════════════════════════════════════════════════
! STEP 1 — Generate RSA key pair
! Minimum 1024 bits; 2048 recommended; 4096 for high security
! ════════════════════════════════════════════════════════════
crypto key generate rsa modulus 2048
! (Answer "yes" if prompted to replace existing keys)

! ════════════════════════════════════════════════════════════
! STEP 2 — Set SSH Version 2 (v1 has known vulnerabilities)
! ════════════════════════════════════════════════════════════
ip ssh version 2
ip ssh time-out 60               ! Disconnect unauthenticated sessions after 60s
ip ssh authentication-retries 3  ! Allow 3 password attempts before disconnect

! ════════════════════════════════════════════════════════════
! STEP 3 — Local user (if not using AAA server)
! ════════════════════════════════════════════════════════════
username admin privilege 15 secret Admin@SSH!P@ss

! ════════════════════════════════════════════════════════════
! STEP 4 — VTY lines: SSH only, local auth
! ════════════════════════════════════════════════════════════
line vty 0 4
 login local
 transport input ssh             ! Explicitly deny Telnet
 exec-timeout 5 0
 access-class MGMT-ACCESS in    ! Restrict SSH to management subnet (see ACL below)

! ════════════════════════════════════════════════════════════
! STEP 5 — Restrict SSH access by source IP (optional but recommended)
! ════════════════════════════════════════════════════════════
ip access-list standard MGMT-ACCESS
 remark Permit only management subnet
 permit 10.10.10.0 0.0.0.255
 deny any log

! ════════════════════════════════════════════════════════════
! STEP 6 — Verify SSH
! ════════════════════════════════════════════════════════════
! show ip ssh
! show ssh
```

---

# 3. Router — AAA Local Authentication

Local AAA uses the device's own username database. Good for small networks or as a fallback.

```
! ════════════════════════════════════════════════════════════
! STEP 1 — Enable AAA globally
! This disables the old 'login' command model on lines
! ════════════════════════════════════════════════════════════
aaa new-model

! ════════════════════════════════════════════════════════════
! STEP 2 — Local user database
! ════════════════════════════════════════════════════════════
username admin privilege 15 secret Admin@Local!1
username helpdesk privilege 5 secret HelpDesk@Local!1
username readonly privilege 1 secret ReadOnly@Local!1

! ════════════════════════════════════════════════════════════
! STEP 3 — Authentication method list
! "default" applies to ALL lines unless overridden
! ════════════════════════════════════════════════════════════
aaa authentication login default local

! Named list — for specific lines only
aaa authentication login CONSOLE-AUTH local
aaa authentication login VTY-AUTH local

! ════════════════════════════════════════════════════════════
! STEP 4 — Authorization (what exec users can do)
! ════════════════════════════════════════════════════════════
aaa authorization exec default local if-authenticated
aaa authorization commands 15 default local if-authenticated

! ════════════════════════════════════════════════════════════
! STEP 5 — Accounting (log what users do)
! local accounting writes to local syslog buffer
! ════════════════════════════════════════════════════════════
aaa accounting exec default start-stop local
aaa accounting commands 15 default start-stop local

! ════════════════════════════════════════════════════════════
! STEP 6 — Apply to lines
! ════════════════════════════════════════════════════════════
line console 0
 login authentication CONSOLE-AUTH
 exec-timeout 5 0

line vty 0 4
 login authentication VTY-AUTH
 transport input ssh
 exec-timeout 5 0

! ════════════════════════════════════════════════════════════
! STEP 7 — Verify
! ════════════════════════════════════════════════════════════
! show aaa local user lockout
! show users
```

---

# 4. Router — AAA Server-Based (TACACS+ & RADIUS)

## 4A — TACACS+ (Recommended for Device Administration)

```
! ════════════════════════════════════════════════════════════
! TACACS+ — TCP port 49; encrypts entire packet
! Best for: device admin (routers, switches, firewalls)
! ════════════════════════════════════════════════════════════

aaa new-model

! ── Define TACACS+ Server ────────────────────────────────────
tacacs server ISE-TACACS-PRIMARY
 address ipv4 10.0.0.50
 key TacacsSharedSecret@123       ! Must match server config
 timeout 5                        ! Seconds to wait for response

tacacs server ISE-TACACS-SECONDARY
 address ipv4 10.0.0.51
 key TacacsSharedSecret@123
 timeout 5

! ── Define Server Group ──────────────────────────────────────
aaa group server tacacs+ TACACS-SERVERS
 server name ISE-TACACS-PRIMARY
 server name ISE-TACACS-SECONDARY

! ── Authentication ───────────────────────────────────────────
! Try TACACS+ first; if server unreachable, use local
aaa authentication login default group TACACS-SERVERS local
aaa authentication enable default group TACACS-SERVERS enable

! ── Authorization ────────────────────────────────────────────
! Control what commands users can run
aaa authorization exec default group TACACS-SERVERS local if-authenticated
aaa authorization commands 1 default group TACACS-SERVERS local if-authenticated
aaa authorization commands 15 default group TACACS-SERVERS local if-authenticated
aaa authorization config-commands                     ! Authorize config-mode commands too

! ── Accounting ───────────────────────────────────────────────
! Log start/stop of exec sessions AND every command to TACACS+ server
aaa accounting exec default start-stop group TACACS-SERVERS
aaa accounting commands 1 default start-stop group TACACS-SERVERS
aaa accounting commands 15 default start-stop group TACACS-SERVERS
aaa accounting connection default start-stop group TACACS-SERVERS

! ── Apply to Lines ───────────────────────────────────────────
line console 0
 login authentication default
 exec-timeout 5 0

line vty 0 4
 login authentication default
 transport input ssh
 exec-timeout 5 0

! ── Verify ───────────────────────────────────────────────────
! show tacacs
! show aaa servers
! show aaa sessions
! test aaa group TACACS-SERVERS admin testpass legacy
```

## 4B — RADIUS (Recommended for Network Access: VPN, 802.1X, Wi-Fi)

```
! ════════════════════════════════════════════════════════════
! RADIUS — UDP ports 1812/1813; encrypts password only
! Best for: network access (VPN, 802.1X, wireless)
! ════════════════════════════════════════════════════════════

aaa new-model

! ── Define RADIUS Server ─────────────────────────────────────
radius server ISE-RADIUS-PRIMARY
 address ipv4 10.0.0.52 auth-port 1812 acct-port 1813
 key RadiusSharedSecret@123

radius server ISE-RADIUS-SECONDARY
 address ipv4 10.0.0.53 auth-port 1812 acct-port 1813
 key RadiusSharedSecret@123

! ── Define Server Group ──────────────────────────────────────
aaa group server radius RADIUS-SERVERS
 server name ISE-RADIUS-PRIMARY
 server name ISE-RADIUS-SECONDARY
 deadtime 15                      ! Don't try a dead server for 15 min

! ── Authentication ───────────────────────────────────────────
aaa authentication login NETWORK-ACCESS group RADIUS-SERVERS local
aaa authentication dot1x default group RADIUS-SERVERS

! ── Authorization ────────────────────────────────────────────
aaa authorization network default group RADIUS-SERVERS
aaa authorization auth-proxy default group RADIUS-SERVERS

! ── Accounting ───────────────────────────────────────────────
aaa accounting network default start-stop group RADIUS-SERVERS
aaa accounting dot1x default start-stop group RADIUS-SERVERS

! ── Verify ───────────────────────────────────────────────────
! show aaa servers
! show radius server-group RADIUS-SERVERS
! test aaa group RADIUS-SERVERS user@company.com testpass legacy
```

---

# 5. Router — Privilege Levels & CLI Views

## 5A — Privilege Levels

```
! ════════════════════════════════════════════════════════════
! Privilege levels 0-15; 1=user, 15=full admin
! Assign specific commands to intermediate levels
! ════════════════════════════════════════════════════════════

! ── Assign commands to level 5 (Help Desk) ──────────────────
privilege exec level 5 show version
privilege exec level 5 show running-config
privilege exec level 5 show ip interface brief
privilege exec level 5 show ip route
privilege exec level 5 show interfaces
privilege exec level 5 ping
privilege exec level 5 traceroute

! ── Assign commands to level 10 (Network Operator) ──────────
privilege exec level 10 debug ip ospf
privilege exec level 10 clear ip ospf process
privilege exec level 10 show crypto isakmp sa
privilege exec level 10 show crypto ipsec sa

! ── Create users at specific levels ─────────────────────────
username helpdesk privilege 5 secret HelpDesk@P@ss!
username netoperator privilege 10 secret NetOp@P@ss!
username admin privilege 15 secret Admin@P@ss!

! ── Set enable secret for each level (so users can 'enable N') ─
enable secret level 5 Level5@Enable!
enable secret level 10 Level10@Enable!

! ── Verify current privilege level ──────────────────────────
! show privilege
```

## 5B — Role-Based CLI Views

```
! ════════════════════════════════════════════════════════════
! CLI Views provide per-command access control
! More granular than privilege levels
! Requires AAA to be enabled
! ════════════════════════════════════════════════════════════

! ── Prerequisite: Enable AAA ─────────────────────────────────
aaa new-model

! ── Enter Root View (required to create views) ───────────────
! enable view          (then enter enable secret)

! ── Create HELPDESK view ─────────────────────────────────────
parser view HELPDESK
 secret HelpDesk@View!
 commands exec include show version
 commands exec include show interfaces
 commands exec include show ip interface brief
 commands exec include show ip route
 commands exec include ping
 commands exec include traceroute
 commands exec include exit
 commands exec include logout

! ── Create NETOPS view ───────────────────────────────────────
parser view NETOPS
 secret NetOps@View!
 commands exec include show
 commands exec include ping
 commands exec include traceroute
 commands exec include debug ip ospf
 commands exec include debug ip routing
 commands exec include undebug all
 commands exec include clear ip ospf process
 commands configure include router ospf
 commands configure include interface

! ── Create ADMIN Superview (combines multiple views) ─────────
parser view ADMIN superview
 secret Admin@Super!
 view HELPDESK
 view NETOPS

! ── Switch into a view ───────────────────────────────────────
! enable view HELPDESK
! (prompted for HELPDESK view password)

! ── Verify ───────────────────────────────────────────────────
! show parser view
! show parser view all
```

---

# 6. Router — Device Monitoring (Syslog, NTP, SNMP, NetFlow)

## 6A — Syslog

```
! ════════════════════════════════════════════════════════════
! Severity levels: 0=Emergency, 1=Alert, 2=Critical, 3=Error,
!                  4=Warning, 5=Notice, 6=Info, 7=Debug
! 'logging trap N' sends level N and all MORE severe (lower #)
! ════════════════════════════════════════════════════════════

! ── Send logs to syslog server ──────────────────────────────
logging host 10.0.0.100
logging trap informational      ! Send levels 0-6 to server
logging source-interface Loopback0   ! Consistent source IP for all syslog

! ── Local buffer (retain recent logs if server unreachable) ──
logging buffered 64000 debugging

! ── Console logging (reduce to warnings to avoid noise) ──────
logging console warnings

! ── Timestamps (essential for log correlation) ───────────────
service timestamps log datetime msec
service timestamps debug datetime msec

! ── Enable sequence numbers in logs ─────────────────────────
service sequence-numbers

! ── Enable logging ───────────────────────────────────────────
logging on

! ── Verify ───────────────────────────────────────────────────
! show logging
! show logging | include %SEC       (filter security messages)
```

## 6B — NTP

```
! ════════════════════════════════════════════════════════════
! Accurate time is critical for:
!   - Log correlation across devices
!   - Certificate validity (Not Before/Not After)
!   - Kerberos authentication (5-min clock skew tolerance)
! ════════════════════════════════════════════════════════════

! ── Basic NTP client ─────────────────────────────────────────
ntp server 10.0.0.10 prefer
ntp server 10.0.0.11

! ── NTP Authentication (prevent rogue NTP servers) ───────────
ntp authenticate
ntp authentication-key 1 md5 NTPSecret@Key!
ntp trusted-key 1
ntp server 10.0.0.10 key 1
ntp server 10.0.0.11 key 1

! ── Set timezone ─────────────────────────────────────────────
clock timezone CET 1
clock summer-time CEST recurring last Sun Mar 2:00 last Sun Oct 3:00

! ── Set this device as NTP master (internal lab use only) ────
! ntp master 5     ! Stratum 5 — only if no external NTP server

! ── Restrict NTP queries (ACL) ───────────────────────────────
ntp access-group peer 10           ! ACL 10 = permitted NTP peers

! ── Verify ───────────────────────────────────────────────────
! show ntp status
! show ntp associations
! show clock detail
```

## 6C — SNMPv3

```
! ════════════════════════════════════════════════════════════
! ALWAYS use SNMPv3 in production
! SNMPv1/v2c send community strings in cleartext — never use
! ════════════════════════════════════════════════════════════

! ── Create SNMPv3 group (auth + priv required) ───────────────
snmp-server group SNMPV3-GROUP v3 priv
! priv = require both authentication AND encryption
! auth = require authentication only (no encryption)
! noauth = no security (equivalent to v1/v2c) — never use

! ── Create SNMPv3 user ───────────────────────────────────────
snmp-server user SNMPUSER SNMPV3-GROUP v3 auth sha Auth@P@ss! priv aes 128 Priv@P@ss!
! auth sha = use SHA for HMAC authentication
! priv aes 128 = use AES-128 for encryption

! ── Set device information ───────────────────────────────────
snmp-server contact noc@company.com
snmp-server location "Server Room, Building A"

! ── Enable SNMP traps (alerts) to NMS ───────────────────────
snmp-server host 10.0.0.200 version 3 priv SNMPUSER
snmp-server enable traps

! ── Restrict SNMP access to NMS only (ACL) ──────────────────
access-list 20 permit host 10.0.0.200
access-list 20 deny any log
snmp-server community READ-ONLY ro 20   ! If v2c must be used (avoid if possible)

! ── Trap source interface ────────────────────────────────────
snmp-server trap-source Loopback0

! ── Verify ───────────────────────────────────────────────────
! show snmp
! show snmp user
! show snmp group
```

## 6D — NetFlow

```
! ════════════════════════════════════════════════════════════
! NetFlow collects IP traffic metadata (who talks to whom)
! Does NOT capture payload — just flow statistics
! Useful for: anomaly detection, capacity planning, forensics
! ════════════════════════════════════════════════════════════

! ── Enable NetFlow on interfaces ─────────────────────────────
interface GigabitEthernet0/0
 ip flow ingress
 ip flow egress

interface GigabitEthernet0/1
 ip flow ingress
 ip flow egress

! ── Export flows to NetFlow collector ────────────────────────
ip flow-export destination 10.0.0.200 9996   ! Collector IP and UDP port
ip flow-export version 9                      ! v9 = flexible, supports IPv6
ip flow-export source GigabitEthernet0/0     ! Source interface

! ── Tune flow timeout ────────────────────────────────────────
ip flow-cache timeout active 1               ! Export active flows every 1 min
ip flow-cache timeout inactive 15            ! Export idle flows after 15 sec

! ── Verify ───────────────────────────────────────────────────
! show ip cache flow
! show ip flow interface
! show ip flow export
```

## 6E — IOS Image & Config Protection

```
! ════════════════════════════════════════════════════════════
! Protect IOS image and startup-config from deletion/replacement
! ════════════════════════════════════════════════════════════

! ── Resilient IOS (saves secure copy of image + config) ──────
secure boot-image           ! Protect IOS image file
secure boot-config          ! Protect startup-config

! ── Verify ───────────────────────────────────────────────────
! show secure bootset

! ── Verify IOS image integrity (MD5 hash check) ──────────────
! verify /md5 flash:/c1900-universalk9-mz.SPA.154-3.M2.bin

! ── Archive configuration ────────────────────────────────────
archive
 path tftp://10.0.0.100/$h-config     ! $h = device hostname
 write-memory                          ! Archive on every write mem
 time-period 1440                      ! Also archive every 24 hours

! ── Secure Copy (SCP) for file transfer ──────────────────────
ip scp server enable
! (Requires AAA and SSH to be configured)
```

---

# 7. Router — Routing Protocol Authentication

## 7A — OSPFv2 Authentication

```
! ════════════════════════════════════════════════════════════
! OSPF MD5 authentication prevents fake routing updates
! Configure on each interface participating in OSPF
! ════════════════════════════════════════════════════════════

! ── Method 1: Interface-level MD5 ───────────────────────────
interface GigabitEthernet0/0
 ip ospf message-digest-key 1 md5 OSPF@AuthKey!    ! Key ID 1, MD5 hash
 ip ospf authentication message-digest              ! Enable MD5 on this interface

! ── Method 2: Area-level (applies to all interfaces in area) ─
router ospf 1
 area 0 authentication message-digest

! ── Verify ───────────────────────────────────────────────────
! show ip ospf interface GigabitEthernet0/0
! show ip ospf neighbor
```

## 7B — EIGRP Authentication

```
! ════════════════════════════════════════════════════════════
! EIGRP uses key chains for authentication
! ════════════════════════════════════════════════════════════

! ── Step 1: Create key chain ─────────────────────────────────
key chain EIGRP-KEYCHAIN
 key 1
  key-string EIGRP@AuthKey!
  ! Optional: set time validity
  accept-lifetime 00:00:00 Jan 1 2024 infinite
  send-lifetime 00:00:00 Jan 1 2024 infinite

! ── Step 2: Apply to interface ───────────────────────────────
interface GigabitEthernet0/0
 ip authentication mode eigrp 100 md5          ! AS number = 100
 ip authentication key-chain eigrp 100 EIGRP-KEYCHAIN

! ── Verify ───────────────────────────────────────────────────
! show ip eigrp neighbors
! show key chain
```

## 7C — RIPv2 Authentication

```
key chain RIP-KEYCHAIN
 key 1
  key-string RIP@AuthKey!

interface GigabitEthernet0/0
 ip rip authentication mode md5
 ip rip authentication key-chain RIP-KEYCHAIN

! Verify
! show ip rip database
```

---

# 8. Router — AutoSecure & NFP / CoPP

## 8A — AutoSecure

```
! ════════════════════════════════════════════════════════════
! AutoSecure applies a security hardening baseline automatically
! Use in labs; review output before using in production
! ════════════════════════════════════════════════════════════

! Interactive mode (guided — asks questions about your topology)
auto secure

! Non-interactive mode (applies defaults without prompting)
auto secure no-interact

! What AutoSecure does automatically:
!   - Disables: Finger, HTTP, CDP, BOOTp, PAD, IP source route
!   - Enables: service password-encryption, SSH, login block-for
!   - Sets: exec-timeout, security passwords min-length
!   - Applies: basic ACLs for bogon filtering
!   - Enables: TCP intercept and uRPF (where applicable)
```

## 8B — Control Plane Policing (CoPP)

```
! ════════════════════════════════════════════════════════════
! CoPP protects the router CPU from being overwhelmed
! by excessive control-plane traffic (floods, scans, attacks)
! Uses MQC (Modular QoS CLI): class-map → policy-map → control-plane
! ════════════════════════════════════════════════════════════

! ── Step 1: Define class maps (what traffic to rate-limit) ────
class-map match-any COPP-CRITICAL
 match protocol ospf
 match protocol eigrp
 match protocol bgp

class-map match-any COPP-IMPORTANT
 match protocol ssh
 match protocol snmp

class-map match-any COPP-NORMAL
 match protocol icmp

class-map match-any COPP-DEFAULT
 match any

! ── Step 2: Define policy map (rate limits) ──────────────────
policy-map COPP-POLICY
 class COPP-CRITICAL
  police rate 64000 bps conform-action transmit exceed-action transmit
 class COPP-IMPORTANT
  police rate 32000 bps conform-action transmit exceed-action drop
 class COPP-NORMAL
  police rate 8000 bps conform-action transmit exceed-action drop
 class COPP-DEFAULT
  police rate 1000 bps conform-action transmit exceed-action drop

! ── Step 3: Apply to control plane ───────────────────────────
control-plane
 service-policy input COPP-POLICY

! ── Verify ───────────────────────────────────────────────────
! show policy-map control-plane
```

---

# 9. Router — Standard & Extended ACLs

## 9A — Standard ACLs (Source IP Only)

```
! ════════════════════════════════════════════════════════════
! Standard ACLs: filter on SOURCE IP only
! Numbered: 1-99, 1300-1999
! Placement: CLOSE TO DESTINATION (to avoid over-blocking)
! Wildcard mask: inverse of subnet mask (0=match, 1=ignore)
! ════════════════════════════════════════════════════════════

! ── Numbered standard ACL ────────────────────────────────────
access-list 10 remark Permit Sales and block everything else
access-list 10 permit 192.168.10.0 0.0.0.255    ! Permit /24 network
access-list 10 deny any log                       ! Deny all, log it

! ── Named standard ACL (preferred — easier to edit) ──────────
ip access-list standard MGMT-RESTRICT
 remark Allow management subnet; deny all others
 permit 10.10.10.0 0.0.0.255
 permit host 10.10.10.250                         ! Single host
 deny any log

! ── Apply to interface ───────────────────────────────────────
interface GigabitEthernet0/1
 ip access-group 10 out                           ! Outbound on internal interface
 ! or:
 ip access-group MGMT-RESTRICT in                 ! Inbound from outside

! ── Apply to VTY lines (restrict management access) ──────────
line vty 0 4
 access-class MGMT-RESTRICT in
```

## 9B — Extended ACLs (Source, Destination, Protocol, Port)

```
! ════════════════════════════════════════════════════════════
! Extended ACLs: filter on src IP, dst IP, protocol, port
! Numbered: 100-199, 2000-2699
! Placement: CLOSE TO SOURCE (to stop unwanted traffic early)
! ════════════════════════════════════════════════════════════

! ── Permit web to DMZ server; block everything else ──────────
ip access-list extended OUTSIDE-TO-DMZ
 remark Permit HTTP and HTTPS to web server only
 permit tcp any host 172.16.1.10 eq 80
 permit tcp any host 172.16.1.10 eq 443
 remark Permit SMTP to mail server
 permit tcp any host 172.16.1.20 eq 25
 remark Block everything else, log it
 deny ip any any log

interface GigabitEthernet0/1
 ip access-group OUTSIDE-TO-DMZ in

! ── Anti-spoofing ACL (apply inbound on external interface) ──
ip access-list extended ANTI-SPOOFING
 remark Block RFC 1918 arriving from outside (spoofed)
 deny ip 10.0.0.0 0.255.255.255 any log
 deny ip 172.16.0.0 0.15.255.255 any log
 deny ip 192.168.0.0 0.0.255.255 any log
 remark Block loopback addresses
 deny ip 127.0.0.0 0.255.255.255 any log
 remark Block multicast source
 deny ip 224.0.0.0 15.255.255.255 any log
 remark Permit everything else
 permit ip any any

interface GigabitEthernet0/1
 ip access-group ANTI-SPOOFING in          ! Apply inbound on WAN interface

! ── Permit established TCP return traffic ────────────────────
ip access-list extended ALLOW-ESTABLISHED
 permit tcp any any established            ! Allow return traffic for existing sessions
 deny tcp any any log

! ── Block specific attack ports ──────────────────────────────
ip access-list extended BLOCK-ATTACKS
 deny tcp any any eq 23 log               ! Telnet
 deny tcp any any eq 135 log              ! RPC
 deny tcp any any eq 137 log              ! NetBIOS
 deny tcp any any eq 138 log              ! NetBIOS
 deny tcp any any eq 139 log              ! NetBIOS
 deny tcp any any eq 445 log              ! SMB
 deny tcp any any eq 3389 log             ! RDP (restrict to VPN only)
 permit ip any any

! ── Verify ───────────────────────────────────────────────────
! show ip access-lists
! show ip access-lists OUTSIDE-TO-DMZ
! show ip interface GigabitEthernet0/1   (shows applied ACLs)
! clear ip access-list counters          (reset hit counters for testing)
```

---

# 10. Router — IPv6 ACLs

```
! ════════════════════════════════════════════════════════════
! IPv6 ACLs: named only (no numbered)
! CRITICAL: Must explicitly permit NDP (Neighbor Discovery)
!           or IPv6 communication will break completely
! Applied with: ipv6 traffic-filter <name> in|out
! Subnet masks used (not wildcard masks)
! ════════════════════════════════════════════════════════════

! ── Permit NDP and specific services ─────────────────────────
ipv6 access-list IPv6-INBOUND
 remark Permit NDP — required for IPv6 to function
 permit icmp any any nd-na                 ! Neighbor Advertisement
 permit icmp any any nd-ns                 ! Neighbor Solicitation
 permit icmp any any router-advertisement  ! Router Advertisement
 permit icmp any any router-solicitation   ! Router Solicitation
 remark Permit specific traffic
 permit tcp any any eq 443                 ! HTTPS
 permit tcp any any eq 22                  ! SSH
 permit udp any any eq 53                  ! DNS
 remark Block everything else
 deny ipv6 any any log

interface GigabitEthernet0/0
 ipv6 traffic-filter IPv6-INBOUND in

! ── Verify ───────────────────────────────────────────────────
! show ipv6 access-list
! show ipv6 interface GigabitEthernet0/0
```

---

# 11. Router — Zone-Based Policy Firewall (ZPF)

```
! ════════════════════════════════════════════════════════════
! ZPF turns an IOS router into a stateful firewall
! Traffic between zones: DENIED by default unless policy permits
! Use 'inspect' for stateful tracking (return traffic auto-allowed)
! Use 'pass' for one-way permit (no state)
! Use 'drop' to block (default for unmatched traffic)
! Self zone = the router itself
! ════════════════════════════════════════════════════════════

! ── STEP 1: Define zones ─────────────────────────────────────
zone security INSIDE
 description Internal trusted network
zone security OUTSIDE
 description Untrusted Internet
zone security DMZ
 description Semi-trusted DMZ

! ── STEP 2: Assign interfaces to zones ───────────────────────
interface GigabitEthernet0/0
 zone-member security INSIDE
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 zone-member security OUTSIDE
 ip address 203.0.113.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/2
 zone-member security DMZ
 ip address 172.16.1.1 255.255.255.0
 no shutdown

! ── STEP 3: Class maps (match traffic) ───────────────────────

! Inside to Outside: web browsing, DNS, email
class-map type inspect match-any CM-INSIDE-TO-OUTSIDE
 match protocol http
 match protocol https
 match protocol dns
 match protocol smtp
 match protocol pop3
 match protocol imap
 match protocol icmp
 match protocol ftp

! Outside to DMZ: only web access to public servers
class-map type inspect match-any CM-OUTSIDE-TO-DMZ
 match protocol http
 match protocol https
 match protocol smtp                         ! For mail server in DMZ

! Inside to DMZ: allow all
class-map type inspect match-any CM-INSIDE-TO-DMZ
 match ip address ACL-INSIDE-TO-DMZ

! Management traffic to router (self zone)
class-map type inspect match-any CM-MGMT-TO-SELF
 match protocol ssh
 match protocol ntp
 match protocol snmp

! ── STEP 4: ACL for inside-to-DMZ matching ───────────────────
ip access-list extended ACL-INSIDE-TO-DMZ
 permit ip 192.168.1.0 0.0.0.255 172.16.1.0 0.0.0.255

! ── STEP 5: Policy maps (what to do with matched traffic) ─────

policy-map type inspect PM-INSIDE-TO-OUTSIDE
 class type inspect CM-INSIDE-TO-OUTSIDE
  inspect                          ! Stateful — return traffic auto-allowed
 class class-default
  drop log                         ! Drop and log unmatched traffic

policy-map type inspect PM-OUTSIDE-TO-DMZ
 class type inspect CM-OUTSIDE-TO-DMZ
  inspect
 class class-default
  drop log

policy-map type inspect PM-INSIDE-TO-DMZ
 class type inspect CM-INSIDE-TO-DMZ
  inspect
 class class-default
  drop log

policy-map type inspect PM-MGMT-TO-SELF
 class type inspect CM-MGMT-TO-SELF
  inspect
 class class-default
  drop

! ── STEP 6: Zone-pairs (direction + attach policy) ────────────

zone-pair security ZP-INSIDE-OUTSIDE source INSIDE destination OUTSIDE
 service-policy type inspect PM-INSIDE-TO-OUTSIDE

zone-pair security ZP-OUTSIDE-DMZ source OUTSIDE destination DMZ
 service-policy type inspect PM-OUTSIDE-TO-DMZ

zone-pair security ZP-INSIDE-DMZ source INSIDE destination DMZ
 service-policy type inspect PM-INSIDE-TO-DMZ

! Management access to router itself
zone-pair security ZP-INSIDE-SELF source INSIDE destination self
 service-policy type inspect PM-MGMT-TO-SELF

! ── STEP 7: Verify ───────────────────────────────────────────
! show zone security
! show zone-pair security
! show policy-map type inspect zone-pair
! show class-map type inspect
```

---

# 12. Router — IOS IPS (Snort/UTD)

## 12A — Classic IOS IPS

```
! ════════════════════════════════════════════════════════════
! Basic IPS on ISR routers using IOS IPS engine
! ════════════════════════════════════════════════════════════

! ── STEP 1: Create IPS directory ─────────────────────────────
mkdir flash:/ipsdir

! ── STEP 2: Configure IPS storage and notifications ──────────
ip ips config location flash:/ipsdir
ip ips notify log              ! Send IPS events to syslog
ip ips notify sdee             ! Alternatively, use SDEE

! ── STEP 3: Create IPS rule ──────────────────────────────────
ip ips name IOS-IPS-RULE

! ── STEP 4: Signature categories ─────────────────────────────
ip ips signature-category
 category all
  retired true                 ! Disable all signatures first (performance)
 category ios_ips basic
  retired false                ! Enable only the basic, curated category
 exit

! ── STEP 5: Apply IPS rule to interface ──────────────────────
interface GigabitEthernet0/1   ! Apply on external-facing interface
 ip ips IOS-IPS-RULE in        ! Inspect inbound traffic

! ── STEP 6: Verify ───────────────────────────────────────────
! show ip ips all
! show ip ips configuration
! show ip ips signature count
! show ip ips statistics
! show ip ips interfaces
```

## 12B — Snort IPS (UTD on IOS-XE)

```
! ════════════════════════════════════════════════════════════
! Unified Threat Defence (UTD) with Snort on IOS-XE ISR
! Requires UTD container to be installed and activated
! ════════════════════════════════════════════════════════════

! ── Verify UTD service availability ──────────────────────────
! show virtual-service list

! ── Configure UTD engine ─────────────────────────────────────
utd engine standard
 threat-inspection
  policy security              ! security|balanced|connectivity
  logging syslog               ! Log to syslog
 web-filter
  logging syslog

! ── Signature updates ────────────────────────────────────────
utd engine standard
 signature update server cisco
 signature update occur-at daily 3 0           ! Daily at 03:00

! ── Enable UTD on interfaces ─────────────────────────────────
interface GigabitEthernet0/0/1
 utd enable

! ── Verify ───────────────────────────────────────────────────
! show utd engine standard status
! show utd engine standard threat-inspection summary
! show utd engine standard statistics
```

---

# 13. Router — Site-to-Site IPsec VPN

```
! ════════════════════════════════════════════════════════════
! Complete site-to-site IPsec VPN configuration
! Both peers must have MATCHING Phase 1 and Phase 2 settings
!
! Topology:
!   R1 (192.168.1.0/24) ──[Internet]── R2 (10.0.0.0/24)
!   R1 outside: 203.0.113.1
!   R2 outside: 203.0.113.2
! ════════════════════════════════════════════════════════════

! ══════════════════════════════════
! R1 CONFIGURATION
! ══════════════════════════════════

hostname R1
ip domain-name company.com

! ── PHASE 1 — ISAKMP Policy ──────────────────────────────────
! Establishes the secure channel between peers
! Both sides must have identical policy parameters

crypto isakmp policy 10
 encryption aes 256             ! Encryption algorithm for IKE channel
 hash sha256                    ! Hash/integrity algorithm
 authentication pre-share       ! Use pre-shared key for authentication
 group 14                       ! Diffie-Hellman group 14 (2048-bit)
 lifetime 86400                 ! IKE SA lifetime = 24 hours

! ── Pre-shared key for peer R2 ───────────────────────────────
crypto isakmp key VPNSecret@Key123 address 203.0.113.2
! Key must be identical on both peers

! ── PHASE 2 — IPsec Transform Set ────────────────────────────
! Defines actual encryption applied to user data
crypto ipsec transform-set TS-AES256-SHA256 esp-aes 256 esp-sha256-hmac
 mode tunnel                    ! Tunnel mode encapsulates entire IP packet

! ── IPsec SA Lifetime ────────────────────────────────────────
crypto ipsec security-association lifetime seconds 3600    ! Phase 2 = 1 hour

! ── Interesting Traffic ACL ──────────────────────────────────
! Defines which traffic gets encrypted
! Must be a MIRROR IMAGE on both peers
ip access-list extended VPN-INTERESTING-TRAFFIC
 remark Traffic between R1 LAN and R2 LAN = encrypt this
 permit ip 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255

! ── NAT Exemption ─────────────────────────────────────────────
! VPN traffic must NOT be translated before encryption
ip access-list extended NAT-EXEMPT
 deny ip 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255   ! Don't NAT VPN traffic
 permit ip 192.168.1.0 0.0.0.255 any                 ! NAT everything else

ip nat inside source list NAT-EXEMPT interface GigabitEthernet0/1 overload

! ── Crypto Map ───────────────────────────────────────────────
crypto map VPN-CRYPTOMAP 10 ipsec-isakmp
 set peer 203.0.113.2                     ! Remote peer IP
 set transform-set TS-AES256-SHA256       ! Phase 2 transform
 set pfs group14                          ! PFS: new DH exchange per Phase 2 SA
 match address VPN-INTERESTING-TRAFFIC    ! Which traffic to encrypt
 set security-association lifetime seconds 3600

! ── Apply to WAN interface ───────────────────────────────────
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip nat inside

interface GigabitEthernet0/1
 ip address 203.0.113.1 255.255.255.0
 ip nat outside
 crypto map VPN-CRYPTOMAP                 ! Apply crypto map here

! ── Static route to remote site ──────────────────────────────
ip route 10.0.0.0 255.255.255.0 203.0.113.2    ! Route to R2 LAN via R2 outside IP

! ══════════════════════════════════
! R2 CONFIGURATION (mirror of R1)
! ══════════════════════════════════
! (Same policy, mirrored ACL and peer address)

! crypto isakmp policy 10
!  encryption aes 256
!  hash sha256
!  authentication pre-share
!  group 14
!  lifetime 86400
! crypto isakmp key VPNSecret@Key123 address 203.0.113.1   ! R1's outside IP
! ...
! ip access-list extended VPN-INTERESTING-TRAFFIC
!  permit ip 10.0.0.0 0.0.0.255 192.168.1.0 0.0.0.255    ! MIRRORED source/dest
! ...
! crypto map VPN-CRYPTOMAP 10 ipsec-isakmp
!  set peer 203.0.113.1                                    ! R1's outside IP

! ── Verify VPN ───────────────────────────────────────────────
! show crypto isakmp sa                ! Phase 1: should show QM_IDLE
! show crypto ipsec sa                 ! Phase 2: check pkts encaps/decaps
! show crypto map
! show crypto isakmp policy
! ping 10.0.0.1 source 192.168.1.1    ! Trigger VPN with interesting traffic

! ── Troubleshoot ─────────────────────────────────────────────
! debug crypto isakmp                  ! Phase 1 issues
! debug crypto ipsec                   ! Phase 2 issues
! no debug all                         ! Stop all debugging
```

---

# 14. Switch — Baseline Hardening

```
! ════════════════════════════════════════════════════════════
! Apply to every switch before deployment
! ════════════════════════════════════════════════════════════

! ── Identity ─────────────────────────────────────────────────
hostname SW1
ip domain-name company.com

! ── Passwords ────────────────────────────────────────────────
enable secret Str0ng!Enable@SW!
service password-encryption
security passwords min-length 10

! ── Local users ──────────────────────────────────────────────
username admin privilege 15 secret Admin@SW!P@ss
username netops privilege 7 secret NetOps@SW!P@ss

! ── SSH access ───────────────────────────────────────────────
crypto key generate rsa modulus 2048
ip ssh version 2
ip ssh time-out 60
ip ssh authentication-retries 3

! ── Management VLAN interface (SVI) ──────────────────────────
! Use a dedicated management VLAN (not VLAN 1)
interface Vlan10
 ip address 10.10.10.50 255.255.255.0
 description Management SVI
 no shutdown

! ── Default gateway ──────────────────────────────────────────
ip default-gateway 10.10.10.1

! ── Console access ───────────────────────────────────────────
line console 0
 login local
 exec-timeout 5 0
 logging synchronous

! ── AUX (disable) ────────────────────────────────────────────
line aux 0
 no exec
 transport input none

! ── VTY — SSH only ───────────────────────────────────────────
line vty 0 15
 login local
 transport input ssh
 exec-timeout 5 0
 access-class MGMT-ACCESS in

! ── Restrict management to management subnet ─────────────────
ip access-list standard MGMT-ACCESS
 permit 10.10.10.0 0.0.0.255
 deny any log

! ── Login attack mitigation ───────────────────────────────────
login block-for 120 attempts 3 within 60
login on-failure log

! ── Banner ────────────────────────────────────────────────────
banner motd ^
=====================================================
  AUTHORISED ACCESS ONLY — Activity is monitored
=====================================================
^

! ── Disable unused services ───────────────────────────────────
no ip http server
no ip http secure-server
no cdp run              ! Re-enable selectively if VoIP phones present

! ── Disable unused ports ─────────────────────────────────────
interface range GigabitEthernet0/5 - 24
 shutdown
 switchport mode access
 switchport access vlan 999     ! Move unused ports to unused VLAN

! ── Disable VLAN 1 use ───────────────────────────────────────
! (All active traffic should be on non-default VLANs)
interface Vlan1
 no ip address
 shutdown

! ── Syslog ────────────────────────────────────────────────────
logging host 10.0.0.100
logging trap warnings
service timestamps log datetime msec

! ── NTP ────────────────────────────────────────────────────────
ntp server 10.0.0.10
ntp authenticate
ntp authentication-key 1 md5 NTPKey@123
ntp trusted-key 1

! ── Save ──────────────────────────────────────────────────────
copy running-config startup-config
```

---

# 15. Switch — Port Security

```
! ════════════════════════════════════════════════════════════
! Port Security limits MAC addresses per port
! Mitigates: MAC flooding, rogue device connection
! Must be on access ports (not trunk ports)
! ════════════════════════════════════════════════════════════

! ── Basic port security on a single port ─────────────────────
interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 10
 switchport port-security                         ! Enable port security
 switchport port-security maximum 1               ! Allow only 1 MAC address
 switchport port-security mac-address sticky      ! Dynamically learn and save MAC
 switchport port-security violation shutdown      ! Default: err-disable on violation

! ── Multiple MACs (e.g., IP phone + PC behind it) ────────────
interface GigabitEthernet0/2
 switchport mode access
 switchport access vlan 10
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict      ! Drop + log; don't shut port

! ── Static MAC address (known device) ────────────────────────
interface GigabitEthernet0/3
 switchport mode access
 switchport port-security maximum 1
 switchport port-security mac-address 0011.2233.4455    ! Hardcoded MAC
 switchport port-security violation protect              ! Drop silently; no log

! ── Apply to a range of access ports ─────────────────────────
interface range GigabitEthernet0/1 - 24
 switchport mode access
 switchport port-security
 switchport port-security maximum 2
 switchport port-security mac-address sticky
 switchport port-security violation restrict

! ── Violation modes summary ──────────────────────────────────
! protect  — drop frames from unknown MACs, no log, no counter increment
! restrict — drop frames, log, increment violation counter
! shutdown — (default) err-disable the port immediately

! ── Auto-recovery from err-disabled ports ─────────────────────
errdisable recovery cause psecure-violation
errdisable recovery interval 300           ! Recover after 5 minutes

! ── Verify ───────────────────────────────────────────────────
! show port-security
! show port-security interface GigabitEthernet0/1
! show port-security address
! show errdisable recovery
! show interfaces GigabitEthernet0/1 status    (shows err-disabled)
```

---

# 16. Switch — VLAN Security

```
! ════════════════════════════════════════════════════════════
! VLAN security: prevent VLAN hopping attacks
! Methods: switch spoofing (via DTP) and double tagging
! ════════════════════════════════════════════════════════════

! ── Configure access ports (no trunk negotiation) ────────────
interface range GigabitEthernet0/1 - 22
 switchport mode access            ! Force access mode — disable trunk negotiation
 switchport access vlan 10         ! Assign to appropriate VLAN
 switchport nonegotiate            ! Disable DTP entirely

! ── Configure trunk ports ────────────────────────────────────
interface GigabitEthernet0/24
 switchport mode trunk             ! Force trunk mode (no auto-negotiation)
 switchport trunk encapsulation dot1q
 switchport nonegotiate            ! Disable DTP on trunk too
 switchport trunk native vlan 999  ! Set native VLAN to unused VLAN (not VLAN 1)
 switchport trunk allowed vlan 10,20,30,40   ! Explicitly define allowed VLANs

! ── Create and name VLANs ─────────────────────────────────────
vlan 10
 name DATA
vlan 20
 name VOICE
vlan 30
 name MGMT
vlan 40
 name SERVERS
vlan 999
 name UNUSED-NATIVE      ! Dedicated unused VLAN for native

! ── Unused ports ──────────────────────────────────────────────
interface range GigabitEthernet0/23 - 24
 switchport mode access
 switchport access vlan 999        ! Move to unused VLAN
 shutdown                          ! Shut down if not in use

! ── Tag native VLAN (optional — both ends must support) ──────
vlan dot1q tag native

! ── Verify ───────────────────────────────────────────────────
! show vlan brief
! show interfaces trunk
! show interfaces GigabitEthernet0/24 switchport
```

---

# 17. Switch — DHCP Snooping

```
! ════════════════════════════════════════════════════════════
! DHCP Snooping prevents:
!   - DHCP Starvation (exhausting the address pool)
!   - Rogue DHCP servers (spoofed gateway/DNS)
!
! Trusted ports = uplinks toward DHCP server (router/core)
! Untrusted ports = all access ports (default)
! Builds binding table used by DAI and IP Source Guard
! ════════════════════════════════════════════════════════════

! ── Enable DHCP Snooping globally ────────────────────────────
ip dhcp snooping
ip dhcp snooping vlan 10,20,30        ! Enable on specific VLANs

! ── Option 82 (DHCP relay agent info) ────────────────────────
! In some environments, disable Option 82 insertion to avoid issues
no ip dhcp snooping information option

! ── Mark uplinks as trusted ──────────────────────────────────
interface GigabitEthernet0/24         ! Uplink to core/DHCP server
 ip dhcp snooping trust

interface GigabitEthernet0/23         ! Another uplink (redundant)
 ip dhcp snooping trust

! ── Rate-limit DHCP on untrusted access ports ─────────────────
! Prevents starvation attack; untrusted = all access ports by default
interface range GigabitEthernet0/1 - 22
 ip dhcp snooping limit rate 15       ! Max 15 DHCP packets/second per port

! ── Verify ───────────────────────────────────────────────────
! show ip dhcp snooping
! show ip dhcp snooping binding        ! Key: shows MAC→IP→port→VLAN table
! show ip dhcp snooping statistics     ! Shows drops and forwards per VLAN
```

---

# 18. Switch — Dynamic ARP Inspection (DAI)

```
! ════════════════════════════════════════════════════════════
! DAI validates ARP packets against the DHCP snooping binding table
! Prevents: ARP spoofing / ARP poisoning (MitM attacks)
!
! PREREQUISITE: DHCP Snooping must be enabled first
!               (DAI uses the snooping binding table)
!
! Trusted = uplinks only
! Untrusted = all access ports (default — all ARP packets checked)
! ════════════════════════════════════════════════════════════

! ── Enable DAI on VLANs ──────────────────────────────────────
ip arp inspection vlan 10,20,30

! ── Mark trusted interfaces (uplinks only) ───────────────────
interface GigabitEthernet0/24
 ip arp inspection trust

interface GigabitEthernet0/23
 ip arp inspection trust

! ── Rate-limit ARP on untrusted ports ────────────────────────
interface range GigabitEthernet0/1 - 22
 ip arp inspection limit rate 100     ! Max 100 ARP packets/second
 ip arp inspection limit rate 100 burst interval 1

! ── Additional ARP validation checks (optional) ──────────────
ip arp inspection validate src-mac    ! Validate sender MAC
ip arp inspection validate dst-mac    ! Validate target MAC
ip arp inspection validate ip         ! Validate IP addresses (no 0.0.0.0, broadcast)

! ── ARP ACL for static IPs (not in DHCP binding table) ───────
! Use when some devices have static IPs (servers, printers)
arp access-list STATIC-ARP-PERMIT
 permit ip host 192.168.1.10 mac host 0011.2233.4455    ! Static entry

ip arp inspection filter STATIC-ARP-PERMIT vlan 10

! ── Verify ───────────────────────────────────────────────────
! show ip arp inspection
! show ip arp inspection vlan 10
! show ip arp inspection statistics
! show ip arp inspection interfaces
```

---

# 19. Switch — IP Source Guard

```
! ════════════════════════════════════════════════════════════
! IP Source Guard prevents IP/MAC address spoofing on switch ports
! Filters traffic based on DHCP snooping binding table
!
! PREREQUISITE: DHCP Snooping must be enabled first
!
! Applied per-interface on untrusted access ports
! ════════════════════════════════════════════════════════════

! ── Enable IP Source Guard on access ports ───────────────────
interface GigabitEthernet0/1
 ip verify source                     ! Filter by IP only (from binding table)

interface GigabitEthernet0/2
 ip verify source port-security       ! Filter by both IP AND MAC address

! ── For static IP devices (no DHCP binding entry) ────────────
! Add static binding manually
ip source binding 0011.2233.4455 vlan 10 192.168.10.50 interface GigabitEthernet0/5

! ── Verify ───────────────────────────────────────────────────
! show ip verify source
! show ip source binding               ! Static + dynamic binding entries
! show ip dhcp snooping binding        ! Dynamic entries from DHCP snooping
```

---

# 20. Switch — STP Security

```
! ════════════════════════════════════════════════════════════
! STP security features protect against STP manipulation attacks
!
! PortFast: Skip listen/learn states on access ports
! BPDU Guard: Shut port if BPDU received (prevents rogue bridges)
! Root Guard: Prevent port from becoming root port
! Loop Guard: Detect unidirectional link failures
! ════════════════════════════════════════════════════════════

! ── PortFast on access ports (speeds up host connections) ─────
interface range GigabitEthernet0/1 - 22
 spanning-tree portfast               ! Access ports only — never on trunk ports!

! ── Enable PortFast globally for all access ports ─────────────
spanning-tree portfast default        ! Applies to all non-trunking ports

! ── BPDU Guard — shut port if BPDU received ──────────────────
! Apply to PortFast ports — hosts should never send BPDUs
interface range GigabitEthernet0/1 - 22
 spanning-tree bpduguard enable

! ── Enable BPDU Guard globally (all PortFast ports) ──────────
spanning-tree portfast bpduguard default

! ── BPDU Filter — suppress BPDUs (use with caution) ──────────
! Use when you want to stop BPDUs being sent but don't want port shutdown
interface GigabitEthernet0/1
 spanning-tree bpdufilter enable

! ── Root Guard — prevent this port becoming a root port ───────
! Apply on ports where you will NEVER expect a superior BPDU
! (typically downlink ports facing access switches, never uplinks to core)
interface GigabitEthernet0/23
 spanning-tree guard root

! ── Loop Guard — detect unidirectional link failures ──────────
interface GigabitEthernet0/24
 spanning-tree guard loop

! ── Enable Loop Guard globally ────────────────────────────────
spanning-tree loopguard default

! ── Set this switch as root bridge (primary and secondary) ────
spanning-tree vlan 10,20,30 root primary         ! Lower priority to 24576
spanning-tree vlan 10,20,30 root secondary       ! Priority 28672
! Or manually:
spanning-tree vlan 10 priority 4096

! ── RSTP (Rapid STP) — enable for faster convergence ─────────
spanning-tree mode rapid-pvst              ! Default on modern IOS

! ── Recovery from err-disabled (BPDU Guard violation) ────────
errdisable recovery cause bpduguard
errdisable recovery interval 300

! ── Verify ───────────────────────────────────────────────────
! show spanning-tree
! show spanning-tree vlan 10
! show spanning-tree detail
! show spanning-tree inconsistentports
! show errdisable recovery
```

---

# 21. Switch — 802.1X Port-Based NAC

```
! ════════════════════════════════════════════════════════════
! 802.1X enforces authentication BEFORE network access is granted
! Supplicant (PC) → Authenticator (switch) → Auth Server (RADIUS/ISE)
!
! Port states:
!   auto         = require 802.1X authentication (recommended)
!   force-auth   = always permit (bypass 802.1X)
!   force-unauth = always block
! ════════════════════════════════════════════════════════════

! ── STEP 1: AAA and RADIUS server ────────────────────────────
aaa new-model

radius server ISE-SERVER
 address ipv4 10.0.0.50 auth-port 1812 acct-port 1813
 key RADIUS@SharedKey!

aaa group server radius RADIUS-GROUP
 server name ISE-SERVER

! ── STEP 2: AAA method lists for 802.1X ──────────────────────
aaa authentication dot1x default group RADIUS-GROUP
aaa authorization network default group RADIUS-GROUP
aaa accounting dot1x default start-stop group RADIUS-GROUP

! ── STEP 3: Enable 802.1X globally ───────────────────────────
dot1x system-auth-control

! ── STEP 4: Configure access ports ───────────────────────────
interface GigabitEthernet0/1
 switchport mode access
 switchport access vlan 10
 spanning-tree portfast                    ! Required for 802.1X on access ports
 dot1x pae authenticator                  ! This port is the authenticator
 authentication port-control auto         ! Require authentication

! ── STEP 5: Guest VLAN (optional — for non-802.1X devices) ───
interface GigabitEthernet0/1
 authentication event fail action authorize vlan 30   ! Auth failed → guest VLAN 30
 authentication event no-response action authorize vlan 30  ! No client → guest VLAN

! ── STEP 6: Critical VLAN (fallback if RADIUS unreachable) ───
interface GigabitEthernet0/1
 authentication event server dead action authorize vlan 20   ! Server dead → VLAN 20
 authentication event server alive action reinitialize        ! Re-auth when server back

! ── Apply 802.1X to range ────────────────────────────────────
interface range GigabitEthernet0/1 - 22
 switchport mode access
 spanning-tree portfast
 dot1x pae authenticator
 authentication port-control auto

! ── Verify ───────────────────────────────────────────────────
! show dot1x all
! show dot1x interface GigabitEthernet0/1
! show authentication sessions
! show authentication sessions interface GigabitEthernet0/1
```

---

# 22. Switch — SPAN / RSPAN

```
! ════════════════════════════════════════════════════════════
! SPAN = copy traffic to a monitoring port (IDS, Wireshark)
! RSPAN = copy traffic to a remote switch over a trunk
! The destination port is for monitoring only — not data traffic
! ════════════════════════════════════════════════════════════

! ── Local SPAN — single interface ────────────────────────────
! Copy all traffic (both directions) from Gi0/1 to Gi0/10 (where IDS is)
monitor session 1 source interface GigabitEthernet0/1 both
monitor session 1 destination interface GigabitEthernet0/10

! ── Local SPAN — capture inbound only ────────────────────────
monitor session 2 source interface GigabitEthernet0/2 rx

! ── Local SPAN — monitor entire VLAN ─────────────────────────
monitor session 3 source vlan 10 both
monitor session 3 destination interface GigabitEthernet0/10

! ── Remove a SPAN session ────────────────────────────────────
no monitor session 1

! ── RSPAN — mirror traffic to another switch ─────────────────
! On source switch (SW1):
vlan 500
 name RSPAN-VLAN
 remote-span                          ! Mark as RSPAN VLAN

monitor session 4 source interface GigabitEthernet0/1 both
monitor session 4 destination remote vlan 500

! On destination switch (SW2):
vlan 500
 name RSPAN-VLAN
 remote-span

monitor session 4 source remote vlan 500
monitor session 4 destination interface GigabitEthernet0/10   ! Connect IDS here

! ── Verify ───────────────────────────────────────────────────
! show monitor
! show monitor session 1
! show monitor session all
```

---

# 23. ASA — Baseline & Interface Configuration

```
! ════════════════════════════════════════════════════════════
! ASA 5506-X Baseline Configuration
!
! Security levels: 0=untrusted (outside) to 100=fully trusted (inside)
! Traffic from HIGH→LOW: permitted by default (stateful return auto-allowed)
! Traffic from LOW→HIGH: DENIED by default (requires explicit ACL)
! Traffic between EQUAL levels: denied by default
!
! nameif: logical name for interface — all config uses this name
! ════════════════════════════════════════════════════════════

! ── Identity ─────────────────────────────────────────────────
hostname ASA-FW
domain-name company.com

! ── Interface configuration ──────────────────────────────────
interface GigabitEthernet1/1
 nameif outside                       ! Logical name (used everywhere in config)
 security-level 0                     ! Least trusted
 ip address 203.0.113.1 255.255.255.0
 description Connected to Internet
 no shutdown

interface GigabitEthernet1/2
 nameif inside
 security-level 100                   ! Most trusted
 ip address 192.168.1.1 255.255.255.0
 description Internal LAN
 no shutdown

interface GigabitEthernet1/3
 nameif dmz
 security-level 50                    ! Semi-trusted
 ip address 172.16.1.1 255.255.255.0
 description DMZ - Public-facing servers
 no shutdown

interface GigabitEthernet1/4
 nameif servers
 security-level 75
 ip address 10.0.1.1 255.255.255.0
 description Internal server segment
 no shutdown

! ── Management interface ─────────────────────────────────────
interface Management1/1
 nameif management
 security-level 100
 ip address 10.10.10.1 255.255.255.0
 management-only                      ! Only management traffic; no transit
 no shutdown

! ── Routing ───────────────────────────────────────────────────
route outside 0.0.0.0 0.0.0.0 203.0.113.254 1   ! Default route to ISP
route inside 10.0.0.0 255.0.0.0 192.168.1.254    ! Internal summary route

! ── DNS ───────────────────────────────────────────────────────
dns server-group DefaultDNS
 name-server 8.8.8.8
 name-server 1.1.1.1
 domain-name company.com

! ── Allow traffic between same-security-level interfaces ──────
! (disabled by default — only enable if needed)
! same-security-traffic permit inter-interface

! ── Verify ───────────────────────────────────────────────────
! show interface ip brief
! show nameif
! show route
```

---

# 24. ASA — Management & SSH/ASDM Access

```
! ════════════════════════════════════════════════════════════
! Securing management access to the ASA
! ════════════════════════════════════════════════════════════

! ── Local admin user ──────────────────────────────────────────
username admin password Admin@ASA!P@ss privilege 15
username auditor password Auditor@ASA! privilege 5

! ── SSH access ───────────────────────────────────────────────
crypto key generate rsa modulus 2048
ssh version 2
ssh 192.168.1.0 255.255.255.0 inside          ! Allow SSH from inside subnet
ssh 10.10.10.0 255.255.255.0 management       ! Allow SSH from mgmt subnet
ssh timeout 5                                  ! Disconnect idle SSH after 5 min

! ── Enable ASDM (HTTPS management GUI) ───────────────────────
http server enable
http 192.168.1.0 255.255.255.0 inside         ! Allow HTTPS from inside
http 10.10.10.0 255.255.255.0 management      ! Allow from mgmt subnet

! ── Authentication for management access ──────────────────────
aaa authentication ssh console LOCAL           ! SSH: use local user database
aaa authentication http console LOCAL          ! ASDM: use local users
aaa authentication serial console LOCAL        ! Console: use local users
! Or use TACACS+/RADIUS server:
! aaa authentication ssh console TACACS-GROUP LOCAL
! aaa authentication http console TACACS-GROUP LOCAL

! ── Console timeout ───────────────────────────────────────────
console timeout 5

! ── Legal banner ──────────────────────────────────────────────
banner asdm ^
======================================
  AUTHORISED ACCESS ONLY
  All activity is monitored and logged
======================================
^

banner login ^
  AUTHORISED ACCESS ONLY
^

! ── Disable Telnet (should already be disabled by default) ────
! (Avoid Telnet entirely — use SSH)

! ── NTP ────────────────────────────────────────────────────────
ntp server 10.0.0.10 source inside
ntp authenticate
ntp authentication-key 1 md5 NTPKey@123
ntp trusted-key 1

! ── Syslog ─────────────────────────────────────────────────────
logging enable
logging host inside 10.0.0.100
logging trap warnings
logging timestamp
service timestamps log datetime msec

! ── Verify ───────────────────────────────────────────────────
! show ssh
! show http
! show aaa
! show logging
```

---

# 25. ASA — AAA Configuration

```
! ════════════════════════════════════════════════════════════
! AAA on ASA for both admin access and VPN user authentication
! ════════════════════════════════════════════════════════════

! ── Define TACACS+ server (for device admin) ─────────────────
aaa-server TACACS-GROUP protocol tacacs+
aaa-server TACACS-GROUP (inside) host 10.0.0.50
 key TacacsSharedKey@123
 timeout 5

! ── Define RADIUS server (for VPN/network access) ────────────
aaa-server RADIUS-GROUP protocol radius
aaa-server RADIUS-GROUP (inside) host 10.0.0.52
 key RadiusSharedKey@123
 authentication-port 1812
 accounting-port 1813

! ── Admin access authentication ───────────────────────────────
aaa authentication ssh console TACACS-GROUP LOCAL       ! SSH: TACACS+ then local
aaa authentication http console TACACS-GROUP LOCAL      ! ASDM: TACACS+ then local
aaa authentication serial console LOCAL                  ! Console: local only

! ── Command authorisation ─────────────────────────────────────
aaa authorization command TACACS-GROUP LOCAL
aaa authorization exec authentication-server

! ── Accounting ────────────────────────────────────────────────
aaa accounting ssh console TACACS-GROUP
aaa accounting command TACACS-GROUP

! ── VPN authentication (uses RADIUS) ─────────────────────────
! (Configured in tunnel-group — see VPN sections below)

! ── Verify ───────────────────────────────────────────────────
! show aaa-server
! show aaa-server TACACS-GROUP
! test aaa-server authentication TACACS-GROUP host 10.0.0.50 username admin password testpass
```

---

# 26. ASA — Object Groups & ACLs

```
! ════════════════════════════════════════════════════════════
! KEY DIFFERENCES from IOS ACLs:
!   1. Named ACLs only (no numbered)
!   2. Subnet masks used (NOT wildcard masks)
!      e.g., 255.255.255.0 NOT 0.0.0.255
!   3. Applied with 'access-group' to interface
!   4. High→Low traffic permitted by default (no ACL needed)
!   5. Low→High traffic denied by default (requires ACL)
! ════════════════════════════════════════════════════════════

! ── Network objects ───────────────────────────────────────────
object network WEB-SERVER
 host 172.16.1.10
 description Primary web server in DMZ

object network MAIL-SERVER
 host 172.16.1.20
 description Mail server in DMZ

object network INTERNAL-LAN
 subnet 192.168.1.0 255.255.255.0
 description Internal LAN

object network DMZ-SUBNET
 subnet 172.16.1.0 255.255.255.0
 description DMZ network

! ── Object groups (collection of objects) ─────────────────────
object-group network DMZ-SERVERS
 description All DMZ servers
 network-object host 172.16.1.10
 network-object host 172.16.1.20
 network-object host 172.16.1.30

! ── Service objects ───────────────────────────────────────────
object service WEB-HTTP
 service tcp destination eq 80

object service WEB-HTTPS
 service tcp destination eq 443

! ── Service object groups ──────────────────────────────────────
object-group service WEB-SERVICES tcp
 port-object eq 80
 port-object eq 443
 port-object eq 8080

object-group service MAIL-SERVICES tcp
 port-object eq 25          ! SMTP
 port-object eq 587         ! STARTTLS
 port-object eq 993         ! IMAPS
 port-object eq 995         ! POP3S

object-group service MGMT-SERVICES tcp
 port-object eq 22           ! SSH
 port-object eq 3389         ! RDP (restrict to VPN only)

! ── ACLs ─────────────────────────────────────────────────────
! REMEMBER: ASA uses subnet masks, not wildcard masks!

! Allow web access to DMZ servers from Internet
access-list OUTSIDE-IN extended permit tcp any object-group DMZ-SERVERS object-group WEB-SERVICES
access-list OUTSIDE-IN extended permit tcp any object MAIL-SERVER object-group MAIL-SERVICES
access-list OUTSIDE-IN extended deny ip any any log

! Restrict DMZ to only connect to specific internal resources
access-list DMZ-TO-INSIDE extended permit tcp object DMZ-SUBNET 192.168.1.0 255.255.255.0 eq 1433   ! SQL
access-list DMZ-TO-INSIDE extended permit tcp object DMZ-SUBNET 192.168.1.0 255.255.255.0 eq 3306   ! MySQL
access-list DMZ-TO-INSIDE extended deny ip any any log

! Apply ACLs to interfaces
access-group OUTSIDE-IN in interface outside
access-group DMZ-TO-INSIDE in interface dmz

! ── Verify ───────────────────────────────────────────────────
! show access-list
! show access-list OUTSIDE-IN
! show access-group
! packet-tracer input outside tcp 1.2.3.4 12345 172.16.1.10 80 detailed
```

---

# 27. ASA — NAT (Object NAT & Twice NAT)

```
! ════════════════════════════════════════════════════════════
! ASA NAT translates IP addresses as traffic passes through
!
! Object NAT (Auto NAT): defined within network objects — simpler
! Twice NAT (Manual NAT): standalone, more flexible
!
! NAT order: Twice NAT > Object NAT
! Within same type: more specific = higher priority
! ════════════════════════════════════════════════════════════

! ── OBJECT NAT ───────────────────────────────────────────────

! Dynamic PAT: Translate all inside hosts to outside interface IP
! (Many-to-one; classic "overload" PAT)
object network INSIDE-HOSTS
 subnet 192.168.1.0 255.255.255.0
 nat (inside,outside) dynamic interface   ! 'interface' = use outside IP

! Dynamic PAT to a pool of addresses (if outside IP is not enough)
object network PAT-POOL
 range 203.0.113.100 203.0.113.110        ! Pool of public IPs for PAT
object network INSIDE-HOSTS
 subnet 192.168.1.0 255.255.255.0
 nat (inside,outside) dynamic PAT-POOL

! Static NAT: Fixed translation for DMZ web server
! External users reach 203.0.113.10 → ASA translates → 172.16.1.10
object network DMZ-WEB-SERVER
 host 172.16.1.10
 nat (dmz,outside) static 203.0.113.10

! Static NAT for mail server
object network DMZ-MAIL-SERVER
 host 172.16.1.20
 nat (dmz,outside) static 203.0.113.20

! Static PAT: Map a single public IP+port to an internal server
! (Port forwarding — e.g., external port 8443 → internal HTTPS)
object network INT-MGMT-SERVER
 host 192.168.1.100
 nat (inside,outside) static 203.0.113.50 service tcp 8443 443

! ── TWICE NAT (Manual NAT) ────────────────────────────────────

! Dynamic PAT with twice NAT (equivalent to object NAT dynamic above)
nat (inside,outside) source dynamic INSIDE-HOSTS interface

! NAT exemption for VPN traffic (no translation for VPN)
! This is critical — VPN traffic must NOT be NATted
nat (inside,outside) source static INSIDE-HOSTS INSIDE-HOSTS destination static REMOTE-VPN REMOTE-VPN
! Where REMOTE-VPN is a network object for the remote VPN site

! ── Verify ───────────────────────────────────────────────────
! show nat
! show nat detail
! show xlate                    ! Active translation table
! show conn                     ! Active connection table
! packet-tracer input inside tcp 192.168.1.10 12345 8.8.8.8 80 detailed
```

---

# 28. ASA — Service Policies (MPF)

```
! ════════════════════════════════════════════════════════════
! ASA Modular Policy Framework (MPF) for application inspection
! Same structure as IOS QoS: class-map → policy-map → service-policy
!
! Used for: deep packet inspection, rate limiting, QoS
! ════════════════════════════════════════════════════════════

! ── Default global inspection policy ─────────────────────────
! (Already present by default — shown here for reference)
class-map inspection_default
 match default-inspection-traffic    ! Matches well-known protocols

policy-map global_policy
 class inspection_default
  inspect dns                        ! DNS inspection
  inspect ftp                        ! FTP inspection (fixes NAT issues with FTP)
  inspect http                       ! HTTP inspection
  inspect icmp                       ! ICMP inspection
  inspect icmp error                 ! ICMP error message inspection
  inspect smtp                       ! SMTP inspection
  inspect tftp
  inspect netbios
  inspect sunrpc

service-policy global_policy global  ! Apply globally to all interfaces

! ── HTTP application inspection ───────────────────────────────
! Block downloads of executable files
regex BLOCK-EXE ".*\.exe$"
regex BLOCK-BAT ".*\.bat$"

class-map type regex match-any BLOCK-EXECUTABLES
 match regex BLOCK-EXE
 match regex BLOCK-BAT

policy-map type inspect http HTTP-INSPECTION
 parameters
  protocol-violation action drop-connection
 class BLOCK-EXECUTABLES
  drop-connection log

! Add HTTP inspection with custom policy to global policy
policy-map global_policy
 class inspection_default
  inspect http HTTP-INSPECTION

! ── Rate limiting (police) specific traffic ───────────────────
class-map BULK-DATA
 match access-list BULK-TRAFFIC-ACL

policy-map RATE-LIMIT-POLICY
 class BULK-DATA
  police output 10000000    ! Rate limit to 10 Mbps

service-policy RATE-LIMIT-POLICY interface outside

! ── Verify ───────────────────────────────────────────────────
! show service-policy global
! show service-policy interface outside
! show service-policy | include drop
```

---

# 29. ASA — Site-to-Site IPsec VPN

```
! ════════════════════════════════════════════════════════════
! ASA Site-to-Site IPsec VPN
!
! Topology:
!   ASA (192.168.1.0/24) ──[Internet]── Remote Router (10.0.0.0/24)
!   ASA outside: 203.0.113.1
!   Remote outside: 203.0.113.2
! ════════════════════════════════════════════════════════════

! ── PHASE 1 — IKEv1 Policy ───────────────────────────────────
crypto ikev1 policy 10
 authentication pre-share
 encryption aes-256
 hash sha
 group 14
 lifetime 86400

! Enable IKEv1 on outside interface
crypto ikev1 enable outside

! ── Pre-shared key via Tunnel Group ──────────────────────────
tunnel-group 203.0.113.2 type ipsec-l2l              ! l2l = LAN-to-LAN
tunnel-group 203.0.113.2 ipsec-attributes
 ikev1 pre-shared-key VPNSecret@Key123

! ── PHASE 2 — Transform Set ──────────────────────────────────
crypto ipsec ikev1 transform-set TS-AES256 esp-aes-256 esp-sha-hmac

! ── Interesting Traffic ACL ──────────────────────────────────
! NOTE: ASA uses subnet masks, not wildcard masks
access-list VPN-TRAFFIC extended permit ip 192.168.1.0 255.255.255.0 10.0.0.0 255.255.255.0

! ── Crypto Map ───────────────────────────────────────────────
crypto map VPN-MAP 10 match address VPN-TRAFFIC
crypto map VPN-MAP 10 set peer 203.0.113.2
crypto map VPN-MAP 10 set ikev1 transform-set TS-AES256
crypto map VPN-MAP 10 set pfs group14

! Apply to outside interface
crypto map VPN-MAP interface outside

! ── NAT Exemption (Twice NAT) ─────────────────────────────────
! VPN traffic must NOT be NATted
object network LOCAL-LAN
 subnet 192.168.1.0 255.255.255.0

object network REMOTE-LAN
 subnet 10.0.0.0 255.255.255.0

nat (inside,outside) source static LOCAL-LAN LOCAL-LAN destination static REMOTE-LAN REMOTE-LAN
! This says: don't translate traffic from LOCAL-LAN going to REMOTE-LAN

! ── Verify ───────────────────────────────────────────────────
! show crypto ikev1 sa
! show crypto ipsec sa
! show crypto map
! show vpn-sessiondb l2l
! debug crypto ikev1
! debug crypto ipsec
```

---

# 30. ASA — AnyConnect SSL VPN

```
! ════════════════════════════════════════════════════════════
! AnyConnect SSL VPN for remote user access
! Uses TLS (TCP 443) or DTLS (UDP 443)
! Works through almost any firewall/NAT
! ════════════════════════════════════════════════════════════

! ── STEP 1: Upload AnyConnect image to flash ─────────────────
! (Done via ASDM or: copy tftp://server/anyconnect.pkg flash:)

! ── STEP 2: Enable WebVPN ────────────────────────────────────
webvpn
 enable outside                              ! Enable SSL VPN on outside interface
 anyconnect image disk0:/anyconnect-win-4.10.07073-k9.pkg 1
 anyconnect image disk0:/anyconnect-macos-4.10.07073-k9.pkg 2
 anyconnect enable
 tunnel-group-list enable                    ! Show tunnel group list to users
 cache
  disable                                    ! Disable caching for security
 error-recovery disable

! ── STEP 3: IP address pool for VPN clients ──────────────────
ip local pool VPN-POOL 10.20.20.1-10.20.20.254 mask 255.255.255.0

! ── STEP 4: Group policy ─────────────────────────────────────
group-policy ANYCONNECT-POLICY internal
group-policy ANYCONNECT-POLICY attributes
 vpn-tunnel-protocol ssl-client              ! SSL/TLS tunnel
 split-tunnel-policy tunnelall               ! Route ALL traffic through VPN
 ! For split tunneling (only corporate traffic over VPN):
 ! split-tunnel-policy tunnelspecified
 ! split-tunnel-network-list value SPLIT-TUNNEL-ACL
 dns-server value 192.168.1.53              ! Push internal DNS to clients
 wins-server none
 default-domain value company.com           ! Push domain suffix
 vpn-idle-timeout 30                        ! Disconnect idle after 30 min
 vpn-session-timeout 480                    ! Max session 8 hours
 address-pools value VPN-POOL

! ── STEP 5: Tunnel group (Connection profile) ────────────────
tunnel-group ANYCONNECT-PROFILE type remote-access
tunnel-group ANYCONNECT-PROFILE general-attributes
 address-pool VPN-POOL
 authentication-server-group RADIUS-GROUP   ! Authenticate against RADIUS
 default-group-policy ANYCONNECT-POLICY
tunnel-group ANYCONNECT-PROFILE webvpn-attributes
 group-alias "Company VPN" enable           ! Name shown in AnyConnect client

! ── STEP 6: NAT Exemption for VPN traffic ────────────────────
object network VPN-POOL-RANGE
 subnet 10.20.20.0 255.255.255.0

object network INTERNAL-LAN
 subnet 192.168.1.0 255.255.255.0

nat (outside,inside) source static VPN-POOL-RANGE VPN-POOL-RANGE destination static INTERNAL-LAN INTERNAL-LAN

! ── STEP 7: Allow VPN traffic through ASA ACL ────────────────
access-list OUTSIDE-IN extended permit ip 10.20.20.0 255.255.255.0 192.168.1.0 255.255.255.0

! ── Verify ───────────────────────────────────────────────────
! show vpn-sessiondb anyconnect
! show vpn-sessiondb summary
! show webvpn anyconnect
! show crypto ssl
```

---

# 31. Verification & Troubleshooting Commands

## Router Verification

```
! ── Device & Access ──────────────────────────────────────────
show version                          ! IOS version, uptime, interfaces
show running-config                   ! Current active config
show startup-config                   ! Saved config
show users                            ! Currently logged-in users
show privilege                        ! Current privilege level
show parser view                      ! Current CLI view
show login                            ! Login block status
show login failures                   ! Recent failed logins
show ip ssh                           ! SSH configuration
show ssh                              ! Active SSH sessions

! ── AAA ──────────────────────────────────────────────────────
show aaa servers                      ! All configured AAA servers
show aaa sessions                     ! Active AAA sessions
show tacacs                           ! TACACS+ server stats
show aaa local user lockout           ! Locked out local users

! ── ACLs ─────────────────────────────────────────────────────
show ip access-lists                  ! All ACLs with hit counters
show ip access-lists <name>           ! Specific ACL
show ip interface <int>               ! See which ACLs are applied
clear ip access-list counters         ! Reset all counters

! ── ZPF ──────────────────────────────────────────────────────
show zone security                    ! Zones and their interfaces
show zone-pair security               ! Zone-pairs and policies
show policy-map type inspect zone-pair ! Policy stats with drop counts

! ── Routing Protocol Auth ────────────────────────────────────
show ip ospf interface <int>          ! OSPF auth config per interface
show ip ospf neighbor                 ! OSPF neighbours (check adjacency)
show key chain                        ! EIGRP/RIP key chains

! ── IPsec VPN ────────────────────────────────────────────────
show crypto isakmp sa                 ! Phase 1 SA (QM_IDLE = up)
show crypto ipsec sa                  ! Phase 2 SA (pkts encaps/decaps)
show crypto map                       ! Crypto map config
show crypto isakmp policy             ! IKE policies

! ── IPS ──────────────────────────────────────────────────────
show ip ips all                       ! IOS IPS config summary
show ip ips statistics                ! Signature hit stats
show utd engine standard status       ! Snort UTD status

! ── Monitoring ───────────────────────────────────────────────
show logging                          ! Syslog buffer contents
show ntp status                       ! NTP sync status
show ntp associations                 ! NTP peers
show snmp                             ! SNMP statistics
show ip cache flow                    ! NetFlow active flows

! ── Debug (use with caution in production) ───────────────────
debug crypto isakmp                   ! VPN Phase 1 negotiation
debug crypto ipsec                    ! VPN Phase 2 negotiation
debug aaa authentication              ! AAA auth troubleshooting
debug ip ospf adj                     ! OSPF adjacency issues
no debug all                          ! CRITICAL: Stop all debugging
```

## Switch Verification

```
! ── VLAN & Port ──────────────────────────────────────────────
show vlan brief                       ! All VLANs and ports
show interfaces trunk                 ! Trunk ports and allowed VLANs
show interfaces <int> switchport      ! Port mode, VLAN, DTP status

! ── Port Security ────────────────────────────────────────────
show port-security                    ! Summary of all port security
show port-security interface Gi0/1    ! Detail for one port
show port-security address            ! All secured MAC addresses
show errdisable recovery              ! Recovery config
show interfaces Gi0/1 status          ! Check for err-disabled

! ── DHCP Snooping ────────────────────────────────────────────
show ip dhcp snooping                 ! Config and enabled VLANs
show ip dhcp snooping binding         ! MAC→IP→port binding table
show ip dhcp snooping statistics      ! Drop and forward counters

! ── Dynamic ARP Inspection ───────────────────────────────────
show ip arp inspection                ! Config and enabled VLANs
show ip arp inspection vlan 10        ! Per-VLAN stats
show ip arp inspection statistics     ! Drop/forward counters

! ── IP Source Guard ──────────────────────────────────────────
show ip verify source                 ! Per-port IPSG config
show ip source binding                ! All source bindings

! ── STP ──────────────────────────────────────────────────────
show spanning-tree                    ! STP overview, root info
show spanning-tree vlan 10            ! Per-VLAN STP
show spanning-tree detail             ! Full detail per port
show spanning-tree inconsistentports  ! Ports in inconsistent state

! ── 802.1X ───────────────────────────────────────────────────
show dot1x all                        ! Global and per-port 802.1X
show dot1x interface Gi0/1            ! Single port detail
show authentication sessions          ! All auth sessions

! ── SPAN ─────────────────────────────────────────────────────
show monitor                          ! All SPAN sessions
show monitor session 1                ! Detail for session 1
```

## ASA Verification

```
! ── Interfaces & Connectivity ────────────────────────────────
show interface ip brief               ! All interfaces with IP/status
show nameif                           ! Interface names and security levels
show route                            ! Routing table
show conn                             ! Active connections
show conn count                       ! Connection count

! ── ACLs & Policy ────────────────────────────────────────────
show access-list                      ! All ACLs with counters
show access-list OUTSIDE-IN           ! Specific ACL
show access-group                     ! ACLs applied to interfaces
packet-tracer input outside tcp 1.2.3.4 1234 172.16.1.10 80 detailed
! packet-tracer simulates a packet and shows each step through ASA

! ── NAT ──────────────────────────────────────────────────────
show nat                              ! All NAT rules
show nat detail                       ! With hit counts
show xlate                            ! Active NAT translations

! ── VPN ──────────────────────────────────────────────────────
show crypto ikev1 sa                  ! IKEv1 Phase 1 SAs
show crypto ikev2 sa                  ! IKEv2 SAs
show crypto ipsec sa                  ! Phase 2 SAs
show vpn-sessiondb                    ! All active VPN sessions
show vpn-sessiondb l2l                ! Site-to-site sessions
show vpn-sessiondb anyconnect         ! AnyConnect sessions
show vpn-sessiondb summary            ! Session summary

! ── AAA & Users ──────────────────────────────────────────────
show aaa-server                       ! All AAA servers
show running-config aaa-server        ! AAA server config
show uauth                            ! Authenticated users (cut-through)

! ── System ───────────────────────────────────────────────────
show version                          ! ASA version and license
show failover                         ! HA failover status
show cpu usage                        ! CPU utilisation
show memory                           ! Memory usage
show logging                          ! Syslog buffer

! ── Troubleshooting ──────────────────────────────────────────
! packet-tracer = simulate traffic through ASA (best troubleshooting tool)
packet-tracer input inside tcp 192.168.1.10 12345 8.8.8.8 80 detailed

! capture = capture live packets
capture CAP1 interface outside match ip any any
show capture CAP1
no capture CAP1

debug crypto ikev1 255                ! VPN IKEv1 debug
debug crypto ipsec 255               ! VPN IPsec debug
undebug all                          ! Stop all debugging
```

---

> 🔐 **Cisco NetAcad Network Security 1.0 — Complete Configuration Reference**  
> Always test changes in a lab before applying to production.  
> Always have console access before making remote access changes.  
> Always save: `copy running-config startup-config`
