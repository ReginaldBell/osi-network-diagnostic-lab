# OSI-Based Network & System Diagnostic Lab

## Overview

This lab demonstrates systematic network and system troubleshooting using the OSI model in a controlled virtualized environment. Failures are intentionally introduced at specific OSI layers and observed end-to-end to understand how low-level issues manifest as user-facing symptoms.

Rather than focusing on individual tools, the lab emphasizes **methodology, isolation, and evidence-based diagnosis**—the same approach used in SOC, NOC, and incident response workflows.

### OSI Layer Reference

| Layer | Name | Protocols/Technologies | Common Issues |
|-------|------|------------------------|---------------|
| **7** | Application | HTTP, DNS, SSH, FTP | Service misconfiguration, authentication |
| **6** | Presentation | SSL/TLS, encryption | Certificate errors, encoding issues |
| **5** | Session | NetBIOS, RPC | Session timeouts, state management |
| **4** | Transport | TCP, UDP | Port blocking, firewall rules |
| **3** | Network | IP, ICMP, routing | Routing failures, IP misconfigurations |
| **2** | Data Link | Ethernet, ARP, switches | MAC conflicts, ARP poisoning, VLAN issues |
| **1** | Physical | Cables, NICs, link state | Cable disconnection, interface down |

---

## Objectives

- Apply OSI-layer reasoning to real network failures
- Differentiate physical, data-link, and network-layer issues
- Build repeatable troubleshooting habits using controlled fault injection
- Document failure → impact → recovery with clear evidence

---

## Environment

| Component | Details |
|-----------|---------|
| **Hypervisor** | VMware Workstation |
| **Control Host** | Ubuntu Server 24.04 LTS |
| **Observer Host** | Windows (GUI endpoint) |
| **Network Mode** | NAT (baseline) |

### Design Rationale

- **Linux** provides fine-grained network control for fault injection
- **Windows** represents a realistic end-user/enterprise endpoint
- **NAT** provides a stable, reproducible baseline

---

## Methodology

Each phase follows the same deterministic workflow:

1. **Establish** a known-good baseline
2. **Introduce** a single, intentional failure
3. **Observe** system and user-level symptoms
4. **Identify** the OSI layer responsible
5. **Restore** service
6. **Validate** full recovery

> **Note:** Only one layer is altered at a time to avoid ambiguity and false conclusions.

---

## Phase Overview

| Phase | OSI Layer | Focus |
|-------|-----------|-------|
| Phase 0 | Baseline | Known-good reference state |
| Phase 1 | Layer 1 | Physical/Link state |
| Phase 2 | Layer 2 | ARP/MAC resolution |
| Phase 3 | Layer 3 | IP routing |
| Phase 4 | Layer 4 | Transport (TCP/UDP) |
| Phase 5 | Layer 7 | Application behavior |
| Phase 6 | Validation | Post-remediation verification |

---

## Phase Details

### Phase 0 – Baseline Validation

**Purpose:** Establish a trusted reference state before introducing failures.

**Validated Conditions:**
- Network adapter enabled
- DHCP address assigned
- Default gateway reachable
- External connectivity confirmed
- ARP table populated dynamically

This phase ensures all subsequent failures are intentional and attributable to controlled changes, not environmental instability.

---

### Phase 1 – Physical Layer (Link State)

**Objective:** Demonstrate how loss of physical or logical link state prevents all higher-layer communication.

**Actions Performed:**
1. Disabled network adapter to simulate link loss
2. Verified absence of IP configuration and connectivity
3. Re-enabled adapter to restore link
4. Confirmed IP assignment and network reachability

**Key Observations:**
- Without Layer 1 connectivity, all higher layers fail immediately
- IP, ARP, DNS, and application checks are irrelevant without a link
- Recovery is confirmed only after end-to-end validation

**Outcome:** Successfully demonstrated physical-layer dependency and recovery.

**What This Proves:** All higher-layer protocols depend on physical connectivity; no Layer 1 means no network stack.

---

### Phase 2 – Data Link Layer (ARP/MAC Resolution)

**Objective:** Demonstrate how incorrect ARP/MAC mappings at OSI Layer 2 can break connectivity even when IP configuration and routing appear correct.

**Actions Performed:**
1. Observed baseline ARP neighbor table with valid gateway resolution
2. Introduced an intentional ARP/MAC mismatch using a static neighbor entry
3. Verified complete packet loss despite correct IP address and default gateway
4. Confirmed failure persisted across multiple ICMP targets
5. Restored connectivity by removing the poisoned ARP entry

**Key Observations:**
- Layer 3 (IP) configuration remained valid throughout the failure
- Traffic failed due to incorrect Layer 2 address resolution
- ARP manipulation alone was sufficient to fully disrupt network access

**OSI Mapping:**
- **Layer 2:** ARP/MAC resolution (failure source)
- **Layer 3:** IP routing (verified healthy)
- **Layer 7:** Application reachability (impacted downstream)

**Outcome:** Successfully demonstrated a realistic Layer 2 failure and recovery scenario commonly encountered in enterprise networks and security investigations.

**What This Proves:** Valid IP configuration does not guarantee connectivity when Layer 2 address resolution is incorrect.

---

### Phase 3 – Network Layer (IP Routing)

**Objective:** Demonstrate how incorrect Layer 3 routing breaks external connectivity even when Layer 1 and Layer 2 remain fully functional.

**Actions Performed:**
1. Validated baseline routing state with correct default gateway (192.168.22.2)
2. Introduced intentional routing failure by replacing valid gateway with invalid target (192.168.22.99)
3. Verified local gateway remained reachable while external traffic failed
4. Confirmed ARP table remained healthy throughout the failure
5. Restored correct routing and validated full connectivity recovery

**Key Observations:**
- Layer 3 failures do not necessarily impact Layer 2
- ARP resolution can remain healthy while traffic still fails
- Incorrect routing manifests as external reachability loss, not link failure
- Symptoms can appear "network-wide" while the root cause is a single route

**OSI Mapping:**
- **Layer 1:** Physical link (verified healthy)
- **Layer 2:** ARP/MAC resolution (verified healthy)
- **Layer 3:** IP routing (failure source)
- **Layer 7:** Application reachability (impacted downstream)

**Outcome:** Successfully demonstrated a realistic Layer 3 routing failure and recovery scenario commonly encountered during misconfigured gateways, VPN split-tunnel issues, cloud route table errors, and on-premises network changes.

**What This Proves:** Lower-layer health does not guarantee higher-layer functionality; routing failures occur independently of link and MAC layer operations.

---

### Phase 4 – Transport Layer (TCP/UDP)

**Status:** Planned

---

### Phase 5 – Application Layer

**Status:** Planned

---

### Phase 6 – Post-Remediation Validation

**Status:** Planned

---

## Evidence & Artifacts

All screenshots and command output are organized by phase under the `screenshots/` directory.

Each phase includes:
- Pre-failure baseline evidence
- Failure state evidence
- Recovery validation evidence

---

## Key Takeaways

- **OSI-layer isolation** dramatically improves troubleshooting accuracy
- User-visible outages often originate **below the application layer**
- **Baseline validation** prevents misdiagnosis and wasted remediation effort
- Small, controlled failures provide clearer insight than complex simulations

---

## Why This Lab Matters

This project focuses on **how engineers and analysts think**, not just which commands they run.

This lab is designed for students, junior engineers, and analysts who want to build production-grade troubleshooting instincts rather than tool-specific familiarity.

The same methodology applies to:
- SOC incident triage
- Network operations troubleshooting
- Cloud networking failures
- Zero-trust and segmentation debugging
- Enterprise network security investigations

---

## Getting Started

### Prerequisites
- VMware Workstation (or compatible hypervisor)
- Ubuntu Server 24.04 LTS ISO
- Windows VM image
- Basic understanding of networking concepts

### Setup Instructions
1. Clone this repository
2. Import the provided VM configurations
3. Verify baseline connectivity (Phase 0)
4. Follow each phase sequentially
5. Document your observations

---

## Contributing

If you'd like to contribute additional phases or failure scenarios, please submit a pull request with:
- Clear documentation of the failure mechanism
- Before/after evidence
- OSI layer mapping
- Recovery procedure

---

## License

This project is provided for educational purposes.

---

## Acknowledgments

Built to support hands-on learning for network engineers, security analysts, and SOC professionals pursuing deeper understanding of network fundamentals.
