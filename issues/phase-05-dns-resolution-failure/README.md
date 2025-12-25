# Issue: DNS Resolution Failure

## OSI Phase
Phase 05 – DNS (Blocked)

## Symptoms
- resolv.conf pointed to 127.0.0.1
- curl could not resolve domain
- curl by IP succeeded

## Initial Hypotheses
- DNS misconfiguration
- Local resolver not running

## Evidence
- [01_linux_phase5_dns_baseline.png](evidence/01_linux_phase5_dns_baseline.png): Baseline connectivity
- [02_linux_phase5_dns_failure.png](evidence/02_linux_phase5_dns_failure.png): DNS failure (resolvectl status, error)
- [03_linux_phase5_dns_bypass_test.png](evidence/03_linux_phase5_dns_bypass_test.png): curl by IP (bypass DNS)
- [04_linux_phase5_dns_recovery.png](evidence/04_linux_phase5_dns_recovery.png): DNS resolver fix (resolvectl dns, flush)
- [05_linux_phase5_dns_validation.png](evidence/05_linux_phase5_dns_validation.png): Final validation (curl success)

## Root Cause
resolv.conf was incorrectly set to use localhost as DNS, but no local resolver was running.

## Remediation
- Restored correct DNS server in resolv.conf
- Flushed DNS and resolver caches

## Validation
- curl osi-lab.example.com → succeeded
- dig osi-lab.example.com → returns valid A record

## Lessons Learned
- Always verify DNS configuration after network changes
- Document resolver settings for lab environments
