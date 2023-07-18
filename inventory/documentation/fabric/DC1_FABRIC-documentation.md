# DC1_FABRIC

# Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

# Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision |
| --- | ---- | ---- | ------------- | -------- | -------------------------- |
| DC1_FABRIC | l3leaf | ENT-DC1-BORDER1 | 10.99.99.55/24 | cEOSLab | Provisioned |
| DC1_FABRIC | l3leaf | ENT-DC1-BORDER2 | 10.99.99.56/24 | cEOSLab | Provisioned |
| DC1_FABRIC | l3leaf | ENT-DC1-LEAF1 | 10.99.99.43/24 | cEOSLab | Provisioned |
| DC1_FABRIC | l3leaf | ENT-DC1-LEAF2 | 10.99.99.44/24 | cEOSLab | Provisioned |
| DC1_FABRIC | l3leaf | ENT-DC1-LEAF3 | 10.99.99.45/24 | cEOSLab | Provisioned |
| DC1_FABRIC | l3leaf | ENT-DC1-LEAF4 | 10.99.99.46/24 | cEOSLab | Provisioned |
| DC1_FABRIC | l3leaf | ENT-DC1-LEAF5 | 10.99.99.47/24 | cEOSLab | Provisioned |
| DC1_FABRIC | l3leaf | ENT-DC1-LEAF6 | 10.99.99.48/24 | cEOSLab | Provisioned |
| DC1_FABRIC | spine | ENT-DC1-SPINE1 | 10.99.99.39/24 | cEOSLab | Provisioned |
| DC1_FABRIC | spine | ENT-DC1-SPINE2 | 10.99.99.40/24 | cEOSLab | Provisioned |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

## Fabric Switches with inband Management IP
| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

# Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | ENT-DC1-BORDER1 | Ethernet49/1 | spine | ENT-DC1-SPINE1 | Ethernet7/1 |
| l3leaf | ENT-DC1-BORDER1 | Ethernet50/1 | spine | ENT-DC1-SPINE2 | Ethernet7/1 |
| l3leaf | ENT-DC1-BORDER1 | Ethernet55/1 | mlag_peer | ENT-DC1-BORDER2 | Ethernet55/1 |
| l3leaf | ENT-DC1-BORDER1 | Ethernet56/1 | mlag_peer | ENT-DC1-BORDER2 | Ethernet56/1 |
| l3leaf | ENT-DC1-BORDER2 | Ethernet49/1 | spine | ENT-DC1-SPINE1 | Ethernet8/1 |
| l3leaf | ENT-DC1-BORDER2 | Ethernet50/1 | spine | ENT-DC1-SPINE2 | Ethernet8/1 |
| l3leaf | ENT-DC1-LEAF1 | Ethernet49/1 | spine | ENT-DC1-SPINE1 | Ethernet1/1 |
| l3leaf | ENT-DC1-LEAF1 | Ethernet50/1 | spine | ENT-DC1-SPINE2 | Ethernet1/1 |
| l3leaf | ENT-DC1-LEAF1 | Ethernet55/1 | mlag_peer | ENT-DC1-LEAF2 | Ethernet55/1 |
| l3leaf | ENT-DC1-LEAF1 | Ethernet56/1 | mlag_peer | ENT-DC1-LEAF2 | Ethernet56/1 |
| l3leaf | ENT-DC1-LEAF2 | Ethernet49/1 | spine | ENT-DC1-SPINE1 | Ethernet2/1 |
| l3leaf | ENT-DC1-LEAF2 | Ethernet50/1 | spine | ENT-DC1-SPINE2 | Ethernet2/1 |
| l3leaf | ENT-DC1-LEAF3 | Ethernet49/1 | spine | ENT-DC1-SPINE1 | Ethernet3/1 |
| l3leaf | ENT-DC1-LEAF3 | Ethernet50/1 | spine | ENT-DC1-SPINE2 | Ethernet3/1 |
| l3leaf | ENT-DC1-LEAF3 | Ethernet55/1 | mlag_peer | ENT-DC1-LEAF4 | Ethernet55/1 |
| l3leaf | ENT-DC1-LEAF3 | Ethernet56/1 | mlag_peer | ENT-DC1-LEAF4 | Ethernet56/1 |
| l3leaf | ENT-DC1-LEAF4 | Ethernet49/1 | spine | ENT-DC1-SPINE1 | Ethernet4/1 |
| l3leaf | ENT-DC1-LEAF4 | Ethernet50/1 | spine | ENT-DC1-SPINE2 | Ethernet4/1 |
| l3leaf | ENT-DC1-LEAF5 | Ethernet49/1 | spine | ENT-DC1-SPINE1 | Ethernet5/1 |
| l3leaf | ENT-DC1-LEAF5 | Ethernet50/1 | spine | ENT-DC1-SPINE2 | Ethernet5/1 |
| l3leaf | ENT-DC1-LEAF5 | Ethernet55/1 | mlag_peer | ENT-DC1-LEAF6 | Ethernet55/1 |
| l3leaf | ENT-DC1-LEAF5 | Ethernet56/1 | mlag_peer | ENT-DC1-LEAF6 | Ethernet56/1 |
| l3leaf | ENT-DC1-LEAF6 | Ethernet49/1 | spine | ENT-DC1-SPINE1 | Ethernet6/1 |
| l3leaf | ENT-DC1-LEAF6 | Ethernet50/1 | spine | ENT-DC1-SPINE2 | Ethernet6/1 |

# Fabric IP Allocation

## Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.1.0.0/23 | 512 | 32 | 6.25 % |

## Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| ENT-DC1-BORDER1 | Ethernet49/1 | 10.1.0.121/31 | ENT-DC1-SPINE1 | Ethernet7/1 | 10.1.0.120/31 |
| ENT-DC1-BORDER1 | Ethernet50/1 | 10.1.0.123/31 | ENT-DC1-SPINE2 | Ethernet7/1 | 10.1.0.122/31 |
| ENT-DC1-BORDER2 | Ethernet49/1 | 10.1.0.125/31 | ENT-DC1-SPINE1 | Ethernet8/1 | 10.1.0.124/31 |
| ENT-DC1-BORDER2 | Ethernet50/1 | 10.1.0.127/31 | ENT-DC1-SPINE2 | Ethernet8/1 | 10.1.0.126/31 |
| ENT-DC1-LEAF1 | Ethernet49/1 | 10.1.0.1/31 | ENT-DC1-SPINE1 | Ethernet1/1 | 10.1.0.0/31 |
| ENT-DC1-LEAF1 | Ethernet50/1 | 10.1.0.3/31 | ENT-DC1-SPINE2 | Ethernet1/1 | 10.1.0.2/31 |
| ENT-DC1-LEAF2 | Ethernet49/1 | 10.1.0.5/31 | ENT-DC1-SPINE1 | Ethernet2/1 | 10.1.0.4/31 |
| ENT-DC1-LEAF2 | Ethernet50/1 | 10.1.0.7/31 | ENT-DC1-SPINE2 | Ethernet2/1 | 10.1.0.6/31 |
| ENT-DC1-LEAF3 | Ethernet49/1 | 10.1.0.9/31 | ENT-DC1-SPINE1 | Ethernet3/1 | 10.1.0.8/31 |
| ENT-DC1-LEAF3 | Ethernet50/1 | 10.1.0.11/31 | ENT-DC1-SPINE2 | Ethernet3/1 | 10.1.0.10/31 |
| ENT-DC1-LEAF4 | Ethernet49/1 | 10.1.0.13/31 | ENT-DC1-SPINE1 | Ethernet4/1 | 10.1.0.12/31 |
| ENT-DC1-LEAF4 | Ethernet50/1 | 10.1.0.15/31 | ENT-DC1-SPINE2 | Ethernet4/1 | 10.1.0.14/31 |
| ENT-DC1-LEAF5 | Ethernet49/1 | 10.1.0.17/31 | ENT-DC1-SPINE1 | Ethernet5/1 | 10.1.0.16/31 |
| ENT-DC1-LEAF5 | Ethernet50/1 | 10.1.0.19/31 | ENT-DC1-SPINE2 | Ethernet5/1 | 10.1.0.18/31 |
| ENT-DC1-LEAF6 | Ethernet49/1 | 10.1.0.21/31 | ENT-DC1-SPINE1 | Ethernet6/1 | 10.1.0.20/31 |
| ENT-DC1-LEAF6 | Ethernet50/1 | 10.1.0.23/31 | ENT-DC1-SPINE2 | Ethernet6/1 | 10.1.0.22/31 |

## Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.1.100.0/24 | 256 | 2 | 0.79 % |
| 10.1.101.0/24 | 256 | 8 | 3.13 % |

## Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| DC1_FABRIC | ENT-DC1-BORDER1 | 10.1.101.31/32 |
| DC1_FABRIC | ENT-DC1-BORDER2 | 10.1.101.32/32 |
| DC1_FABRIC | ENT-DC1-LEAF1 | 10.1.101.1/32 |
| DC1_FABRIC | ENT-DC1-LEAF2 | 10.1.101.2/32 |
| DC1_FABRIC | ENT-DC1-LEAF3 | 10.1.101.3/32 |
| DC1_FABRIC | ENT-DC1-LEAF4 | 10.1.101.4/32 |
| DC1_FABRIC | ENT-DC1-LEAF5 | 10.1.101.5/32 |
| DC1_FABRIC | ENT-DC1-LEAF6 | 10.1.101.6/32 |
| DC1_FABRIC | ENT-DC1-SPINE1 | 10.1.100.1/32 |
| DC1_FABRIC | ENT-DC1-SPINE2 | 10.1.100.2/32 |

## VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 10.1.102.0/24 | 256 | 8 | 3.13 % |

## VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| DC1_FABRIC | ENT-DC1-BORDER1 | 10.1.102.31/32 |
| DC1_FABRIC | ENT-DC1-BORDER2 | 10.1.102.31/32 |
| DC1_FABRIC | ENT-DC1-LEAF1 | 10.1.102.1/32 |
| DC1_FABRIC | ENT-DC1-LEAF2 | 10.1.102.1/32 |
| DC1_FABRIC | ENT-DC1-LEAF3 | 10.1.102.3/32 |
| DC1_FABRIC | ENT-DC1-LEAF4 | 10.1.102.3/32 |
| DC1_FABRIC | ENT-DC1-LEAF5 | 10.1.102.5/32 |
| DC1_FABRIC | ENT-DC1-LEAF6 | 10.1.102.5/32 |
