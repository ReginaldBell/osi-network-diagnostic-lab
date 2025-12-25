# Incident: Phase 2 – Layer 2 (Data Link)

## Summary
Layer 2 connectivity was disrupted to test detection and recovery from ARP poisoning and MAC-level faults.

## Scope
- Affected VM(s): Linux VM
- OSI layer under test: Layer 2 (Data Link)
- What was intentionally broken: ARP table poisoned

## Hypothesis
- ARP cache corruption
- MAC address spoofing
- Switch misconfiguration

## Evidence
- screenshots/phase-2-layer2/01_linux_ip_addr_confirm.png.png
- screenshots/phase-2-layer2/02_linux_baseline_ping_success.png
- screenshots/phase-2-layer2/03_linux_arp_before_poison.png
- screenshots/phase-2-layer2/04_linux_arp_poisoned.png
- screenshots/phase-2-layer2/05_linux_recovery_success.png

## Analysis
Evidence shows ARP table state before and after poisoning, with loss and restoration of Layer 2 connectivity.

## Root Cause
Intentional ARP poisoning disrupted Layer 2 communication.

## Remediation
Cleared ARP cache and restored correct MAC associations.

## Validation
Ping and ARP table confirm recovery.

## Lessons Learned
- ARP poisoning is a rapid way to disrupt Layer 2.
