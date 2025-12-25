# Incident: Phase 5 – Layer 5 (Session)

## Summary
Session layer (DNS) resolution was intentionally broken to validate detection and recovery from name resolution failures.

## Scope
- Affected VM(s): Linux VM
- OSI layer under test: Layer 5 (Session)
- What was intentionally broken: DNS resolver misconfigured

## Hypothesis
- DNS server unreachable
- Resolver misconfiguration
- Firewall blocking DNS

## Evidence
- screenshots/phase-5-layer5/01_linux_phase5_dns_baseline.png
- screenshots/phase-5-layer5/02_linux_phase5_dns_failure.png
- screenshots/phase-5-layer5/03_linux_phase5_dns_bypass_test.png
- screenshots/phase-5-layer5/04_linux_phase5_dns_recovery.png
- screenshots/phase-5-layer5/phase5-dns-name-resolve-recovery.png
- screenshots/phase-5-layer5/phase5-name-resolution-fails.png
- screenshots/phase-5-layer5/phase5_dns_resolution_failure_linux.png

## Analysis
Evidence demonstrates DNS failure, bypass, and recovery, confirming Layer 5 disruption and restoration.

## Root Cause
DNS resolver was intentionally misconfigured, breaking session layer name resolution.

## Remediation
Restored correct DNS configuration and flushed resolver cache.

## Validation
DNS queries and application connectivity confirm restoration.

## Lessons Learned
- DNS failures are a common cause of session layer outages.
