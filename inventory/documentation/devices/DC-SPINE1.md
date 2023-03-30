# DC-SPINE1
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
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [MAC Address Table](#mac-address-table)
  - [MAC Address Table Summary](#mac-address-table-summary)
  - [MAC Address Table Device Configuration](#mac-address-table-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
  - [ARP](#arp)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [ACL](#acl)
  - [Standard Access-lists](#standard-access-lists)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Quality Of Service](#quality-of-service)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | management | 10.99.99.178/24 | 10.99.99.1 |

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
   ip address 10.99.99.178/24
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

# Spanning Tree

## Spanning Tree Summary

STP mode: **none**

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
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

*Inherited from Port-Channel Interface

#### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1/1 | P2P_LINK_TO_DC-LEAF1_Ethernet49/1 | routed | - | 10.0.0.0/31 | default | 1500 | False | - | - |
| Ethernet2/1 | P2P_LINK_TO_DC-LEAF2_Ethernet49/1 | routed | - | 10.0.0.4/31 | default | 1500 | False | - | - |
| Ethernet3/1 | P2P_LINK_TO_DC-LEAF3_Ethernet49/1 | routed | - | 10.0.0.8/31 | default | 1500 | False | - | - |
| Ethernet4/1 | P2P_LINK_TO_DC-LEAF4_Ethernet49/1 | routed | - | 10.0.0.12/31 | default | 1500 | False | - | - |
| Ethernet7/1 | P2P_LINK_TO_DC-BORDER1_Ethernet49/1 | routed | - | 10.0.1.248/31 | default | 1500 | False | - | - |
| Ethernet8/1 | P2P_LINK_TO_DC-BORDER2_Ethernet49/1 | routed | - | 10.0.1.252/31 | default | 1500 | False | - | - |

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1/1
   description P2P_LINK_TO_DC-LEAF1_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.0/31
!
interface Ethernet2/1
   description P2P_LINK_TO_DC-LEAF2_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.4/31
!
interface Ethernet3/1
   description P2P_LINK_TO_DC-LEAF3_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.8/31
!
interface Ethernet4/1
   description P2P_LINK_TO_DC-LEAF4_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.0.12/31
!
interface Ethernet7/1
   description P2P_LINK_TO_DC-BORDER1_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.1.248/31
!
interface Ethernet8/1
   description P2P_LINK_TO_DC-BORDER2_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.0.1.252/31
```

## Loopback Interfaces

### Loopback Interfaces Summary

#### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.255.252.1/32 |

#### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |


### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.255.252.1/32
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| management | false |

### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf management
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| management | false |

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
| 65000|  10.255.252.1 |

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
| Next-hop unchanged | True |
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

### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- |
| 10.0.0.1 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.0.0.5 | 65101 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.0.0.9 | 65102 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.0.0.13 | 65102 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.0.1.249 | 65199 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.0.1.253 | 65199 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - |
| 10.255.254.1 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - |
| 10.255.254.2 | 65101 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - |
| 10.255.254.3 | 65102 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - |
| 10.255.254.4 | 65102 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - |
| 10.255.254.127 | 65199 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - |
| 10.255.254.128 | 65199 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - |

### Router BGP EVPN Address Family

#### EVPN Peer Groups

| Peer Group | Activate |
| ---------- | -------- |
| EVPN-OVERLAY-PEERS | True |

### Router BGP Device Configuration

```eos
!
router bgp 65000
   router-id 10.255.252.1
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   maximum-paths 4 ecmp 4
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS next-hop-unchanged
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 10.0.0.1 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.0.1 remote-as 65101
   neighbor 10.0.0.1 description DC-LEAF1_Ethernet49/1
   neighbor 10.0.0.5 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.0.5 remote-as 65101
   neighbor 10.0.0.5 description DC-LEAF2_Ethernet49/1
   neighbor 10.0.0.9 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.0.9 remote-as 65102
   neighbor 10.0.0.9 description DC-LEAF3_Ethernet49/1
   neighbor 10.0.0.13 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.0.13 remote-as 65102
   neighbor 10.0.0.13 description DC-LEAF4_Ethernet49/1
   neighbor 10.0.1.249 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.1.249 remote-as 65199
   neighbor 10.0.1.249 description DC-BORDER1_Ethernet49/1
   neighbor 10.0.1.253 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.0.1.253 remote-as 65199
   neighbor 10.0.1.253 description DC-BORDER2_Ethernet49/1
   neighbor 10.255.254.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.254.1 remote-as 65101
   neighbor 10.255.254.1 description DC-LEAF1
   neighbor 10.255.254.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.254.2 remote-as 65101
   neighbor 10.255.254.2 description DC-LEAF2
   neighbor 10.255.254.3 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.254.3 remote-as 65102
   neighbor 10.255.254.3 description DC-LEAF3
   neighbor 10.255.254.4 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.254.4 remote-as 65102
   neighbor 10.255.254.4 description DC-LEAF4
   neighbor 10.255.254.127 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.254.127 remote-as 65199
   neighbor 10.255.254.127 description DC-BORDER1
   neighbor 10.255.254.128 peer group EVPN-OVERLAY-PEERS
   neighbor 10.255.254.128 remote-as 65199
   neighbor 10.255.254.128 description DC-BORDER2
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
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

# Filters

## Prefix-lists

### Prefix-lists Summary

#### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.255.252.0/28 eq 32 |

### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.255.252.0/28 eq 32
```

## Route-maps

### Route-maps Summary

#### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
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

## VRF Instances Device Configuration

```eos
!
vrf instance management
```

# Quality Of Service
