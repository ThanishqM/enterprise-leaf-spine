# IP Plan + ASN Plan

## ASN Plan
- Spines: AS **65000**
- Leaf1: AS **65101**
- Leaf2: AS **65102**

## Loopbacks (Router IDs / BGP Update Source)
- spine1 Lo0: **10.255.0.1/32**
- spine2 Lo0: **10.255.0.2/32**
- leaf1  Lo0: **10.255.1.1/32**
- leaf2  Lo0: **10.255.1.2/32**

## Underlay /31 (Leaf ↔ Spine Links)
(Using /31 keeps addressing clean in point-to-point links)

- leaf1 ↔ spine1: **10.0.0.0/31**
  - leaf1: 10.0.0.0/31
  - spine1: 10.0.0.1/31

- leaf1 ↔ spine2: **10.0.0.2/31**
  - leaf1: 10.0.0.2/31
  - spine2: 10.0.0.3/31

- leaf2 ↔ spine1: **10.0.0.4/31**
  - leaf2: 10.0.0.4/31
  - spine1: 10.0.0.5/31

- leaf2 ↔ spine2: **10.0.0.6/31**
  - leaf2: 10.0.0.6/31
  - spine2: 10.0.0.7/31

## Server / VLAN Prefixes (Advertised from Leafs)
- leaf1 server VLAN: **192.168.10.0/24**
- leaf2 server VLAN: **192.168.20.0/24**

## OSPF
- Area: **0**
- Advertise loopbacks + all underlay point-to-point links