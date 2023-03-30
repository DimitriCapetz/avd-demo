# DC-LEAF4
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [Name Servers](#name-servers)
  - [Domain Lookup](#domain-lookup)
  - [Clock Settings](#clock-settings)
  - [NTP](#ntp)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
  - [Local Users](#local-users)
  - [TACACS Servers](#tacacs-servers)
  - [IP TACACS Source Interfaces](#ip-tacacs-source-interfaces)
  - [AAA Server Groups](#aaa-server-groups)
  - [AAA Authentication](#aaa-authentication)
  - [AAA Authorization](#aaa-authorization)
  - [AAA Accounting](#aaa-accounting)
- [Aliases](#aliases)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
  - [Logging](#logging)
  - [SNMP](#snmp)
  - [SFlow](#sflow)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [MAC Address Table](#mac-address-table)
  - [MAC Address Table Summary](#mac-address-table-summary)
  - [MAC Address Table Device Configuration](#mac-address-table-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [ARP](#arp)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
  - [Standard Access-lists](#standard-access-lists)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Virtual Source NAT](#virtual-source-nat)
  - [Virtual Source NAT Summary](#virtual-source-nat-summary)
  - [Virtual Source NAT Configuration](#virtual-source-nat-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | management | 10.99.99.183/24 | 10.99.99.1 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | management | - | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf management
   ip address 10.99.99.183/24
```

## DNS Domain

### DNS domain: dime-a-tron.com

### DNS Domain Device Configuration

```eos
dns domain dime-a-tron.com
!
```

## Name Servers

### Name Servers Summary

| Name Server | Source VRF |
| ----------- | ---------- |
| 1.1.1.1 | management |

### Name Servers Device Configuration

```eos
ip name-server vrf management 1.1.1.1
```

## Domain Lookup

### DNS Domain Lookup Summary

| Source interface | vrf |
| ---------------- | --- |
| Mangement1 | management |

### DNS Domain Lookup Device Configuration

```eos
ip domain lookup vrf management source-interface Mangement1
```

## Clock Settings

### Clock Timezone Settings

Clock Timezone is set to **US/Central**.

### Clock Configuration

```eos
!
clock timezone US/Central
```

## NTP

### NTP Summary

#### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management1 | management |

#### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 0.pool.ntp.org | management | - | False | True | - | - | - | Management1 | - |

#### NTP Authentication

### NTP Device Configuration

```eos
!
ntp local-interface vrf management Management1
ntp server vrf management 0.pool.ntp.org iburst local-interface Management1
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| management | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf management
      no shutdown
```

# Authentication

## Local Users

### Local Users Summary

| User | Privilege | Role | Disabled |
| ---- | --------- | ---- | -------- |
| admin | 15 | network-admin | False |
| dcapetz | 15 | network-admin | False |

### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 $6$cObeccKViX0iyttR$A.3Pb7XxO15mEowsBC8fUjcrt3E77viyGgoWroBJ6jh0ZppqqMhVHpR2xyDBxOOIJD0aXh.mYpZjz5F88rbR.0
username dcapetz privilege 15 role network-admin secret sha512 $6$MmPwehEOaBQNSZCO$eAg4JPnxxyqKVqVNYak76EoYiCjaJAFQVQSFVXqTzBKcA57ZbYpJt/1lIFtVwGOpnD4/OgSHMxfmyWIhbq7bB/
username dcapetz ssh-key ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDZ148VUrWGE12gbl/yuMn2tCKwaJTmayGqEZGseVCtWO54gB0J2vt/m75xY5dmHquZvLQVfgp7JfV/FUa+Ao+QhHFFxdq7Br+T8PBE8u64Mr97MWpUT3Kc79lL48MvJfNH/adQ8sRK2LkLDp2Lgfc8XQNsA28NkOOh0EV4iSWVCv9K0m19Hihp6cjfvAb8Jt9y5NvtZiJvsBmv+A6DJ5okEsmQ3qVgmEzJ2xWHigYwZOmJsW5XcS0+cqVVT28J/rE4d6QPds6kSx9gkp3/RJxxH9lwhzDQFoOmfMlz5TixG7a4cnvvxETmEfw+QKYcKcIbCZdTi8/1+FLLQ2SOVQlVFsbzRcfUa3Z+FfYk358H6AYQ5fY2R7pYHC8jIOjVyp4Q0KcCo29NzZ07eXr6Z0hmaB7cXTP2devB6VqPxnhLeBXIPgLs8JU/yJd7aneYEonn5vSnEBiC3E61l8Dx3pz0WBYsZHNfL+A/stCBdJRl35XAoGqnEfU73eaZiXx46CU= dcapetz@dcapetz
```

## TACACS Servers

### TACACS Servers

| VRF | TACACS Servers | Single-Connection |
| --- | -------------- | ----------------- |
| management | home-ise.dime-a-tron.com | False |

### TACACS Servers Device Configuration

```eos
!
tacacs-server host home-ise.dime-a-tron.com vrf management key 7 014354560C2F052B220D
```

## IP TACACS Source Interfaces

### IP TACACS Source Interfaces

| VRF | Source Interface Name |
| --- | --------------- |
| management | Management1 |

### IP TACACS Source Interfaces Device Configuration

```eos
!
ip tacacs vrf management source-interface Management1
```

## AAA Server Groups

### AAA Server Groups Summary

| Server Group Name | Type  | VRF | IP address |
| ------------------| ----- | --- | ---------- |
| home-ise | tacacs+ | management | home-ise.dime-a-tron.com |

### AAA Server Groups Device Configuration

```eos
!
aaa group server tacacs+ home-ise
   server home-ise.dime-a-tron.com vrf management
```

## AAA Authentication

### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | local |
| Login | console | local |

### AAA Authentication Device Configuration

```eos
aaa authentication login default local
aaa authentication login console local
!
```

## AAA Authorization

### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

### AAA Authorization Privilege Levels Summary

| Privilege Level | User Stores |
| --------------- | ----------- |
| all | local |

### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
aaa authorization commands all default local
!
```

## AAA Accounting

### AAA Accounting Summary

| Type | Commands | Record type | Group | Logging |
| ---- | -------- | ----------- | ----- | ------- |
| Commands - Console | all | start-stop |  -  | True |

### AAA Accounting Device Configuration

```eos
aaa accounting commands all console start-stop logging
```

# Aliases

```eos
alias 1min show log last 1 minute
alias tail bash sudo tail -f /var/log/messages
!
```

# Monitoring

## TerminAttr Daemon

### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | apiserver.arista.io:443 | management | token-secure,/mnt/flash/cv-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=apiserver.arista.io:443 -cvauth=token-secure,/mnt/flash/cv-onboarding-token -cvvrf=management -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## Logging

### Logging Servers and Features Summary

| Type | Level |
| -----| ----- |
| Buffer | informational |

| VRF | Source Interface |
| --- | ---------------- |
| - | Management1 |
| management | Management1 |

| VRF | Hosts | Ports | Protocol |
| --- | ----- | ----- | -------- |
| management | 10.112.112.31 | 514 | UDP |

### Logging Servers and Features Device Configuration

```eos
!
logging buffered 100000000 informational
logging vrf management host 10.112.112.31 514
logging source-interface Management1
logging vrf management source-interface Management1
```

## SNMP

### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| - | - | All | Enabled |

### SNMP ACLs
| IP | ACL | VRF |
| -- | --- | --- |
| IPv4 | SNMP-ACL | management |

### SNMP Local Interfaces

| Local Interface | VRF |
| --------------- | --- |
| Management1 | management |

### SNMP Hosts Configuration

| Host | VRF | Community | Username | Authentication level | SNMP Version |
| ---- |---- | --------- | -------- | -------------------- | ------------ |
| 10.112.34.58 | management | COMMUNITY | - | - | 2c |

### SNMP Communities

| Community | Access | Access List IPv4 | Access List IPv6 | View |
| --------- | ------ | ---------------- | ---------------- | ---- |
| COMMUNITY | ro | SNMP-ACL | - | - |

### SNMP Device Configuration

```eos
!
snmp-server ipv4 access-list SNMP-ACL vrf management
snmp-server vrf management local-interface Management1
snmp-server community COMMUNITY ro SNMP-ACL
snmp-server host 10.112.34.58 vrf management version 2c COMMUNITY
snmp-server enable traps
```

## SFlow

### SFlow Summary

| VRF | SFlow Source | SFlow Destination | Port |
| --- | ------------ | ----------------- | ---- |
| default | - | 127.0.0.1 | 6343 |
| default | Loopback0 | - | - |

sFlow Sample Rate: 16384

sFlow is enabled.

### SFlow Device Configuration

```eos
!
sflow sample 16384
sflow vrf default destination 127.0.0.1
sflow vrf default source-interface Loopback0
sflow run
```

# MLAG

## MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| DC_COMPUTE_POD2 | Vlan4094 | 10.1.0.4 | Port-Channel551 |

Dual primary detection is disabled.

## MLAG Device Configuration

```eos
!
mlag configuration
   domain-id DC_COMPUTE_POD2
   local-interface Vlan4094
   peer-address 10.1.0.4
   peer-link Port-Channel551
   reload-delay mlag 300
   reload-delay non-mlag 330
```

# Spanning Tree

## Spanning Tree Summary

STP mode: **mstp**

### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 32768 |

### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 32768
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# VLANs

## VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 10 | Tenant_10_OP_Zone_1 | - |
| 11 | Tenant_10_OP_Zone_2 | - |
| 12 | Tenant_10_WEB_Zone_1 | - |
| 13 | Tenant_10_WEB_Zone_2 | - |
| 14 | Tenant_10_VMOTION | - |
| 20 | Tenant_20_OP_Zone_1 | - |
| 21 | Tenant_20_OP_Zone_2 | - |
| 3009 | MLAG_iBGP_Tenant_10_OP_Zone | LEAF_PEER_L3 |
| 3010 | MLAG_iBGP_Tenant_10_WEB_Zone | LEAF_PEER_L3 |
| 3019 | MLAG_iBGP_Tenant_20_OP_Zone | LEAF_PEER_L3 |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

## VLANs Device Configuration

```eos
!
vlan 10
   name Tenant_10_OP_Zone_1
!
vlan 11
   name Tenant_10_OP_Zone_2
!
vlan 12
   name Tenant_10_WEB_Zone_1
!
vlan 13
   name Tenant_10_WEB_Zone_2
!
vlan 14
   name Tenant_10_VMOTION
!
vlan 20
   name Tenant_20_OP_Zone_1
!
vlan 21
   name Tenant_20_OP_Zone_2
!
vlan 3009
   name MLAG_iBGP_Tenant_10_OP_Zone
   trunk group LEAF_PEER_L3
!
vlan 3010
   name MLAG_iBGP_Tenant_10_WEB_Zone
   trunk group LEAF_PEER_L3
!
vlan 3019
   name MLAG_iBGP_Tenant_20_OP_Zone
   trunk group LEAF_PEER_L3
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

# MAC Address Table

## MAC Address Table Summary

- MAC address table entry maximum age: 1300 seconds

## MAC Address Table Device Configuration

```eos
!
mac address-table aging-time 1300
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet55/1 | MLAG_PEER_DC-LEAF3_Ethernet55/1 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 551 |
| Ethernet56/1 | MLAG_PEER_DC-LEAF3_Ethernet56/1 | *trunk | *2-4094 | *- | *['LEAF_PEER_L3', 'MLAG'] | 551 |

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet49/1 | P2P_LINK_TO_DC-SPINE1_Ethernet4/1 | routed | - | 10.0.0.13/31 | default | 1500 | False | - | - |
| Ethernet50/1 | P2P_LINK_TO_DC-SPINE2_Ethernet4/1 | routed | - | 10.0.0.15/31 | default | 1500 | False | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet49/1
   description P2P_LINK_TO_DC-SPINE1_Ethernet4/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.13/31
!
interface Ethernet50/1
   description P2P_LINK_TO_DC-SPINE2_Ethernet4/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.15/31
!
interface Ethernet55/1
   description MLAG_PEER_DC-LEAF3_Ethernet55/1
   no shutdown
   channel-group 551 mode active
!
interface Ethernet56/1
   description MLAG_PEER_DC-LEAF3_Ethernet56/1
   no shutdown
   channel-group 551 mode active
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel551 | MLAG_PEER_DC-LEAF3_Po551 | switched | trunk | 2-4094 | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel551
   description MLAG_PEER_DC-LEAF3_Po551
   no shutdown
   switchport
   switchport trunk allowed vlan 2-4094
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.255.254.4/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 10.255.255.3/32 |
| Loopback10 | Tenant_10_OP_Zone_VTEP_DIAGNOSTICS | Tenant_10_OP_Zone | 10.255.10.4/32 |
| Loopback11 | Tenant_10_WEB_Zone_VTEP_DIAGNOSTICS | Tenant_10_WEB_Zone | 10.255.11.4/32 |
| Loopback20 | Tenant_20_OP_Zone_VTEP_DIAGNOSTICS | Tenant_20_OP_Zone | 10.255.20.4/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |
| Loopback10 | Tenant_10_OP_Zone_VTEP_DIAGNOSTICS | Tenant_10_OP_Zone | - |
| Loopback11 | Tenant_10_WEB_Zone_VTEP_DIAGNOSTICS | Tenant_10_WEB_Zone | - |
| Loopback20 | Tenant_20_OP_Zone_VTEP_DIAGNOSTICS | Tenant_20_OP_Zone | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.255.254.4/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 10.255.255.3/32
!
interface Loopback10
   description Tenant_10_OP_Zone_VTEP_DIAGNOSTICS
   no shutdown
   vrf Tenant_10_OP_Zone
   ip address 10.255.10.4/32
!
interface Loopback11
   description Tenant_10_WEB_Zone_VTEP_DIAGNOSTICS
   no shutdown
   vrf Tenant_10_WEB_Zone
   ip address 10.255.11.4/32
!
interface Loopback20
   description Tenant_20_OP_Zone_VTEP_DIAGNOSTICS
   no shutdown
   vrf Tenant_20_OP_Zone
   ip address 10.255.20.4/32
```

## VLAN Interfaces

### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan10 | Tenant_10_OP_Zone_1 | Tenant_10_OP_Zone | - | False |
| Vlan11 | Tenant_10_OP_Zone_2 | Tenant_10_OP_Zone | - | False |
| Vlan12 | Tenant_10_WEB_Zone_1 | Tenant_10_WEB_Zone | - | False |
| Vlan13 | Tenant_10_WEB_Zone_2 | Tenant_10_WEB_Zone | - | False |
| Vlan20 | Tenant_20_OP_Zone_1 | Tenant_20_OP_Zone | - | False |
| Vlan21 | Tenant_20_OP_Zone_2 | Tenant_20_OP_Zone | - | False |
| Vlan3009 | MLAG_PEER_L3_iBGP: vrf Tenant_10_OP_Zone | Tenant_10_OP_Zone | 1500 | False |
| Vlan3010 | MLAG_PEER_L3_iBGP: vrf Tenant_10_WEB_Zone | Tenant_10_WEB_Zone | 1500 | False |
| Vlan3019 | MLAG_PEER_L3_iBGP: vrf Tenant_20_OP_Zone | Tenant_20_OP_Zone | 1500 | False |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 1500 | False |
| Vlan4094 | MLAG_PEER | default | 1500 | False |

#### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan10 |  Tenant_10_OP_Zone  |  -  |  10.10.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan11 |  Tenant_10_OP_Zone  |  -  |  10.10.11.1/24  |  -  |  -  |  -  |  -  |
| Vlan12 |  Tenant_10_WEB_Zone  |  -  |  10.10.12.1/24  |  -  |  -  |  -  |  -  |
| Vlan13 |  Tenant_10_WEB_Zone  |  -  |  10.10.13.1/24  |  -  |  -  |  -  |  -  |
| Vlan20 |  Tenant_20_OP_Zone  |  -  |  10.20.20.0/24  |  -  |  -  |  -  |  -  |
| Vlan21 |  Tenant_20_OP_Zone  |  -  |  10.20.21.1/24  |  -  |  -  |  -  |  -  |
| Vlan3009 |  Tenant_10_OP_Zone  |  10.1.1.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan3010 |  Tenant_10_WEB_Zone  |  10.1.1.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan3019 |  Tenant_20_OP_Zone  |  10.1.1.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.1.1.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.1.0.5/31  |  -  |  -  |  -  |  -  |  -  |

### VLAN Interfaces Device Configuration

```eos
!
interface Vlan10
   description Tenant_10_OP_Zone_1
   no shutdown
   vrf Tenant_10_OP_Zone
   ip address virtual 10.10.10.1/24
!
interface Vlan11
   description Tenant_10_OP_Zone_2
   no shutdown
   vrf Tenant_10_OP_Zone
   ip address virtual 10.10.11.1/24
!
interface Vlan12
   description Tenant_10_WEB_Zone_1
   no shutdown
   vrf Tenant_10_WEB_Zone
   ip address virtual 10.10.12.1/24
!
interface Vlan13
   description Tenant_10_WEB_Zone_2
   no shutdown
   vrf Tenant_10_WEB_Zone
   ip address virtual 10.10.13.1/24
!
interface Vlan20
   description Tenant_20_OP_Zone_1
   no shutdown
   vrf Tenant_20_OP_Zone
   ip address virtual 10.20.20.0/24
!
interface Vlan21
   description Tenant_20_OP_Zone_2
   no shutdown
   vrf Tenant_20_OP_Zone
   ip address virtual 10.20.21.1/24
!
interface Vlan3009
   description MLAG_PEER_L3_iBGP: vrf Tenant_10_OP_Zone
   no shutdown
   mtu 1500
   vrf Tenant_10_OP_Zone
   ip address 10.1.1.5/31
!
interface Vlan3010
   description MLAG_PEER_L3_iBGP: vrf Tenant_10_WEB_Zone
   no shutdown
   mtu 1500
   vrf Tenant_10_WEB_Zone
   ip address 10.1.1.5/31
!
interface Vlan3019
   description MLAG_PEER_L3_iBGP: vrf Tenant_20_OP_Zone
   no shutdown
   mtu 1500
   vrf Tenant_20_OP_Zone
   ip address 10.1.1.5/31
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 1500
   ip address 10.1.1.5/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 1500
   no autostate
   ip address 10.1.0.5/31
```

## VXLAN Interface

### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

#### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 10 | 10010 | - | - |
| 11 | 10011 | - | - |
| 12 | 10012 | - | - |
| 13 | 10013 | - | - |
| 14 | 10014 | - | - |
| 20 | 10020 | - | - |
| 21 | 10021 | - | - |

#### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| Tenant_10_OP_Zone | 10 | - |
| Tenant_10_WEB_Zone | 11 | - |
| Tenant_20_OP_Zone | 20 | - |

### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description DC-LEAF4_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vlan 11 vni 10011
   vxlan vlan 12 vni 10012
   vxlan vlan 13 vni 10013
   vxlan vlan 14 vni 10014
   vxlan vlan 20 vni 10020
   vxlan vlan 21 vni 10021
   vxlan vrf Tenant_10_OP_Zone vni 10
   vxlan vrf Tenant_10_WEB_Zone vni 11
   vxlan vrf Tenant_20_OP_Zone vni 20
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## Virtual Router MAC Address

### Virtual Router MAC Address Summary

#### Virtual Router MAC Address: 00:1c:73:00:00:01

### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:01
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| management | false |
| Tenant_10_OP_Zone | true |
| Tenant_10_WEB_Zone | true |
| Tenant_20_OP_Zone | true |

### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf management
ip routing vrf Tenant_10_OP_Zone
ip routing vrf Tenant_10_WEB_Zone
ip routing vrf Tenant_20_OP_Zone
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| management | false |
| Tenant_10_OP_Zone | false |
| Tenant_10_WEB_Zone | false |
| Tenant_20_OP_Zone | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| management | 0.0.0.0/0 | 10.99.99.1 | - | 1 | - | - | - |

### Static Routes Device Configuration

```eos
!
ip route vrf management 0.0.0.0/0 10.99.99.1
```

## ARP

Global ARP timeout: 900

## Router BGP

### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65102|  10.255.254.4 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| maximum-paths 4 ecmp 4 |

### Router BGP Peer Groups

#### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

#### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

#### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65102 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- |
| 10.0.0.12 | 65000 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.0.0.14 | 65000 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.1.1.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - |
| 10.255.252.1 | 65000 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - |
| 10.255.252.2 | 65000 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - |
| 10.1.1.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Tenant_10_OP_Zone | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - |
| 10.1.1.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Tenant_10_WEB_Zone | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - |
| 10.1.1.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Tenant_20_OP_Zone | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-PEERS | True |

### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 10 | 10.255.254.4:10010 | 10010:10010 | - | - | learned |
| 11 | 10.255.254.4:10011 | 10011:10011 | - | - | learned |
| 12 | 10.255.254.4:10012 | 10012:10012 | - | - | learned |
| 13 | 10.255.254.4:10013 | 10013:10013 | - | - | learned |
| 14 | 10.255.254.4:10014 | 10014:10014 | - | - | learned |
| 20 | 10.255.254.4:10020 | 10020:10020 | - | - | learned |
| 21 | 10.255.254.4:10021 | 10021:10021 | - | - | learned |

### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| Tenant_10_OP_Zone | 10.255.254.4:10 | connected<br>static |
| Tenant_10_WEB_Zone | 10.255.254.4:11 | connected |
| Tenant_20_OP_Zone | 10.255.254.4:20 | connected |

### Router BGP Device Configuration

```eos
!
router bgp 65102
   router-id 10.255.254.4
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65102
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description DC-LEAF3
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 10.0.0.12 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.0.12 remote-as 65000
   neighbor 10.0.0.12 description DC-SPINE1_Ethernet4/1
   neighbor 10.0.0.14 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.0.14 remote-as 65000
   neighbor 10.0.0.14 description DC-SPINE2_Ethernet4/1
   neighbor 10.1.1.4 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.1.1.4 description DC-LEAF3
   neighbor 10.255.252.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.252.1 remote-as 65000
   neighbor 10.255.252.1 description DC-SPINE1
   neighbor 10.255.252.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.252.2 remote-as 65000
   neighbor 10.255.252.2 description DC-SPINE2
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 10
      rd 10.255.254.4:10010
      route-target both 10010:10010
      redistribute learned
   !
   vlan 11
      rd 10.255.254.4:10011
      route-target both 10011:10011
      redistribute learned
   !
   vlan 12
      rd 10.255.254.4:10012
      route-target both 10012:10012
      redistribute learned
   !
   vlan 13
      rd 10.255.254.4:10013
      route-target both 10013:10013
      redistribute learned
   !
   vlan 14
      rd 10.255.254.4:10014
      route-target both 10014:10014
      redistribute learned
   !
   vlan 20
      rd 10.255.254.4:10020
      route-target both 10020:10020
      redistribute learned
   !
   vlan 21
      rd 10.255.254.4:10021
      route-target both 10021:10021
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf Tenant_10_OP_Zone
      rd 10.255.254.4:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.255.254.4
      neighbor 10.1.1.4 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
      redistribute static
   !
   vrf Tenant_10_WEB_Zone
      rd 10.255.254.4:11
      route-target import evpn 11:11
      route-target export evpn 11:11
      router-id 10.255.254.4
      neighbor 10.1.1.4 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
   !
   vrf Tenant_20_OP_Zone
      rd 10.255.254.4:20
      route-target import evpn 20:20
      route-target export evpn 20:20
      router-id 10.255.254.4
      neighbor 10.1.1.4 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
```

# BFD

## Router BFD

### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 1200 | 1200 | 3 |

### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
```

# Multicast

## IP IGMP Snooping

### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

### IP IGMP Snooping Device Configuration

```eos
```

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.255.254.0/24 eq 32 |
| 20 | permit 10.255.255.0/24 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.255.254.0/24 eq 32
   seq 20 permit 10.255.255.0/24 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

#### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

# ACL

## Standard Access-lists

### Standard Access-lists Summary

#### SNMP-ACL

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.0.0.0/8 |

### Standard Access-lists Device Configuration

```eos
!
ip access-list standard SNMP-ACL
   10 permit 10.0.0.0/8
```

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| management | disabled |
| Tenant_10_OP_Zone | enabled |
| Tenant_10_WEB_Zone | enabled |
| Tenant_20_OP_Zone | enabled |

## VRF Instances Device Configuration

```eos
!
vrf instance management
!
vrf instance Tenant_10_OP_Zone
!
vrf instance Tenant_10_WEB_Zone
!
vrf instance Tenant_20_OP_Zone
```

# Virtual Source NAT

## Virtual Source NAT Summary

| Source NAT VRF | Source NAT IP Address |
| -------------- | --------------------- |
| Tenant_10_OP_Zone | 10.255.10.4 |
| Tenant_10_WEB_Zone | 10.255.11.4 |
| Tenant_20_OP_Zone | 10.255.20.4 |

## Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf Tenant_10_OP_Zone address 10.255.10.4
ip address virtual source-nat vrf Tenant_10_WEB_Zone address 10.255.11.4
ip address virtual source-nat vrf Tenant_20_OP_Zone address 10.255.20.4
```

# Quality Of Service
