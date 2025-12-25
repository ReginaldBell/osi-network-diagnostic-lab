# STAR Interview Narrative — Phase 1: Physical Layer (Link State)

**Situation:**
In a controlled lab environment, I needed to demonstrate how physical layer failures cascade through the entire network stack and understand what evidence proves a Layer 1 issue versus higher-layer problems.

**Task:**
My objective was to intentionally disable network connectivity at the physical layer, observe the complete failure of all dependent services, and prove through systematic evidence collection that no higher-layer troubleshooting would be relevant until Layer 1 was restored.

**Action:**
I disabled the network interface using `ip link set ens33 down`, then collected evidence showing:
- Absence of IP address assignment
- Empty ARP neighbor table
- Failed connectivity tests to local and remote targets
- Complete loss of all network services

I documented each observation with timestamps and command output. After re-enabling the interface with `ip link set ens33 up`, I validated full recovery by confirming DHCP assignment, ARP population, and end-to-end connectivity.

**Result:**
I successfully proved that Layer 1 failures prevent all higher-layer functionality, establishing a baseline understanding that physical connectivity is the foundation for all network operations. This methodology directly applies to production scenarios like interface flapping, cable faults, or misconfigured port channels. The systematic evidence collection I used—checking link state before investigating routing or DNS—is the same approach used in enterprise NOC escalations.
