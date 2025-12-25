# OSI-Based Network & System Diagnostic Lab

## Production-Grade Failure Isolation Using OSI Layer Methodology

---

### 🎯 Executive Value Proposition

I isolate complex network and security failures using the same evidence-driven, OSI-layer methodology employed by SOC Tier 2/3, NOC, and SRE teams in production environments.

This lab intentionally injects failures across the OSI stack and restores service using:

- **Layer-by-layer isolation** (no guesswork)
- **Audit-ready evidence** (timestamps, command output, artifacts)
- **Minimal blast radius remediation** (single-variable changes only)

**Outcome:** Each failure is isolated to a single OSI layer, remediated with minimal blast radius, and validated end-to-end using repeatable evidence.

This lab is a controlled incident-response simulation designed to mirror real on-call troubleshooting workflows used by SOC, NOC, and SRE teams.

---

## ✅ What This Project Proves

- Diagnose outages systematically, not reactively
- Validate lower layers before higher layers
- Collect packet captures, command output, and service-state evidence before proposing fixes
- Apply change-controlled, minimal-scope remediation
- Distinguish service health vs transport security
- Reason in root cause attribution, not symptoms

---

## 🏗️ Lab Architecture (High-Level)

| Component         | Platform                | Purpose                                   | Trust Boundary      |
|-------------------|------------------------|-------------------------------------------|---------------------|
| Diagnostic Host   | Ubuntu 24.04 LTS       | Client-side testing, packet capture, evidence collection | Untrusted client    |
| Target Service    | Windows Server 2022 (IIS) | HTTPS, DNS, application-layer failure injection | Trusted service     |
| Network Fabric    | VMware NAT (192.168.22.0/24) | Isolated, reproducible network           | Controlled perimeter|

**Design Rationale:**
- Linux provides low-level, deterministic diagnostics (tcpdump, openssl, ip, iptables)
- Windows mirrors real enterprise service endpoints
- NAT ensures a stable baseline with no external dependencies
- Phase locking prevents cross-layer ambiguity

---

## 🧠 OSI Phase Model & Real-World Analogs

Each phase is locked. Lower layers must be validated before progressing upward.

| Phase | OSI Layer    | Failure Injected      | Primary Diagnostic Signal | Real-World Analog         |
|-------|--------------|----------------------|--------------------------|---------------------------|
| 0     | Baseline     | None                 | Known-good validation    | Production sanity check   |
| 1     | Physical     | Interface disabled   | ip link down             | Cable / SFP failure       |
| 2     | Data Link    | ARP poisoning        | arp -an MAC mismatch     | Switch MAC corruption     |
| 3     | Network      | Invalid gateway      | ip route, traceroute     | Route propagation error   |
| 4     | Transport    | TCP/443 blocked      | SYN timeout / RST        | Firewall / Security Group |
| 5     | Application  | DNS resolution failure | SERVFAIL / NXDOMAIN    | Route 53 / Azure DNS      |
| 6     | Presentation | TLS binding removed  | openssl s_client failure | ALB / App Service TLS     |
| 7     | Validation   | End-to-end verification | Full request success  | SLA confirmation          |

**Key Insight:**
Success at Layer 3 does not guarantee functionality at Layer 7.

Phase 7 intentionally injects no failures and exists solely to confirm post-remediation stability and security posture.

---

## 🚀 Quickstart: Reproduce a Real TLS Failure (Layer 6)

This example demonstrates a common production outage: the service is running, but secure transport is broken.

**Inject (Windows Server):**
- Remove the HTTPS binding in IIS
- Leave the IIS service running

**Diagnose (Linux):**
```sh
openssl s_client -connect 192.168.22.131:443
```

**Analyze:**
- ICMP & routing succeed → Layers 1–3 healthy
- TCP reachable (or previously reachable) → Layer 4 verified
- TLS handshake fails → Layer 6 isolated

**Remediate (Correct Fix):**
- Restore the HTTPS binding in IIS with a valid certificate

**Validate TLS handshake:**
```sh
openssl s_client -connect 192.168.22.131:443
```

**Validate:**
```sh
curl https://192.168.22.131
```

> Note: `-k` was used earlier only to observe certificate trust behavior, not to validate remediation success.

---

**📂 Full evidence and RCA:**
issues/phase-06-https-session-establishment/

---

## 🔍 Methodology Guarantees

- **Phase Locking:** No out-of-order fixes
- **Single-Variable Changes:** One action per iteration
- **Evidence-First Reasoning:** No assumptions
- **Confidence Levels:** High / Medium / Low per root cause
- **Audit Readiness:** Before/after validation for every fix

---
## 📂 Repository Navigation

osi-diagnostic-lab/
├── README.md                     # Executive overview
├── issues/                       # Phase-by-phase RCAs & remediation
│   ├── phase-05-dns-resolution-failure/
│   ├── phase-06-https-session-establishment/
│   └── phase-07-validation/
├── screenshots/                  # Visual proof of failure & recovery
├── notes/                        # OSI tables & diagnostic references

> Deep command output, logs, and screenshots intentionally live outside the README to preserve clarity.

---

## 🧪 Skills Demonstrated

| Skill                | How It’s Proven                                 |
|----------------------|-------------------------------------------------|
| OSI Isolation        | Strict layer validation, no cross-layer fixes    |
| Root Cause Analysis  | Evidence-backed conclusions with confidence     |
| Change Control       | Single-variable remediation only                |
| Failure Containment  | Service degradation isolated without collateral impact |
| Incident Documentation | Timestamped logs, screenshots, tables         |
| Transport Security   | TLS handshake failure isolation                 |
| Production Thinking  | MTTR tracking, blast-radius minimization        |

---

## 🎤 Interview Usage

**Weak framing:**

> “I did a networking lab.”

**Strong framing:**

> “I built a production-style OSI diagnostic lab that simulates real enterprise outages. Each failure is isolated with evidence, remediated with minimal scope, and validated at the exact OSI layer where it occurred. This mirrors how SOC and SRE teams reduce MTTR in production.”

---

## 🤝 Contributing

Contributions are welcome if they follow these rules:

- Clear OSI-layer attribution
- Single-variable failure injection
- Timestamped validation evidence
- Minimal-scope remediation only

Pull requests violating phase locking or introducing cross-layer fixes will be rejected.

See CONTRIBUTING.md.
