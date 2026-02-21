# Validation + Failover Tests

## 1) Underlay Validation (OSPF)
On each device:
- Verify OSPF neighbors are FULL on leaf-to-spine links
- Verify loopback reachability via OSPF

Expected:
- leaf1 can ping spine1/spine2 loopbacks
- leaf2 can ping spine1/spine2 loopbacks
- spines can ping leaf loopbacks

Commands (EOS-style examples):
- `show ip ospf neighbor`
- `show ip route ospf`
- `ping 10.255.x.x`

## 2) Overlay Validation (eBGP)
On leaf1 and leaf2:
- Verify BGP sessions to both spines are Established
- Verify remote leaf server prefix is learned (ECMP possible via both spines)

Expected:
- leaf1 learns `192.168.20.0/24`
- leaf2 learns `192.168.10.0/24`

Commands:
- `show ip bgp summary`
- `show ip bgp`
- `show ip route bgp`

## 3) Failover Test A — Leaf-to-Spine Link Down
Action:
- Shut down leaf1↔spine1 interface

Expected:
- OSPF adjacency drops on that link
- leaf1 remains connected via leaf1↔spine2
- BGP sessions remain Established to spine2
- leaf1 still learns remote prefixes (may lose one ECMP path)

Commands:
- `show ip ospf neighbor`
- `show ip bgp summary`
- `show ip route`

## 4) Failover Test B — Spine Failure
Action:
- Simulate spine1 down

Expected:
- leafs maintain full reachability via spine2
- BGP remains up via spine2
- Traffic continues with reduced redundancy

## 5) Optional Enhancement
- Enable BFD for faster failure detection (platform-dependent)
- Track convergence time using timestamps/logs during failover tests