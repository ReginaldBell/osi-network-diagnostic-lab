# Incident: Phase 1 – Layer 1 (Physical)

## Summary
Physical network connectivity was intentionally disrupted to validate detection and recovery at OSI Layer 1.

## Scope
- Affected VM(s): Windows VM
- OSI layer under test: Layer 1 (Physical)
- What was intentionally broken: Network adapter disabled

## Hypothesis
- Physical link failure
- Cable or adapter issue
- VM network interface misconfiguration

## Evidence
- screenshots/phase-1-layer1/01_windows_adapter_disabled.png
- screenshots/phase-1-layer1/02_windows_adapter_enabled.png
- screenshots/phase-1-layer1/03_windows_ipconfig_confirm_link.png
- screenshots/phase-1-layer1/04_windows_adapter_renabled.png
- screenshots/phase-1-layer1/05_windows_arp_cache_before_flush.png
- screenshots/phase-1-layer1/06_windows_arp_flush_and_gateway_ping.png
- screenshots/phase-1-layer1/07_windows_arp_relearned.png
- screenshots/phase-1-layer1/08_windows_phase1_full_baseline_validation.png

## Analysis
Evidence demonstrates loss and restoration of physical connectivity. ARP cache behavior and link status confirm Layer 1 disruption and recovery.

## Root Cause
Network adapter was intentionally disabled, simulating a physical disconnect.

## Remediation
Re-enabled network adapter to restore connectivity.

## Validation
Link status and ARP cache confirm restoration. Baseline validation screenshot included.

## Lessons Learned
- Layer 1 failures are immediately visible via link status and ARP behavior.
