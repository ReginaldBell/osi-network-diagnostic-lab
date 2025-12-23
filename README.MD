# OSI-Based Network & System Diagnostic Lab

## Overview
This project demonstrates systematic network and system troubleshooting using the OSI model in a virtualized environment.  
Failures are intentionally introduced on a Linux system and observed from a Windows endpoint to map root cause to user-facing impact.

## Environment
- Hypervisor: VMware Workstation
- VM-1 (Control): Ubuntu Server 24.04 LTS
- VM-2 (Observer): Windows
- Network Mode: NAT (baseline)

## Methodology
1. Establish a known-good baseline
2. Break one OSI layer at a time
3. Observe symptoms
4. Diagnose root cause
5. Restore service
6. Validate recovery

## Phases
- Phase 0: Baseline validation
- Phase 1: Layer 1 (Physical)
- Phase 2: Layer 2 (Data Link)
- Phase 3: Layer 3 (Network)
- Phase 4: Layer 4 (Transport)
- Phase 5: Layer 7 (Application)
- Phase 7: Post-remediation validation

## Evidence
All screenshots are organized by phase under the `screenshots/` directory.

## Key Takeaways
- OSI-layer isolation improves troubleshooting speed
- User-facing symptoms often originate multiple layers below
- Baseline validation is critical before incident response
