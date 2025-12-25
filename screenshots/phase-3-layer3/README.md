# Incident: Phase 3 – Layer 3 (Network)

## Summary
Layer 3 routing was intentionally misconfigured to validate detection and recovery from network-level faults.

## Scope
- Affected VM(s): Linux VM
- OSI layer under test: Layer 3 (Network)
- What was intentionally broken: Default gateway misconfigured

## Hypothesis
- Incorrect default gateway
- Routing table corruption
- Network segmentation

## Evidence
- screenshots/phase-3-layer3/01_linux_phase3_baseline_routing.png.png
- screenshots/phase-3-layer3/02_linux_phase3_wrong_default_gateway.png
- screenshots/phase-3-layer3/03_linux_phase3_routing_failure.png
- screenshots/phase-3-layer3/04_linux_phase3_arp_still_healthy.png
- screenshots/phase-3-layer3/05_linux_phase3_recovery_success.png

## Analysis
Routing table and connectivity tests confirm Layer 3 disruption and successful remediation.

## Root Cause
Default gateway was intentionally set incorrectly, breaking Layer 3 routing.

## Remediation
Restored correct default gateway in routing table.

## Validation
Ping and routing table confirm restoration.

## Lessons Learned
- Routing misconfigurations are quickly diagnosed with table inspection.
