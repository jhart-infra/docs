# Cisco FTD ↔ FortiGate IPsec VPN

## Overview
Establishing a route-based IKEv2 tunnel between Cisco FTD (via FMC) and FortiGate (via FortiManager).

![Diagram](../assets/images/ftd-fgt-diagram.png)

## Configuration Steps
1. Define IKEv2 proposals and pre-shared keys.  
2. Create crypto maps and access lists for interesting traffic.  
3. Deploy configuration via FMC and FortiManager.  

## Highlights
- NAT exemption to prevent double encryption  
- Redundant phase 2 selectors for multiple subnets  
- Route-based design with static routes over the tunnel  

## Lessons Learned
- Always verify tunnel interface MTU on both sides.  
- Use identical DH groups and lifetimes for stable negotiation.
