# Issue: HTTPS Listener Missing

## OSI Phase
Phase 06 – Presentation (Blocked)

## Symptoms
- ping to server works
- nc/openSSL timeout to port 443
- No LISTENING process on port 443

## Initial Hypotheses
- HTTPS service not started
- Firewall blocking port 443

## Evidence
- netstat -an | findstr 443 → no LISTENING
- nc -zv server 443 → timeout
- openssl s_client -connect server:443 → connection refused

## Root Cause
No process was listening on port 443 due to missing HTTPS configuration.

## Remediation
- Added HTTPS listener to web server configuration
- Restarted web service

## Validation
- nc -zv server 443 → succeeded
- openssl s_client -connect server:443 → handshake successful

## Lessons Learned
- Always confirm service listeners after configuration changes
- Use netstat and nc for quick listener checks
