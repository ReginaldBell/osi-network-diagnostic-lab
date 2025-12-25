# STAR Interview Narrative — Phase 6: Presentation Layer (TLS/HTTPS)

**Situation:**
After validating all layers up through application-level name resolution, I needed to isolate presentation-layer TLS failures and demonstrate how encryption/certificate issues can break secure access even when services are running.

**Task:**
My objective was to remove the HTTPS binding from IIS while leaving the service operational, then prove the failure occurred at Layer 6 by showing that the HTTP service remained healthy while TLS handshakes became impossible.

**Action:**
I removed the HTTPS binding from IIS using the IIS Manager interface, leaving only HTTP on port 80 active. I collected evidence showing:
- IIS service (W3SVC) remained running
- HTTP access on port 80 succeeded
- Port 443 stopped listening (netstat showed no listener)
- TLS handshake attempts failed with 'Connection refused'
- Browser HTTPS access showed ERR_CONNECTION_REFUSED

After restoring the HTTPS binding with the correct certificate, I validated successful TLS handshakes and browser access.

**Result:**
This demonstrated a critical distinction: service availability does not guarantee secure transport availability. Layer 7 (IIS) was healthy while Layer 6 (TLS) was broken. This mirrors real-world production incidents like expired certificates, AWS ALB listener misconfigurations, Azure App Service TLS binding removal, or Kubernetes Ingress missing TLS secrets. The methodology I used—proving service health while isolating TLS failure—is essential for SRE incident response in modern environments where certificate management and TLS enforcement are critical security controls.
