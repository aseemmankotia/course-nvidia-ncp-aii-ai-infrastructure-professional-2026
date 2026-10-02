## Chapter 6: Power, Cooling, Link Validation, and Signal Monitoring — Practice Questions

**Q1.** A rack of 4x DGX H100 systems (10.2kW max draw each) must survive the loss of either PDU feed without any system going offline. Which power design meets this requirement?

A) Single 3-phase PDU sized for the full rack load, with a spare PDU in the storage room
B) Dual-corded systems fed by two independent PDUs (A and B), each individually sized to carry the entire rack load
C) Dual-corded systems fed by two PDUs, each sized for half the rack load, sharing the total between them
D) Single-corded systems alternating between PDU A and PDU B for load balancing

<details>
<summary>Answer</summary>

**Correct: B**

True A/B feed redundancy requires each independent PDU path to be capable of carrying 100% of the rack's critical load, so a single feed failure causes zero downtime.

**Why the others are wrong:**
- A single PDU with a spare in storage is not redundant — there is no automatic failover and the spare must be physically swapped in.
- PDUs sized only for half the load will overload and trip if one feed fails, since the remaining PDU can't absorb the full rack draw.
- Alternating single-corded connections still creates single points of failure per server; losing a feed drops half the servers.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A customer plans to deploy 8x DGX H200 systems in a single rack, exceeding 60kW of heat load in a facility with limited hot/cold aisle containment. Which cooling approach BEST addresses this density?

A) Standard perforated floor tiles with increased CRAC unit airflow
B) Direct-to-chip or rear-door heat exchanger liquid cooling
C) Adding more server fans and raising the cold aisle temperature setpoint
D) Spacing DGX systems further apart within the same rack

<details>
<summary>Answer</summary>

**Correct: B**

At densities above roughly 30-40kW per rack, air cooling alone cannot efficiently remove heat; liquid cooling (direct-to-chip or rear-door heat exchangers) is the standard approach for high-density DGX deployments.

**Why the others are wrong:**
- Perforated tiles and increased CRAC airflow do not scale past typical air-cooling density limits and risk hot spots.
- Raising fan speed and cold aisle temperature increases noise and energy draw but doesn't solve the fundamental heat removal limit of air.
- Spacing systems apart isn't feasible within a fixed rack footprint and doesn't change total heat output.

</details>

---

**Q3.** An InfiniBand port shows a rapidly increasing CRC error counter while symbol error counters remain at zero. What is the MOST likely root cause?

A) A firmware mismatch between the HCA and switch
B) A faulty or improperly seated cable/transceiver causing data corruption in transit
C) Incorrect subnet manager configuration
D) MTU mismatch between endpoints

<details>
<summary>Answer</summary>

**Correct: B**

CRC errors without symbol errors typically indicate corruption occurring after signal recovery — consistent with a marginal cable, dirty connector, or failing transceiver introducing bit errors that pass physical detection but fail frame checksum.

**Why the others are wrong:**
- Firmware mismatches usually cause link training failures or negotiation issues, not isolated CRC growth.
- Subnet manager misconfiguration affects routing/topology, not per-frame CRC integrity.
- MTU mismatches cause fragmentation or drops at a higher layer, not physical-layer CRC errors.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which single answer describes the correct BER-based pass/fail criterion commonly used during physical-layer link validation of a high-speed interconnect cable?

A) A raw bit error rate of 1e-3 or worse over a 10-second test window
B) Any single bit error observed during a 60-second test automatically fails the cable
C) BER thresholds only apply to fiber optic links, not copper DAC cables
D) A measured bit error rate worse than 1e-12 fails validation

<details>
<summary>Answer</summary>

**Correct: D**

1e-12 is the widely used industry pass/fail BER threshold for high-speed link/cable certification testing — links measuring worse than this are considered marginal or failing.

**Why the others are wrong:**
- 1e-3 is far too high (poor) to represent a validated, healthy link.
- A single bit error within a short window doesn't necessarily violate the statistical BER threshold used for pass/fail.
- BER testing applies to both copper (DAC/ACC) and optical links during cable qualification.

</details>

---

**Q5.** What is the key functional difference between a physical-layer 'symbol error' counter and a 'CRC error' counter on an InfiniBand or Ethernet port?

A) Symbol errors are only reported on optical links; CRC errors are only reported on copper links
B) They measure the exact same condition and are interchangeable terms
C) Symbol errors indicate corruption at the encoding/signal level detected during link training or transmission; CRC errors indicate the received frame's checksum failed after the frame was otherwise received
D) Symbol errors are a software-layer statistic, while CRC errors are a hardware-layer statistic

<details>
<summary>Answer</summary>

**Correct: C**

Symbol errors reflect issues in the physical encoding/signaling (e.g., 8b/10b or 64b/66b violations), while CRC errors indicate the frame was received but failed its checksum — both point to physical-layer degradation but at different stages of processing.

**Why the others are wrong:**
- Both counter types apply to copper and optical links; the media type doesn't determine which counter is used.
- They measure distinct conditions at different points in the receive pipeline, not the same thing.
- Both are physical/link-layer hardware counters, not a software vs hardware distinction.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** During monitoring, a DGX H100 GPU reports a temperature approaching its documented thermal slowdown threshold. What should the engineer expect the system to do BEFORE a full shutdown occurs?

A) Immediately power off the affected GPU with no warning
B) Redirect all workloads to CPU-only processing
C) Automatically reduce GPU clock speeds (throttle) to lower heat output while remaining operational
D) Trigger a fan-only response with no change to compute performance

<details>
<summary>Answer</summary>

**Correct: C**

Data center GPUs implement a thermal slowdown threshold below the critical shutdown threshold — clocks are reduced to lower power/heat while the GPU keeps running, protecting hardware while preserving availability.

**Why the others are wrong:**
- Immediate power-off only occurs at the higher shutdown threshold, not the slowdown threshold.
- GPUs don't redirect workloads to CPU automatically; that's an application-level decision, not a thermal protection mechanism.
- Fan speed may increase, but the defining protective behavior tested on the exam is clock throttling, not just fan response.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which tool/command is used to read a transceiver's Digital Diagnostic Monitoring (DDM) data — including Tx/Rx optical power, temperature, and voltage — for troubleshooting a suspected failing optic?

A) mlxlink or ethtool -m against the port
B) nvidia-smi -q -d POWER
C) ibqueryerrors -k
D) dcgmi diag -r 3

<details>
<summary>Answer</summary>

**Correct: A**

mlxlink (for NVIDIA/Mellanox NICs and switches) and ethtool -m expose transceiver DDM data such as Tx/Rx power in dBm, module temperature, and voltage, which is the standard method to diagnose failing optics/cables.

**Why the others are wrong:**
- nvidia-smi -q -d POWER reports GPU power draw, not transceiver diagnostics.
- ibqueryerrors -k reports port error counters (like CRC/symbol errors), not optical DDM data.
- dcgmi diag runs GPU health diagnostics and has no visibility into network transceiver optics.

</details>

---

**Q8.** A cluster requires N+1 PDU redundancy for a row of 6 racks, where each rack draws 25kW under normal load. Which configuration correctly implements N+1 at the row level?

A) 6 PDUs, each sized to exactly 25kW, with no spare capacity
B) 5 PDUs sized to cover the full 150kW row load, with a 6th PDU as a cold spare in storage
C) 1 large PDU rated for 150kW with a manual transfer switch to a backup generator
D) 7 PDUs total, where any 6 of the 7 can carry the full row load if one fails

<details>
<summary>Answer</summary>

**Correct: D**

N+1 redundancy means one additional unit (or feed path) beyond the number required (N) to carry full load is present and active, so if any single unit fails, the remaining units still support the entire load without interruption.

**Why the others are wrong:**
- Exactly 25kW per PDU with 6 PDUs leaves zero spare capacity — a single PDU failure would drop a rack's load (this is N, not N+1).
- A cold spare in storage isn't active/automatic redundancy — it requires manual intervention and causes downtime during swap.
- A single large PDU with a manual transfer switch is a single point of failure during the switch event and isn't true N+1 redundancy.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

