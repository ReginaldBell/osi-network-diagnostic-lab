# STAR Interview Narrative — Phase 2: Data Link Layer (ARP Poisoning)

**Situation:**
After validating Layer 1 connectivity, I designed a scenario to isolate Layer 2 failures and demonstrate how address resolution problems can break networking even when IP configuration and routing appear correct.

**Task:**
My goal was to introduce an ARP poisoning attack by creating a static neighbor entry with an incorrect MAC address, then prove the failure occurred at Layer 2 by showing that Layer 3 (routing) remained healthy while all traffic failed.

**Action:**
I injected a poisoned ARP entry using `ip neighbor add 192.168.22.2 lladdr 00:00:00:00:00:99 dev ens33`, targeting the default gateway. I collected evidence showing:
- Valid IP configuration and routing table
- Incorrect MAC address in ARP cache
- Complete packet loss to gateway and external hosts
- Failed application connectivity despite correct Layer 3 setup

I then removed the static entry with `ip neighbor del`, allowing dynamic ARP resolution to restore the correct MAC address. Validation showed immediate recovery of connectivity.

**Result:**
This demonstrated a critical principle: valid IP configuration does not guarantee connectivity when Layer 2 address resolution is incorrect. This scenario mirrors real-world incidents I've studied, including ARP spoofing attacks, switch MAC table corruption, and VLAN misconfigurations. The methodology—verifying Layer 3 health while isolating Layer 2 as the root cause—is essential for SOC analysts investigating potential man-in-the-middle attacks or network engineers troubleshooting intermittent connectivity.
