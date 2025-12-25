
# OSI-Based Network & System Diagnostic Lab
## Production-Grade Troubleshooting Methodology

---

## 📊 Quick Reference

| Component | Value |
|-----------|-------|
| **Lab Type** | OSI Layer Failure Isolation |
| **Methodology** | Evidence-based, single-variable diagnostics |
| **Industry Alignment** | SOC Tier 2/3, NOC Operations, SRE Incident Response |
| **Total Phases** | 8 (Baseline + 7 OSI layers) |
| **Environment** | VMware Workstation, Ubuntu 24.04 LTS, Windows Server |

---

## 🎯 Executive Value Proposition

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

> **OSI dependency flow and failure propagation are documented in [notes/osi-reference.md](notes/osi-reference.md).**

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
├── interview/                    # STAR interview narratives

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

> STAR-formatted interview narratives are included under [interview/](interview/).

## 📋 Enhanced Phase Summary Table

| Phase | OSI Layer | Failure Type | Root Cause | MTTR | Confidence |
|-------|-----------|--------------|------------|------|------------|
| 0 | Baseline | None | N/A | N/A | N/A |
| 1 | Physical (L1) | Link state down | Interface disabled | <1 min | High |
| 2 | Data Link (L2) | ARP poisoning | Static neighbor entry | 2 min | High |
| 3 | Network (L3) | Routing failure | Wrong default gateway | 2 min | High |
| 4 | Transport (L4) | Port blocking | iptables DROP on TCP/443 | 3 min | High |
| 5 | Application (L7) | DNS resolution | Missing resolv.conf | 2 min | High |
| 6 | Presentation (L6) | TLS handshake | Missing HTTPS binding | 5 min | High |
| 7 | Validation | End-to-end | N/A (verification only) | 5 min | N/A |

**Total Lab MTTR:** ~20 minutes (all phases)

---

## 🔍 PHASE 6: PRESENTATION LAYER (TLS/HTTPS) — REWRITTEN

### **Phase Metadata**

| Attribute | Value |
|-----------|-------|
| **OSI Layer** | Layer 6 (Presentation) |
| **Objective** | Isolate TLS binding failure from service availability |
| **Timestamp** | 2025-12-25 12:10:46 UTC |
| **Confidence Level** | **HIGH** |
| **MTTR** | ~5 minutes |
| **Real-World Analog** | IIS certificate expiration, AWS ALB listener misconfiguration, Azure App Service TLS binding removal |

---

### 🟢 **Baseline State (Known-Good)**

**Pre-Failure Validation:**

```bash
# Linux Host: Verify HTTPS reachable
linuxdst:~$ nc -zv 192.168.22.131 443
Connection to 192.168.22.131 443 port [tcp/https] succeeded!

# Linux Host: Verify TLS handshake
linuxdst:~$ openssl s_client -connect 192.168.22.131:443 -showcerts
CONNECTED(00000003)
depth=0 CN = WIN-SERVER
verify return:1
---
Certificate chain
 0 s:CN = WIN-SERVER
   i:CN = WIN-SERVER
---
Server certificate
-----BEGIN CERTIFICATE-----
[...certificate data...]
-----END CERTIFICATE-----
---
SSL handshake has read 1234 bytes
TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
```

**Windows Host: Verify IIS HTTPS Binding**

```powershell
PS C:\> Get-WebBinding -Name "Default Web Site" | Where-Object {$_.protocol -eq "https"}

protocol  bindingInformation     sslFlags
--------  ------------------     --------
https     *:443:                 0
```

**Browser Validation:**
- URL: `https://192.168.22.131`
- Result: ✅ Page loads successfully
- Certificate: Self-signed, accepted by browser

**Artifact:** `screenshots/phase6-baseline-success.png`

---

### ⚠️ **Failure Injection — Presentation Layer**

**Exact Action Taken:**

1. Opened **IIS Manager** → Sites → Default Web Site → Bindings
2. Selected **HTTPS binding (port 443)**
3. Clicked **Remove** (leaving HTTP on port 80 active)
4. IIS service (W3SVC) remained running throughout

**Why This Targets Layer 6:**
- ✅ Service availability intact (Layer 7 — IIS still running)
- ✅ Port reachable via TCP (Layer 4 — network stack healthy)
- ❌ TLS handshake impossible (Layer 6 — no certificate presented)

**Expected Behavior:**
- TCP connection should succeed (Layer 4)
- TLS negotiation should fail (Layer 6)

---

### 📊 **Evidence Collection**

**Evidence 1: Port Reachability (Layer 4 Health Check)**

```bash
linuxdst:~$ nc -zv 192.168.22.131 443
nc: connect to 192.168.22.131 port 443 (tcp) failed: Connection refused
```

**Interpretation:**
- Port 443 not listening → HTTPS binding removed
- Layer 4 reachable, but no listener present
- This confirms Layer 6 failure (no TLS endpoint)

---

**Evidence 2: TLS Handshake Attempt**

```bash
linuxdst:~$ openssl s_client -connect 192.168.22.131:443 -showcerts
connect: Connection refused
connect:errno=111
```

**Interpretation:**
- No TLS handshake possible
- Connection refused at TCP layer because no HTTPS listener exists
- Layer 6 (Presentation) is the root cause

---

**Evidence 3: Windows Port State**

```powershell
PS C:\> netstat -ano | findstr :443
# (no output — port not listening)

PS C:\> Get-WebBinding -Name "Default Web Site"
protocol  bindingInformation
--------  ------------------
http      *:80:
# (HTTPS binding missing)
```

**Interpretation:**
- IIS service running (Get-Service W3SVC shows "Running")
- HTTP binding present, HTTPS binding absent
- Confirms Layer 6 misconfiguration, not service failure

---

**Evidence 4: Browser Validation**

**Action:** Navigate to `https://192.168.22.131` in browser

**Result:**
```
ERR_CONNECTION_REFUSED
This site can't be reached
192.168.22.131 refused to connect.
```

**Artifact:** `screenshots/phase6-browser-failure.png`

---

### 🧠 **Root Cause Determination**

**Root Cause:**
HTTPS binding removed from IIS, eliminating Layer 6 (TLS) presentation capability while leaving Layer 7 (IIS service) operational.

**Why High Confidence:**
1. ✅ IIS service confirmed running (Layer 7 healthy)
2. ✅ HTTP binding present and functional (Layer 7 healthy)
3. ✅ Port 443 not listening (Layer 6 missing)
4. ✅ TLS handshake impossible (Layer 6 failure)
5. ✅ Failure isolated to HTTPS only (Layer 6 scope confirmed)

**What Was Eliminated:**
- ❌ Network connectivity (ping successful)
- ❌ Routing issues (Layer 3 verified healthy)
- ❌ Firewall blocking (port 80 accessible)
- ❌ Service failure (IIS running, HTTP functional)
- ❌ Certificate expiration (no certificate presented at all)

**Confidence Level:** **HIGH**

---

### 🔧 **Remediation — Minimal-Scope Fix**

**Action Taken:**

1. Opened **IIS Manager** → Sites → Default Web Site → Bindings
2. Clicked **Add**
3. Selected:
   - Type: `https`
   - IP Address: `All Unassigned`
   - Port: `443`
   - SSL Certificate: `WIN-SERVER` (self-signed)
4. Clicked **OK**
5. **No IIS restart required** (binding change applied immediately)

**Why This Is Minimal-Scope:**
- Only restored the missing HTTPS binding
- No changes to firewall, routing, DNS, or application logic
- No modifications to other services or layers

**Timestamp:** 2025-12-25 12:15:32 UTC

---

### ✅ **Validation — Full Recovery Confirmed**

**Validation 1: Port Listening State**

```powershell
PS C:\> netstat -ano | findstr :443
TCP    0.0.0.0:443            0.0.0.0:0              LISTENING       4
```

**Result:** ✅ Port 443 now listening

---

**Validation 2: TLS Handshake**

```bash
linuxdst:~$ openssl s_client -connect 192.168.22.131:443 -showcerts
CONNECTED(00000003)
depth=0 CN = WIN-SERVER
verify return:1
---
SSL handshake has read 1234 bytes
TLSv1.3, Cipher is TLS_AES_256_GCM_SHA384
---
```

**Result:** ✅ TLS handshake successful

---

**Validation 3: HTTPS Application Access**

```bash
linuxdst:~$ curl -k https://192.168.22.131
<!DOCTYPE html>
<html>
<head><title>IIS Windows Server</title></head>
<body>
<h1>Welcome to IIS</h1>
</body>
</html>
```

**Result:** ✅ HTTPS content retrieved successfully

---

**Validation 4: Browser Access**

**Action:** Navigate to `https://192.168.22.131`

**Result:** ✅ Page loads with self-signed certificate warning (expected)

**Artifact:** `screenshots/phase6-validation-success.png`

---

### 📊 **Before/After Comparison**

| Check | Before (Failed) | After (Restored) |
|-------|----------------|------------------|
| `nc -zv 192.168.22.131 443` | Connection refused | Connection succeeded |
| `openssl s_client -connect ...` | Connection refused | Handshake complete |
| Browser HTTPS | ERR_CONNECTION_REFUSED | Page loads |
| IIS HTTPS Binding | Missing | Present |
| Port 443 State | Not listening | LISTENING |

---

### 🎓 **Key Takeaways — Phase 6**

1. **Service Running ≠ Secure Transport Available**
   - IIS can be operational while HTTPS is broken
   - Layer 7 (Application) health does not guarantee Layer 6 (Presentation) functionality

2. **TLS Failures Are Silent to Lower Layers**
   - Network reachability tests (ping, Layer 3) pass
   - TCP connectivity (Layer 4) may appear healthy
   - Only Layer 6-specific tools reveal the failure

3. **Real-World Analogs:**
   - AWS ALB listener removed but target group healthy
   - Azure App Service TLS binding disabled
   - Kubernetes Ingress missing TLS secret
   - Certificate expired but service running

4. **Why This Matters in Production:**
   - Monitoring tools that only check service status will miss this
   - Users experience immediate outage while dashboards show "green"
   - Requires Layer 6-specific validation (TLS handshake checks)

---

### 📎 **Evidence Artifacts**

- `screenshots/phase6-baseline-success.png` — Healthy HTTPS before failure
- `screenshots/phase6-browser-failure.png` — Browser ERR_CONNECTION_REFUSED
- `screenshots/phase6-iis-binding-missing.png` — IIS Manager showing no HTTPS
- `screenshots/phase6-validation-success.png` — Restored HTTPS functionality
- `logs/phase6-openssl-output.log` — Complete TLS handshake logs

---

## 🎤 INTERVIEW DISCUSSION GUIDE (STAR FORMAT)

### **Phase 1: Physical Layer (Link State)**

**Situation:**
"In a controlled lab environment, I needed to demonstrate how physical layer failures cascade through the entire network stack and understand what evidence proves a Layer 1 issue versus higher-layer problems."

**Task:**
"My objective was to intentionally disable network connectivity at the physical layer, observe the complete failure of all dependent services, and prove through systematic evidence collection that no higher-layer troubleshooting would be relevant until Layer 1 was restored."

**Action:**
"I disabled the network interface using `ip link set ens33 down`, then collected evidence showing:
- Absence of IP address assignment
- Empty ARP neighbor table
- Failed connectivity tests to local and remote targets
- Complete loss of all network services

I documented each observation with timestamps and command output. After re-enabling the interface with `ip link set ens33 up`, I validated full recovery by confirming DHCP assignment, ARP population, and end-to-end connectivity."

**Result:**
"I successfully proved that Layer 1 failures prevent all higher-layer functionality, establishing a baseline understanding that physical connectivity is the foundation for all network operations. This methodology directly applies to production scenarios like interface flapping, cable faults, or misconfigured port channels. The systematic evidence collection I used—checking link state before investigating routing or DNS—is the same approach used in enterprise NOC escalations."

---

### **Phase 2: Data Link Layer (ARP Poisoning)**

**Situation:**
"After validating Layer 1 connectivity, I designed a scenario to isolate Layer 2 failures and demonstrate how address resolution problems can break networking even when IP configuration and routing appear correct."

**Task:**
"My goal was to introduce an ARP poisoning attack by creating a static neighbor entry with an incorrect MAC address, then prove the failure occurred at Layer 2 by showing that Layer 3 (routing) remained healthy while all traffic failed."

**Action:**
"I injected a poisoned ARP entry using `ip neighbor add 192.168.22.2 lladdr 00:00:00:00:00:99 dev ens33`, targeting the default gateway. I collected evidence showing:
- Valid IP configuration and routing table
- Incorrect MAC address in ARP cache
- Complete packet loss to gateway and external hosts
- Failed application connectivity despite correct Layer 3 setup

I then removed the static entry with `ip neighbor del`, allowing dynamic ARP resolution to restore the correct MAC address. Validation showed immediate recovery of connectivity."

**Result:**
"This demonstrated a critical principle: valid IP configuration does not guarantee connectivity when Layer 2 address resolution is incorrect. This scenario mirrors real-world incidents I've studied, including ARP spoofing attacks, switch MAC table corruption, and VLAN misconfigurations. The methodology—verifying Layer 3 health while isolating Layer 2 as the root cause—is essential for SOC analysts investigating potential man-in-the-middle attacks or network engineers troubleshooting intermittent connectivity."

---

### **Phase 3: Network Layer (IP Routing)**

**Situation:**
"With Layer 1 and Layer 2 validated as healthy, I needed to demonstrate how routing failures at Layer 3 can break external connectivity while lower layers remain fully operational."

**Task:**
"My objective was to misconfigure the default gateway to simulate a routing failure, then prove through evidence that Layer 2 (ARP) remained healthy while Layer 3 (routing) became the sole point of failure."

**Action:**
"I replaced the valid default gateway (192.168.22.2) with an unreachable target (192.168.22.99) using `ip route replace default via 192.168.22.99`. I systematically collected evidence showing:
- Local gateway still reachable via ICMP (Layer 2 healthy)
- ARP table correctly populated (Layer 2 functioning)
- External connectivity failed (Layer 3 routing broken)
- Traceroute confirmed traffic sent to wrong gateway

After restoring the correct gateway with `ip route replace default via 192.168.22.2`, I validated full connectivity to external hosts."

**Result:**
"This phase proved that lower-layer health does not guarantee higher-layer functionality. Routing operates independently of MAC address resolution. This directly applies to production scenarios like incorrect route propagation in BGP, VPN split-tunnel misconfigurations, or cloud route table errors in AWS/Azure. The evidence-based approach—showing that Layer 2 was healthy while Layer 3 failed—is the same methodology used in enterprise network operations and cloud networking troubleshooting."

---

### **Phase 4: Transport Layer (TCP Port Blocking)**

**Situation:**
"After validating Layers 1–3, I needed to demonstrate how transport-layer controls can selectively block application traffic even when network routing is fully functional."

**Task:**
"My objective was to introduce a port-based firewall rule targeting HTTPS (TCP/443) while leaving ICMP unrestricted, then prove the failure occurred at Layer 4 by showing that Layer 3 reachability tests succeeded while application connectivity failed."

**Action:**
"I applied an iptables rule using `iptables -A OUTPUT -p tcp --dport 443 -j DROP` to block outbound HTTPS traffic. I collected evidence showing:
- Successful ICMP connectivity to external hosts (Layer 3 healthy)
- Failed HTTPS requests with connection timeouts (Layer 4 blocked)
- Port-specific enforcement confirmed via tcpdump
- HTTP (port 80) remained functional, proving selective blocking

After removing the rule with `iptables -D OUTPUT -p tcp --dport 443 -j DROP`, I validated immediate HTTPS restoration."

**Result:**
"This demonstrated a critical distinction: network reachability at Layer 3 does not guarantee application connectivity when transport-layer controls are enforced. This mirrors real-world scenarios like AWS security group misconfigurations, endpoint firewall policies blocking legitimate traffic, or cloud NAT gateway port exhaustion. The methodology I used—showing successful pings while HTTPS failed—is essential for troubleshooting modern zero-trust architectures where port-level enforcement is common."

---

### **Phase 5: Application Layer (DNS Resolution)**

**Situation:**
"With all transport-layer connectivity validated, I designed a scenario to isolate application-layer name resolution failures and demonstrate how DNS problems break user access even when IP connectivity is fully functional."

**Task:**
"My objective was to introduce a DNS resolution failure by misconfiguring the resolver, then prove the failure occurred at Layer 7 by showing that direct IP connectivity succeeded while hostname-based access failed."

**Action:**
"I simulated DNS failure by clearing `/etc/resolv.conf`, removing all nameserver entries. I collected evidence showing:
- Failed DNS resolution for public hostnames
- Successful ICMP to public IPs (8.8.8.8)
- Direct IP-based HTTP requests succeeded
- Hostname-based requests failed with 'Could not resolve host'

After restoring DNS configuration with valid nameservers, I validated immediate recovery of name resolution."

**Result:**
"This proved that Layer 7 failures are independent of lower-layer health. Users experience complete service outages even when the network is fully operational. This scenario mirrors real-world incidents like recursive resolver failures, DNS zone file corruption, or cloud DNS service outages in Route 53 or Azure DNS. The evidence-based approach—showing successful IP connectivity while DNS failed—is the same methodology used in SOC incident response when investigating potential DNS hijacking or misconfigured corporate resolvers."

---

### **Phase 6: Presentation Layer (TLS/HTTPS)**

**Situation:**
"After validating all layers up through application-level name resolution, I needed to isolate presentation-layer TLS failures and demonstrate how encryption/certificate issues can break secure access even when services are running."

**Task:**
"My objective was to remove the HTTPS binding from IIS while leaving the service operational, then prove the failure occurred at Layer 6 by showing that the HTTP service remained healthy while TLS handshakes became impossible."

**Action:**
"I removed the HTTPS binding from IIS using the IIS Manager interface, leaving only HTTP on port 80 active. I collected evidence showing:
- IIS service (W3SVC) remained running
- HTTP access on port 80 succeeded
- Port 443 stopped listening (netstat showed no listener)
- TLS handshake attempts failed with 'Connection refused'
- Browser HTTPS access showed ERR_CONNECTION_REFUSED

After restoring the HTTPS binding with the correct certificate, I validated successful TLS handshakes and browser access."

**Result:**
"This demonstrated a critical distinction: service availability does not guarantee secure transport availability. Layer 7 (IIS) was healthy while Layer 6 (TLS) was broken. This mirrors real-world production incidents like expired certificates, AWS ALB listener misconfigurations, Azure App Service TLS binding removal, or Kubernetes Ingress missing TLS secrets. The methodology I used—proving service health while isolating TLS failure—is essential for SRE incident response in modern environments where certificate management and TLS enforcement are critical security controls."

---

## 🚨 Common Pitfalls & Production Lessons

### **Pitfall 1: Assuming ICMP Success Means Full Connectivity**
**Reality:** ICMP (ping) operates at Layer 3. Success only proves IP routing, not transport or application functionality.

**Production Example:** AWS security group allows ICMP but blocks TCP/443 → pings succeed, HTTPS fails.

**Lesson:** Always validate at the layer where the application operates.

---

### **Pitfall 2: Skipping Lower-Layer Validation**
**Reality:** DNS failures often trigger when routing is actually broken.

**Production Example:** Engineer troubleshoots DNS for 30 minutes before realizing default gateway is wrong.

**Lesson:** Always validate from Layer 1 upward when diagnosing new issues.

---

### **Pitfall 3: Confusing Service Status with Transport Availability**
**Reality:** A service can be "running" while its secure transport is broken.

**Production Example:** IIS shows "Running" in Task Manager, but HTTPS binding is missing → users see outage, monitoring shows "green."

**Lesson:** Service health checks must validate the specific transport (HTTP vs HTTPS vs TLS handshake).

---

### **Pitfall 4: Changing Multiple Variables Simultaneously**
**Reality:** Applying multiple fixes masks the root cause and creates audit gaps.

**Production Example:** Engineer restarts service, flushes DNS, and reboots server simultaneously → cannot prove which action fixed the issue.

**Lesson:** Single-variable changes enable root cause attribution and prevent recurrence.

---

## 📊 Quantified Skills Matrix

| Skill | Demonstrated How | Real-World Application |
|-------|------------------|------------------------|
| **OSI Layer Isolation** | Strict phase-locking, no cross-layer fixes | Prevents misdiagnosis and wasted effort |
| **Evidence-Based Reasoning** | Every conclusion backed by command output | Audit-ready incident reports |
| **Minimal-Scope Remediation** | Single-variable changes only | Reduces blast radius, enables rollback |
| **Change Control** | One phase, one layer, one fix | Mirrors enterprise CAB workflows |
| **Root Cause Attribution** | Confidence levels, eliminated causes | Prevents recurrence, informs postmortems |
| **Production Rigor** | Timestamps, artifacts, validation | Resume/interview-grade documentation |

---

## 🎯 Interview Preparation Tips

### **How to Discuss This Lab**

**❌ Weak Approach:**
"I worked on a networking lab where I broke things and fixed them."

**✅ Strong Approach:**
"I designed a systematic OSI-based diagnostic lab to build production-grade troubleshooting instincts. Each phase isolated a single layer failure, collected evidence to prove root cause, and applied minimal-scope fixes with validation. This mirrors the methodology used in enterprise NOC escalations and SRE incident response."

---

### **Sample Interview Questions & Responses**

**Q: "Tell me about a time you diagnosed a complex network issue."**

**A (using Phase 3):**
"In a controlled lab, I simulated a routing failure by misconfiguring the default gateway. I systematically ruled out Layer 1 and Layer 2 by confirming link state and ARP resolution, then proved the failure was at Layer 3 by showing the local gateway was reachable but external traffic failed. This evidence-based approach—eliminating possibilities before proposing fixes—is the same methodology I'd apply to production incidents like cloud route table errors or BGP misconfiguration."

---

**Q: "How do you approach troubleshooting when users report 'the internet is down'?"**

**A:**
"I follow OSI-layer validation from bottom-up:
1. **Layer 1:** Check link state (interface up?)
2. **Layer 2:** Verify ARP resolution (correct MAC?)
3. **Layer 3:** Confirm routing (correct gateway?)
4. **Layer 4:** Test transport (firewall blocking ports?)
5. **Layer 7:** Validate application (DNS working?)

This prevents jumping to conclusions. For example, if ICMP succeeds but HTTPS fails, I know it's not a routing issue—I need to investigate Layer 4 (firewall) or Layer 6 (TLS). This systematic approach reduces MTTR and prevents misdiagnosis."

---

**Q: "What's the difference between a service being 'up' and a service being 'reachable'?"**

**A (using Phase 6):**
"A service can be running (Layer 7) while its transport is broken (Layer 6). In my lab, IIS was operational and serving HTTP on port 80, but HTTPS failed because the TLS binding was missing. The service was 'up' but HTTPS was not 'reachable.' This distinction matters in production—monitoring tools that only check service status will miss transport-layer failures like expired certificates or misconfigured listeners."

---

## 📁 Repository Structure

```

osi-diagnostic-lab/
├── README.MD                          # This file
├── notes/
│   └── osi-diagnostic-table.md
├── issues/
│   ├── phase-05-dns-resolution-failure/
│   │   ├── README.md
│   │   └── evidence/
│   │       ├── 01_linux_phase5_dns_baseline.png
│   │       ├── 02_linux_phase5_dns_failure.png
│   │       ├── 03_linux_phase5_dns_bypass_test.png
│   │       ├── 04_linux_phase5_dns_recovery.png
│   │       └── 05_linux_phase5_dns_validation.png
│   ├── phase-06-https-session-establishment/
│   │   ├── README.md
│   │   └── evidence/
│   └── phase-06-windows-https-service/
│       ├── README.md
│       └── evidence/
├── screenshots/
│   ├── phase-0-baseline/
│   ├── phase-1-layer1/
│   ├── phase-2-layer2/
│   ├── phase-3-layer3/
│   ├── phase-4-layer4/
│   ├── phase-5-layer5/
│   ├── phase-6-layer6/
│   └── phase-7-validation/
