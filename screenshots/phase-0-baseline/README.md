# Incident: Phase 0 – Baseline

## Summary
Initial baseline connectivity and configuration validation for all VMs prior to OSI-layer fault injection.

## Scope
- Affected VM(s): All lab VMs
- OSI layer under test: Baseline (pre-fault)
- What was intentionally broken: Nothing; this is the control state

## Hypothesis
- All network layers are functional
- No misconfigurations present

## Evidence
- screenshots/phase-0-baseline/01_ubuntu_arp_and_external_connectivity.png
- screenshots/phase-0-baseline/02_windows_ipconfig_all.png
- screenshots/phase-0-baseline/03_windows_network_status.png
- screenshots/phase-0-baseline/04_windows_ping_8.8.8.8.png
- screenshots/phase-0-baseline/05_windows_ping_google_com.png
- screenshots/phase-0-baseline/06_windows_browser_access.png

## Analysis
Evidence confirms all OSI layers are operational and baseline connectivity is established. No faults detected.

## Root Cause
N/A – Baseline phase, no incident.

## Remediation
N/A

## Validation
All connectivity and configuration checks pass as expected.

## Lessons Learned
- Establishing a baseline is critical for comparative diagnostics.
