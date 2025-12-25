# OSI Reference Table & Dependency Visualization

## OSI Layer Reference Table

| Layer | Name | Protocols/Technologies | Common Real-World Failures |
|-------|------|------------------------|----------------------------|
| **7** | Application | HTTP, DNS, SSH, FTP | Service misconfiguration, authentication failures, DNS poisoning |
| **6** | Presentation | SSL/TLS, certificates, encryption | Expired certificates, cipher mismatch, encoding errors |
| **5** | Session | NetBIOS, RPC, session state | Session timeouts, state corruption, abandoned connections |
| **4** | Transport | TCP, UDP, port control | Firewall blocks, security group misconfig, port exhaustion |
| **3** | Network | IP, ICMP, routing | Wrong gateway, route propagation failure, IP conflicts |
| **2** | Data Link | Ethernet, ARP, MAC, switches | ARP poisoning, MAC table overflow, VLAN mismatch |
| **1** | Physical | Cables, NICs, link state | Interface down, cable fault, duplex mismatch |

## OSI Dependency Visualization

```
┌─────────────────────────────────────────────────────────────┐
│  Layer 7: APPLICATION                                       │
│  ├─ HTTP/HTTPS, DNS, SSH                                    │
│  └─ Depends on: Session, Presentation, Transport            │
└─────────────────────────────────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  Layer 6: PRESENTATION                                      │
│  ├─ TLS/SSL, Certificates, Encoding                         │
│  └─ Depends on: Transport                                   │
└─────────────────────────────────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  Layer 5: SESSION                                           │
│  ├─ Connection State, Session Management                    │
│  └─ Depends on: Transport                                   │
└─────────────────────────────────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  Layer 4: TRANSPORT                                         │
│  ├─ TCP/UDP, Port Control                                   │
│  └─ Depends on: Network                                     │
└─────────────────────────────────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  Layer 3: NETWORK                                           │
│  ├─ IP Routing, ICMP                                        │
│  └─ Depends on: Data Link                                   │
└─────────────────────────────────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  Layer 2: DATA LINK                                         │
│  ├─ ARP, MAC Addressing, Switching                          │
│  └─ Depends on: Physical                                    │
└─────────────────────────────────────────────────────────────┘
                          ▼
┌─────────────────────────────────────────────────────────────┐
│  Layer 1: PHYSICAL                                          │
│  ├─ Link State, NIC Status                                  │
│  └─ Foundation for all upper layers                         │
└─────────────────────────────────────────────────────────────┘
```

**KEY INSIGHT:**
Failure at ANY layer breaks all dependent layers above it. Success at Layer 3 does NOT guarantee Layer 7 functionality.
