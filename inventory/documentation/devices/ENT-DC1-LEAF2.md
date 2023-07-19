# ENT-DC1-LEAF2

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [DNS Domain](#dns-domain)
  - [IP Name Servers](#ip-name-servers)
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
- [EOS CLI](#eos-cli)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | management | 10.99.99.44/24 | 10.99.99.1 |

##### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | management | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf management
   ip address 10.99.99.44/24
```

### DNS Domain

#### DNS domain: dime-a-tron.com

#### DNS Domain Device Configuration

```eos
dns domain dime-a-tron.com
!
```

### IP Name Servers

#### IP Name Servers Summary

| Name Server | VRF | Priority |
| ----------- | --- | -------- |
| 1.1.1.1 | management | - |

#### IP Name Servers Device Configuration

```eos
ip name-server vrf management 1.1.1.1
```

### Domain Lookup

#### DNS Domain Lookup Summary

| Source interface | vrf |
| ---------------- | --- |
| Management1 | management |

#### DNS Domain Lookup Device Configuration

```eos
ip domain lookup vrf management source-interface Management1
```

### Clock Settings

#### Clock Timezone Settings

Clock Timezone is set to **US/Central**.

#### Clock Configuration

```eos
!
clock timezone US/Central
```

### NTP

#### NTP Summary

##### NTP Local Interface

| Interface | VRF |
| --------- | --- |
| Management1 | management |

##### NTP Servers

| Server | VRF | Preferred | Burst | iBurst | Version | Min Poll | Max Poll | Local-interface | Key |
| ------ | --- | --------- | ----- | ------ | ------- | -------- | -------- | --------------- | --- |
| 0.pool.ntp.org | management | - | False | True | - | - | - | Management1 | - |

##### NTP Authentication

#### NTP Device Configuration

```eos
!
ntp local-interface vrf management Management1
ntp server vrf management 0.pool.ntp.org iburst local-interface Management1
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| management | - | - |

#### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf management
      no shutdown
```

## Authentication

### Local Users

#### Local Users Summary

| User | Privilege | Role | Disabled | Shell |
| ---- | --------- | ---- | -------- | ----- |
| admin | 15 | network-admin | False | - |
| dcapetz | 15 | network-admin | False | - |

#### Local Users Device Configuration

```eos
!
username admin privilege 15 role network-admin secret sha512 <removed>
username dcapetz privilege 15 role network-admin secret sha512 <removed>
username dcapetz ssh-key ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDZ148VUrWGE12gbl/yuMn2tCKwaJTmayGqEZGseVCtWO54gB0J2vt/m75xY5dmHquZvLQVfgp7JfV/FUa+Ao+QhHFFxdq7Br+T8PBE8u64Mr97MWpUT3Kc79lL48MvJfNH/adQ8sRK2LkLDp2Lgfc8XQNsA28NkOOh0EV4iSWVCv9K0m19Hihp6cjfvAb8Jt9y5NvtZiJvsBmv+A6DJ5okEsmQ3qVgmEzJ2xWHigYwZOmJsW5XcS0+cqVVT28J/rE4d6QPds6kSx9gkp3/RJxxH9lwhzDQFoOmfMlz5TixG7a4cnvvxETmEfw+QKYcKcIbCZdTi8/1+FLLQ2SOVQlVFsbzRcfUa3Z+FfYk358H6AYQ5fY2R7pYHC8jIOjVyp4Q0KcCo29NzZ07eXr6Z0hmaB7cXTP2devB6VqPxnhLeBXIPgLs8JU/yJd7aneYEonn5vSnEBiC3E61l8Dx3pz0WBYsZHNfL+A/stCBdJRl35XAoGqnEfU73eaZiXx46CU= dcapetz@dcapetz
```

### TACACS Servers

#### TACACS Servers

| VRF | TACACS Servers | Single-Connection |
| --- | -------------- | ----------------- |
| management | home-ise.dime-a-tron.com | False |

#### TACACS Servers Device Configuration

```eos
!
tacacs-server host home-ise.dime-a-tron.com vrf management key 7 <removed>
```

### IP TACACS Source Interfaces

#### IP TACACS Source Interfaces

| VRF | Source Interface Name |
| --- | --------------- |
| management | Management1 |

#### IP TACACS Source Interfaces Device Configuration

```eos
!
ip tacacs vrf management source-interface Management1
```

### AAA Server Groups

#### AAA Server Groups Summary

| Server Group Name | Type  | VRF | IP address |
| ------------------| ----- | --- | ---------- |
| home-ise | tacacs+ | management | home-ise.dime-a-tron.com |

#### AAA Server Groups Device Configuration

```eos
!
aaa group server tacacs+ home-ise
   server home-ise.dime-a-tron.com vrf management
```

### AAA Authentication

#### AAA Authentication Summary

| Type | Sub-type | User Stores |
| ---- | -------- | ---------- |
| Login | default | local |
| Login | console | local |

#### AAA Authentication Device Configuration

```eos
aaa authentication login default local
aaa authentication login console local
!
```

### AAA Authorization

#### AAA Authorization Summary

| Type | User Stores |
| ---- | ----------- |
| Exec | local |

Authorization for configuration commands is disabled.

#### AAA Authorization Privilege Levels Summary

| Privilege Level | User Stores |
| --------------- | ----------- |
| all | local |

#### AAA Authorization Device Configuration

```eos
aaa authorization exec default local
aaa authorization commands all default local
!
```

### AAA Accounting

#### AAA Accounting Summary

| Type | Commands | Record type | Group | Logging |
| ---- | -------- | ----------- | ----- | ------- |
| Commands - Console | all | start-stop |  -  | True |

#### AAA Accounting Device Configuration

```eos
aaa accounting commands all console start-stop logging
```

## Aliases

```eos
alias 1min show log last 1 minute
alias tail bash sudo tail -f /var/log/messages
!
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | apiserver.arista.io:443 | management | token-secure,/mnt/flash/cv-onboarding-token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=apiserver.arista.io:443 -cvauth=token-secure,/mnt/flash/cv-onboarding-token -cvvrf=management -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

### Logging

#### Logging Servers and Features Summary

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

#### Logging Servers and Features Device Configuration

```eos
!
logging buffered 100000000 informational
logging vrf management host 10.112.112.31 514
logging source-interface Management1
logging vrf management source-interface Management1
```

### SNMP

#### SNMP Configuration Summary

| Contact | Location | SNMP Traps | State |
| ------- | -------- | ---------- | ----- |
| - | - | All | Enabled |

#### SNMP ACLs
| IP | ACL | VRF |
| -- | --- | --- |
| IPv4 | SNMP-ACL | management |

#### SNMP Local Interfaces

| Local Interface | VRF |
| --------------- | --- |
| Management1 | management |

#### SNMP Hosts Configuration

| Host | VRF | Community | Username | Authentication level | SNMP Version |
| ---- |---- | --------- | -------- | -------------------- | ------------ |
| 10.112.34.58 | management | <removed> | - | - | 2c |

#### SNMP Communities

| Community | Access | Access List IPv4 | Access List IPv6 | View |
| --------- | ------ | ---------------- | ---------------- | ---- |
| <removed> | ro | SNMP-ACL | - | - |

#### SNMP Device Configuration

```eos
!
snmp-server ipv4 access-list SNMP-ACL vrf management
snmp-server vrf management local-interface Management1
snmp-server community <removed> ro SNMP-ACL
snmp-server host 10.112.34.58 vrf management version 2c <removed>
snmp-server enable traps
```

### SFlow

#### SFlow Summary

| VRF | SFlow Source | SFlow Destination | Port |
| --- | ------------ | ----------------- | ---- |
| default | - | 127.0.0.1 | 6343 |
| default | Loopback0 | - | - |

sFlow Sample Rate: 16384

sFlow is enabled.

#### SFlow Device Configuration

```eos
!
sflow sample 16384
sflow vrf default destination 127.0.0.1
sflow vrf default source-interface Loopback0
sflow run
```

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| EnterpriseDC1LeafDomain1 | Vlan4094 | 10.255.255.0 | Port-Channel551 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id EnterpriseDC1LeafDomain1
   local-interface Vlan4094
   peer-address 10.255.255.0
   peer-link Port-Channel551
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 32768 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 32768
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 10 | CORP_GLOBAL_SHARED_10 | - |
| 20 | CORP_GLOBAL_SHARED_20 | - |
| 110 | CORP_DC1_SHARED | - |
| 111 | CORP_DC1_RACK1 | - |
| 3009 | MLAG_iBGP_CORP | LEAF_PEER_L3 |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 10
   name CORP_GLOBAL_SHARED_10
!
vlan 20
   name CORP_GLOBAL_SHARED_20
!
vlan 110
   name CORP_DC1_SHARED
!
vlan 111
   name CORP_DC1_RACK1
!
vlan 3009
   name MLAG_iBGP_CORP
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

## MAC Address Table

### MAC Address Table Summary

- MAC address table entry maximum age: 1300 seconds

### MAC Address Table Device Configuration

```eos
!
mac address-table aging-time 1300
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | ENT-DC1-HOST1_Et2 | *trunk | *10,20,110,111 | *- | *- | 1 |
| Ethernet55/1 | MLAG_PEER_ENT-DC1-LEAF1_Ethernet55/1 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 551 |
| Ethernet56/1 | MLAG_PEER_ENT-DC1-LEAF1_Ethernet56/1 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 551 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet49/1 | P2P_LINK_TO_ENT-DC1-SPINE1_Ethernet2/1 | routed | - | 10.1.0.5/31 | default | 1500 | False | - | - |
| Ethernet50/1 | P2P_LINK_TO_ENT-DC1-SPINE2_Ethernet2/1 | routed | - | 10.1.0.7/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description ENT-DC1-HOST1_Et2
   no shutdown
   channel-group 1 mode active
!
interface Ethernet49/1
   description P2P_LINK_TO_ENT-DC1-SPINE1_Ethernet2/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.1.0.5/31
!
interface Ethernet50/1
   description P2P_LINK_TO_ENT-DC1-SPINE2_Ethernet2/1
   no shutdown
   mtu 1500
   no switchport
   ip address 10.1.0.7/31
!
interface Ethernet55/1
   description MLAG_PEER_ENT-DC1-LEAF1_Ethernet55/1
   no shutdown
   channel-group 551 mode active
!
interface Ethernet56/1
   description MLAG_PEER_ENT-DC1-LEAF1_Ethernet56/1
   no shutdown
   channel-group 551 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | ENT-DC1-HOST1_PortChannel1 | switched | trunk | 10,20,110,111 | - | - | - | - | 1 | - |
| Port-Channel551 | MLAG_PEER_ENT-DC1-LEAF1_Po551 | switched | trunk | - | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description ENT-DC1-HOST1_PortChannel1
   no shutdown
   switchport
   switchport trunk allowed vlan 10,20,110,111
   switchport mode trunk
   mlag 1
   spanning-tree portfast
   sflow enable
!
interface Port-Channel551
   description MLAG_PEER_ENT-DC1-LEAF1_Po551
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 10.1.101.2/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 10.1.102.1/32 |
| Loopback10 | CORP_VTEP_DIAGNOSTICS | CORP | 10.255.10.2/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |
| Loopback10 | CORP_VTEP_DIAGNOSTICS | CORP | - |


#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 10.1.101.2/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 10.1.102.1/32
!
interface Loopback10
   description CORP_VTEP_DIAGNOSTICS
   no shutdown
   vrf CORP
   ip address 10.255.10.2/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan10 | CORP_GLOBAL_SHARED_10 | CORP | - | False |
| Vlan20 | CORP_GLOBAL_SHARED_20 | CORP | - | False |
| Vlan110 | CORP_DC1_SHARED | CORP | - | False |
| Vlan111 | CORP_DC1_RACK1 | CORP | - | False |
| Vlan3009 | MLAG_PEER_L3_iBGP: vrf CORP | CORP | 1500 | False |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 1500 | False |
| Vlan4094 | MLAG_PEER | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan10 |  CORP  |  -  |  10.10.10.1/24  |  -  |  -  |  -  |  -  |
| Vlan20 |  CORP  |  -  |  10.20.20.1/24  |  -  |  -  |  -  |  -  |
| Vlan110 |  CORP  |  -  |  10.110.110.1/24  |  -  |  -  |  -  |  -  |
| Vlan111 |  CORP  |  -  |  10.111.111.1/24  |  -  |  -  |  -  |  -  |
| Vlan3009 |  CORP  |  10.255.251.1/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  10.255.251.1/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  10.255.255.1/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan10
   description CORP_GLOBAL_SHARED_10
   no shutdown
   vrf CORP
   ip address virtual 10.10.10.1/24
!
interface Vlan20
   description CORP_GLOBAL_SHARED_20
   no shutdown
   vrf CORP
   ip address virtual 10.20.20.1/24
!
interface Vlan110
   description CORP_DC1_SHARED
   no shutdown
   vrf CORP
   ip address virtual 10.110.110.1/24
!
interface Vlan111
   description CORP_DC1_RACK1
   no shutdown
   vrf CORP
   ip address virtual 10.111.111.1/24
!
interface Vlan3009
   description MLAG_PEER_L3_iBGP: vrf CORP
   no shutdown
   mtu 1500
   vrf CORP
   ip address 10.255.251.1/31
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 1500
   ip address 10.255.251.1/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 1500
   no autostate
   ip address 10.255.255.1/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 10 | 10010 | - | - |
| 20 | 10020 | - | - |
| 110 | 10110 | - | - |
| 111 | 10111 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| CORP | 10 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description ENT-DC1-LEAF2_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 10 vni 10010
   vxlan vlan 20 vni 10020
   vxlan vlan 110 vni 10110
   vxlan vlan 111 vni 10111
   vxlan vrf CORP vni 10
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

##### Virtual Router MAC Address: 00:1c:73:00:00:01

#### Virtual Router MAC Address Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:01
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| CORP | True |
| management | False |

#### IP Routing Device Configuration

```eos
!
ip routing
ip routing vrf CORP
no ip routing vrf management
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| CORP | false |
| management | false |

### Static Routes

#### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| management | 0.0.0.0/0 | 10.99.99.1 | - | 1 | - | - | - |

#### Static Routes Device Configuration

```eos
!
ip route vrf management 0.0.0.0/0 10.99.99.1
```

### ARP

Global ARP timeout: 900

### Router BGP

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 65101|  10.1.101.2 |

| BGP Tuning |
| ---------- |
| distance bgp 20 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 65101 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- |
| 10.1.0.4 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.1.0.6 | 65100 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - |
| 10.1.100.1 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.1.100.2 | 65100 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - |
| 10.255.251.0 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - |
| 10.255.251.0 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | CORP | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 10 | 10.1.101.2:10010 | 10010:10010 | - | - | learned |
| 20 | 10.1.101.2:10020 | 10020:10020 | - | - | learned |
| 110 | 10.1.101.2:10110 | 10110:10110 | - | - | learned |
| 111 | 10.1.101.2:10111 | 10111:10111 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| CORP | 10.1.101.2:10 | connected |

#### Router BGP Device Configuration

```eos
!
router bgp 65101
   router-id 10.1.101.2
   maximum-paths 4 ecmp 4
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
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
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 65101
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description ENT-DC1-LEAF1
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 10.1.0.4 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.1.0.4 remote-as 65100
   neighbor 10.1.0.4 description ENT-DC1-SPINE1_Ethernet2/1
   neighbor 10.1.0.6 peer group IPv4-UNDERLAY-PEERS
   neighbor 10.1.0.6 remote-as 65100
   neighbor 10.1.0.6 description ENT-DC1-SPINE2_Ethernet2/1
   neighbor 10.1.100.1 peer group EVPN-OVERLAY-PEERS
   neighbor 10.1.100.1 remote-as 65100
   neighbor 10.1.100.1 description ENT-DC1-SPINE1
   neighbor 10.1.100.2 peer group EVPN-OVERLAY-PEERS
   neighbor 10.1.100.2 remote-as 65100
   neighbor 10.1.100.2 description ENT-DC1-SPINE2
   neighbor 10.255.251.0 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 10.255.251.0 description ENT-DC1-LEAF1
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 10
      rd 10.1.101.2:10010
      route-target both 10010:10010
      redistribute learned
   !
   vlan 110
      rd 10.1.101.2:10110
      route-target both 10110:10110
      redistribute learned
   !
   vlan 111
      rd 10.1.101.2:10111
      route-target both 10111:10111
      redistribute learned
   !
   vlan 20
      rd 10.1.101.2:10020
      route-target both 10020:10020
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
   vrf CORP
      rd 10.1.101.2:10
      route-target import evpn 10:10
      route-target export evpn 10:10
      router-id 10.1.101.2
      neighbor 10.255.251.0 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 15000 | 15000 | 5 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 15000 min-rx 15000 multiplier 5
```

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.1.101.0/24 eq 32 |
| 20 | permit 10.1.102.0/24 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 10.1.101.0/24 eq 32
   seq 20 permit 10.1.102.0/24 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## ACL

### Standard Access-lists

#### Standard Access-lists Summary

##### SNMP-ACL

| Sequence | Action |
| -------- | ------ |
| 10 | permit 10.0.0.0/8 |

#### Standard Access-lists Device Configuration

```eos
!
ip access-list standard SNMP-ACL
   10 permit 10.0.0.0/8
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| CORP | enabled |
| management | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance CORP
!
vrf instance management
```

## Virtual Source NAT

### Virtual Source NAT Summary

| Source NAT VRF | Source NAT IP Address |
| -------------- | --------------------- |
| CORP | 10.255.10.2 |

### Virtual Source NAT Configuration

```eos
!
ip address virtual source-nat vrf CORP address 10.255.10.2
```

## EOS CLI

```eos
!
interface Management1
   no lldp transmit
   no lldp receive
```
