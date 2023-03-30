# DC_FABRIC

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
| DC_FABRIC | l3leaf | DC-BORDER1 | 10.99.99.186/24 | cEOSLab | Provisioned |
| DC_FABRIC | l3leaf | DC-BORDER2 | 10.99.99.187/24 | cEOSLab | Provisioned |
| DC_FABRIC | l3leaf | DC-LEAF1 | 10.99.99.180/24 | cEOSLab | Provisioned |
| DC_FABRIC | l3leaf | DC-LEAF2 | 10.99.99.181/24 | cEOSLab | Provisioned |
| DC_FABRIC | l3leaf | DC-LEAF3 | 10.99.99.182/24 | cEOSLab | Provisioned |
| DC_FABRIC | l3leaf | DC-LEAF4 | 10.99.99.183/24 | cEOSLab | Provisioned |
| DC_FABRIC | spine | DC-SPINE1 | 10.99.99.178/24 | cEOSLab | Provisioned |
| DC_FABRIC | spine | DC-SPINE2 | 10.99.99.179/24 | cEOSLab | Provisioned |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

## Fabric Switches with inband Management IP
| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

# Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | DC-BORDER1 | Ethernet49/1 | spine | DC-SPINE1 | Ethernet7/1 |
| l3leaf | DC-BORDER1 | Ethernet50/1 | spine | DC-SPINE2 | Ethernet7/1 |
| l3leaf | DC-BORDER1 | Ethernet55/1 | mlag_peer | DC-BORDER2 | Ethernet55/1 |
| l3leaf | DC-BORDER1 | Ethernet56/1 | mlag_peer | DC-BORDER2 | Ethernet56/1 |
| l3leaf | DC-BORDER2 | Ethernet49/1 | spine | DC-SPINE1 | Ethernet8/1 |
| l3leaf | DC-BORDER2 | Ethernet50/1 | spine | DC-SPINE2 | Ethernet8/1 |
| l3leaf | DC-LEAF1 | Ethernet49/1 | spine | DC-SPINE1 | Ethernet1/1 |
| l3leaf | DC-LEAF1 | Ethernet50/1 | spine | DC-SPINE2 | Ethernet1/1 |
| l3leaf | DC-LEAF1 | Ethernet55/1 | mlag_peer | DC-LEAF2 | Ethernet55/1 |
| l3leaf | DC-LEAF1 | Ethernet56/1 | mlag_peer | DC-LEAF2 | Ethernet56/1 |
| l3leaf | DC-LEAF2 | Ethernet49/1 | spine | DC-SPINE1 | Ethernet2/1 |
| l3leaf | DC-LEAF2 | Ethernet50/1 | spine | DC-SPINE2 | Ethernet2/1 |
| l3leaf | DC-LEAF3 | Ethernet49/1 | spine | DC-SPINE1 | Ethernet3/1 |
| l3leaf | DC-LEAF3 | Ethernet50/1 | spine | DC-SPINE2 | Ethernet3/1 |
| l3leaf | DC-LEAF3 | Ethernet55/1 | mlag_peer | DC-LEAF4 | Ethernet55/1 |
| l3leaf | DC-LEAF3 | Ethernet56/1 | mlag_peer | DC-LEAF4 | Ethernet56/1 |
| l3leaf | DC-LEAF4 | Ethernet49/1 | spine | DC-SPINE1 | Ethernet4/1 |
| l3leaf | DC-LEAF4 | Ethernet50/1 | spine | DC-SPINE2 | Ethernet4/1 |

# Fabric IP Allocation

## Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 10.0.0.0/23 | 512 | 24 | 4.69 % |

## Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| DC-BORDER1 | Ethernet49/1 | 10.0.1.249/31 | DC-SPINE1 | Ethernet7/1 | 10.0.1.248/31 |
| DC-BORDER1 | Ethernet50/1 | 10.0.1.251/31 | DC-SPINE2 | Ethernet7/1 | 10.0.1.250/31 |
| DC-BORDER2 | Ethernet49/1 | 10.0.1.253/31 | DC-SPINE1 | Ethernet8/1 | 10.0.1.252/31 |
| DC-BORDER2 | Ethernet50/1 | 10.0.1.255/31 | DC-SPINE2 | Ethernet8/1 | 10.0.1.254/31 |
| DC-LEAF1 | Ethernet49/1 | 10.0.0.1/31 | DC-SPINE1 | Ethernet1/1 | 10.0.0.0/31 |
| DC-LEAF1 | Ethernet50/1 | 10.0.0.3/31 | DC-SPINE2 | Ethernet1/1 | 10.0.0.2/31 |
| DC-LEAF2 | Ethernet49/1 | 10.0.0.5/31 | DC-SPINE1 | Ethernet2/1 | 10.0.0.4/31 |
| DC-LEAF2 | Ethernet50/1 | 10.0.0.7/31 | DC-SPINE2 | Ethernet2/1 | 10.0.0.6/31 |
| DC-LEAF3 | Ethernet49/1 | 10.0.0.9/31 | DC-SPINE1 | Ethernet3/1 | 10.0.0.8/31 |
| DC-LEAF3 | Ethernet50/1 | 10.0.0.11/31 | DC-SPINE2 | Ethernet3/1 | 10.0.0.10/31 |
| DC-LEAF4 | Ethernet49/1 | 10.0.0.13/31 | DC-SPINE1 | Ethernet4/1 | 10.0.0.12/31 |
| DC-LEAF4 | Ethernet50/1 | 10.0.0.15/31 | DC-SPINE2 | Ethernet4/1 | 10.0.0.14/31 |

## Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 10.255.252.0/28 | 16 | 2 | 12.5 % |
| 10.255.254.0/24 | 256 | 6 | 2.35 % |

## Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| DC_FABRIC | DC-BORDER1 | 10.255.254.127/32 |
| DC_FABRIC | DC-BORDER2 | 10.255.254.128/32 |
| DC_FABRIC | DC-LEAF1 | 10.255.254.1/32 |
| DC_FABRIC | DC-LEAF2 | 10.255.254.2/32 |
| DC_FABRIC | DC-LEAF3 | 10.255.254.3/32 |
| DC_FABRIC | DC-LEAF4 | 10.255.254.4/32 |
| DC_FABRIC | DC-SPINE1 | 10.255.252.1/32 |
| DC_FABRIC | DC-SPINE2 | 10.255.252.2/32 |

## VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 10.255.255.0/24 | 256 | 6 | 2.35 % |

## VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| DC_FABRIC | DC-BORDER1 | 10.255.255.127/32 |
| DC_FABRIC | DC-BORDER2 | 10.255.255.127/32 |
| DC_FABRIC | DC-LEAF1 | 10.255.255.1/32 |
| DC_FABRIC | DC-LEAF2 | 10.255.255.1/32 |
| DC_FABRIC | DC-LEAF3 | 10.255.255.3/32 |
| DC_FABRIC | DC-LEAF4 | 10.255.255.3/32 |
