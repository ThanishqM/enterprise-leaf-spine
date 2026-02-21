# Enterprise Leaf–Spine Network Deployment (OSPF Underlay + eBGP Overlay)

This project documents the design and configuration of a scalable leaf–spine data center network using:
- **OSPF** as the L3 underlay for fast convergence and simple fabric reachability
- **eBGP** as the overlay control plane for route distribution between leaf switches and spines
- **ECMP** via leaf-to-spine dual-homing for resiliency and load sharing

## Goals
- Build a repeatable leaf–spine design (2 spines, 2 leafs)
- Provide clean configs with an IP plan, ASN plan, and validation steps
- Document failover testing (link-down and device-down scenarios) and expected behavior

## Topology (Logical)
- Spine layer: `spine1`, `spine2`
- Leaf layer: `leaf1`, `leaf2`
- Each leaf connects to both spines (full-mesh leaf-to-spine)

## Protocol Strategy
### Underlay (OSPF)
- OSPF runs on all leaf-to-spine links
- Loopbacks are advertised in OSPF for stable router IDs and reachability

### Overlay (eBGP)
- Spines act as a simple “fabric transit” for route exchange
- Leafs peer eBGP to both spines using loopbacks (multi-hop)
- Leafs advertise local “server VLAN” prefixes into BGP

## Repository Contents
- `ip-plan.md` — addressing, ASNs, and interface plan
- `configs/` — per-device configuration templates
- `validation-tests.md` — step-by-step verification and failover procedures

## Notes
- These configurations are written in an **Arista EOS-style format** (readable and industry-aligned).
- Adjust interface names and platform syntax if deploying on different NOS/vendors (Cisco/Juniper/FRR).