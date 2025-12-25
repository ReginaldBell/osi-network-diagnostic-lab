# Incident: Phase 4 – Layer 4 (Transport)

## Summary
Transport layer connectivity was intentionally blocked to validate detection and recovery from TCP/UDP faults.

## Scope
- Affected VM(s): Linux VM
- OSI layer under test: Layer 4 (Transport)
- What was intentionally broken: TCP port blocked

## Hypothesis
- Firewall rule blocking port
- Service not listening
- Network ACL misconfiguration

## Evidence
- screenshots/phase-4-layer4/01_linux_phase4_baseline_connectivity.png
- screenshots/phase-4-layer4/02_linux_phase4_https_baseline.png
- screenshots/phase-4-layer4/03_linux_phase4_tcp_block_failure.png
- screenshots/phase-4-layer4/04_linux_phase4_routing_and_arp_healthy.png
- screenshots/phase-4-layer4/05_linux_phase4_recovery_success.png

## Analysis
TCP connectivity tests and firewall inspection confirm Layer 4 disruption and recovery.

## Root Cause
Firewall rule was added to block TCP port, simulating a transport layer failure.

## Remediation
Removed firewall rule to restore transport connectivity.

## Validation
TCP connection and service checks confirm restoration.

## Lessons Learned
- Transport layer faults are best diagnosed with port and service checks.
