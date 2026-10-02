## Chapter 5: Cabling Standards, Transceivers, and Fabric Topology — Practice Questions

**Q1.** A company is connecting NDR 400Gb/s InfiniBand ports between GPU servers and a leaf switch within the same rack, at a cable length of 1.5 meters. Which cable type minimizes cost and power consumption while meeting typical passive copper distance limits?

A) AOC (Active Optical Cable)
B) Multimode optical transceiver + fiber patch
C) Active Copper Cable (ACC)
D) Passive copper DAC

<details>
<summary>Answer</summary>

**Correct: D**

Passive DAC supports NDR 400Gb/s up to roughly 2m, has no active components, and is the lowest cost/lowest power option — ideal for a 1.5m in-rack run.

**Why the others are wrong:**
- AOC is unnecessary and more expensive for a distance well within passive DAC range.
- Optical transceivers add cost, power draw, and complexity that aren't justified at this distance.
- ACC is used to extend copper reach beyond passive limits — not needed when passive DAC already covers the distance.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A cluster requires connecting spine switches to leaf switches in adjacent rows, with a cable run of 40 meters at NDR 400Gb/s. Passive copper is not viable at this distance. Which option is BEST, balancing cost and reliability without requiring separate optics and fiber patching?

A) Passive DAC
B) AOC (Active Optical Cable)
C) Discrete optical transceiver + OM4 fiber patch
D) Single-mode optical transceiver + SMF patch cable

<details>
<summary>Answer</summary>

**Correct: B**

AOC integrates the optics into a fixed cable assembly, supporting reaches up to roughly 50m at NDR without needing separate transceivers and patch cables — simpler and typically cheaper for this range.

**Why the others are wrong:**
- Passive DAC is limited to about 2-3m, far short of 40m.
- A discrete transceiver + patch cable works but adds procurement/management complexity and cost versus an integrated AOC at this distance.
- Single-mode optics are designed for much longer reaches and add unnecessary cost for a 40m intra-datacenter run.

</details>

---

**Q3.** A DGX SuperPOD-style deployment requires minimizing the number of switch hops for all-reduce operations across multi-GPU nodes. Which topology design should the network architect implement for the backend GPU fabric?

A) Fat-tree with 3:1 oversubscription
B) Ring topology with direct GPU-to-GPU cabling
C) Rail-optimized topology connecting same-rank GPU NICs to a common leaf switch
D) Full mesh topology between all leaf switches

<details>
<summary>Answer</summary>

**Correct: C**

Rail-optimized topology maps each GPU's NIC (by rank) to a dedicated leaf/rail switch, minimizing hops and contention for east-west collective communication patterns like NCCL all-reduce.

**Why the others are wrong:**
- Oversubscription describes bandwidth ratio, not hop-count design, and 3:1 would add contention, not reduce hops.
- Ring topologies are not used in scalable switched InfiniBand fabrics.
- Full mesh between leaf switches is not a standard scalable fat-tree/rail design and doesn't target GPU rank alignment.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A data center interconnect requires linking two InfiniBand switches located in buildings 2 kilometers apart. Which transceiver type is required?

A) Single-mode DR/FR transceiver
B) Multimode SR transceiver
C) AOC
D) Passive DAC

<details>
<summary>Answer</summary>

**Correct: A**

Single-mode DR/FR-class optics support reaches from roughly 500m up to 2km, making them the correct choice for a 2km building-to-building link.

**Why the others are wrong:**
- Multimode SR optics typically max out around 30-100m depending on fiber grade — far too short.
- AOC assemblies are generally rated only up to roughly 50-100m.
- Passive DAC is limited to a few meters at most.

</details>

---

**Q5.** Within a single rack, a GPU server's ConnectX NDR port must connect to the top-of-rack InfiniBand leaf switch at a distance of 1 meter. Which cabling option is MOST cost-effective and lowest latency?

A) Passive copper DAC
B) AOC
C) Single-mode transceiver + fiber
D) Active Copper Cable (ACC)

<details>
<summary>Answer</summary>

**Correct: A**

Passive copper DAC is the cheapest, lowest-latency, lowest-power option and comfortably covers a 1m in-rack connection.

**Why the others are wrong:**
- AOC introduces unneeded cost and slightly higher latency from optical conversion.
- Single-mode optics are the most expensive option and designed for much longer distances.
- ACC is used when passive DAC can't reach the required distance — unnecessary at 1m.

</details>

---

**Q6.** An AI infrastructure design separates the GPU compute (backend) fabric from the storage/frontend network. To reduce cost, which network should typically implement a higher oversubscription ratio such as 3:1?

A) The GPU compute (backend) fabric
B) Both networks equally
C) The storage/frontend network
D) Neither — all AI fabrics must be non-blocking

<details>
<summary>Answer</summary>

**Correct: C**

Storage and frontend traffic patterns are less sensitive to oversubscription, so applying a ratio like 3:1 there reduces uplink/spine port cost without harming training performance.

**Why the others are wrong:**
- The GPU compute fabric carries latency-sensitive collective traffic and should remain near non-blocking (1:1) to avoid degrading NCCL performance.
- Applying the same ratio to both ignores their different traffic sensitivity and cost tradeoffs.
- Non-blocking design is a best practice for compute fabrics specifically, not a universal requirement for every network in the cluster.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** A leaf switch has 32 downlink ports connected to GPU servers and 8 uplink ports connected to spine switches, all at the same port speed. What is the oversubscription ratio of this leaf switch?

A) 1:1
B) 4:1
C) 2:1
D) 8:1

<details>
<summary>Answer</summary>

**Correct: B**

Oversubscription ratio is downlink bandwidth to uplink bandwidth: 32 downlinks to 8 uplinks at equal port speed simplifies to 4:1.

**Why the others are wrong:**
- 1:1 would require equal downlink and uplink port counts, which is not the case here.
- 2:1 would require 16 uplinks for 32 downlinks, not 8.
- 8:1 would require only 4 uplinks for 32 downlinks, not 8.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement BEST differentiates a rail-optimized topology from a traditional fat-tree topology in a GPU AI cluster?

A) Rail-optimized topology eliminates the spine layer entirely
B) Rail-optimized topology aligns each GPU's NIC to a dedicated leaf switch rail matching GPU rank, reducing hops for east-west collective traffic, while fat-tree provides uniform any-to-any connectivity through a Clos hierarchy
C) Fat-tree topology only supports Ethernet fabrics, while rail-optimized only supports InfiniBand
D) Rail-optimized topology requires 3:1 oversubscription while fat-tree always requires 1:1

<details>
<summary>Answer</summary>

**Correct: B**

Rail-optimized designs group GPUs by NIC rank onto shared leaf switches to shorten paths for collective operations, while fat-tree emphasizes uniform, non-blocking any-to-any connectivity — the two concepts address different goals and can coexist within the same Clos hierarchy.

**Why the others are wrong:**
- Rail-optimized fabrics still typically retain a spine layer; the spine isn't eliminated.
- Both topology styles are used with either Ethernet or InfiniBand fabrics — protocol isn't the differentiator.
- Oversubscription ratio is an independent design parameter, not what defines rail-optimized versus fat-tree.

</details>

---

