## Chapter 4: InfiniBand, RoCE, and BlueField DPU Deployment — Practice Questions

**Q1.** A cluster architect is designing a 2,000-GPU training pod requiring the lowest possible latency, deterministic job completion times, and native in-network congestion control. Which fabric BEST meets these requirements?

A) Standard Ethernet running TCP/IP with default switch settings
B) InfiniBand NDR/XDR fabric with adaptive routing and SHARP
C) RoCEv2 with PFC disabled for maximum throughput
D) Fibre Channel over Ethernet (FCoE)

<details>
<summary>Answer</summary>

**Correct: B**

InfiniBand provides the lowest latency, native adaptive routing, and hardware-based congestion control (plus SHARP in-network reduction) needed for large-scale, latency-sensitive GPU training.

**Why the others are wrong:**
- Standard TCP/IP Ethernet has far higher latency and no RDMA, unsuitable for large collective operations.
- RoCEv2 without PFC is not lossless and will drop packets under congestion, causing severe performance degradation.
- FCoE is a storage transport, not designed for GPU-to-GPU RDMA training traffic.

</details>

---

**Q2.** An organization is deploying RoCEv2 for GPU-to-GPU communication on an existing all-Ethernet data center. Which configuration is REQUIRED on the switches to make the fabric lossless?

A) Enable Spanning Tree Protocol on all uplinks
B) Enable Priority Flow Control (PFC) and Explicit Congestion Notification (ECN)
C) Disable all flow control mechanisms to maximize throughput
D) Configure only 802.1Q VLAN tagging

<details>
<summary>Answer</summary>

**Correct: B**

RoCEv2 requires a lossless underlay, achieved by combining PFC (pause per priority class) with ECN (congestion marking), typically implemented via DCQCN.

**Why the others are wrong:**
- STP prevents loops but has no role in lossless transport for RDMA traffic.
- Disabling flow control guarantees packet loss under congestion, which breaks RDMA reliability assumptions.
- VLAN tagging is a Layer 2 segmentation feature unrelated to lossless delivery.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A deployment requires offloading storage virtualization, accelerating NVMe-oF traffic, and enforcing tenant network isolation at line rate without consuming host CPU cycles. Which component should be deployed?

A) A standard dual-port NIC
B) An InfiniBand Subnet Manager appliance
C) A BlueField DPU running DOCA-based services
D) A GPU configured with vGPU profiles

<details>
<summary>Answer</summary>

**Correct: C**

BlueField DPUs, programmed via the DOCA framework, are purpose-built to offload storage, networking, and security functions from the host CPU while operating at line rate.

**Why the others are wrong:**
- A standard NIC has no programmable offload engine for storage virtualization or security isolation.
- The Subnet Manager only manages InfiniBand fabric routing, not storage or tenant isolation.
- vGPU addresses GPU compute partitioning, not network/storage offload.

</details>

---

**Q4.** Which storage solution is BEST suited for high-throughput, multi-node checkpoint writes during large-scale distributed AI training?

A) NFS v3 mounted from a single storage node
B) Local NVMe drives on each compute node only
C) A parallel filesystem such as Lustre or WEKA accessed over RDMA
D) SMB file shares over a 1GbE management network

<details>
<summary>Answer</summary>

**Correct: C**

Parallel filesystems distribute I/O across many storage servers and support RDMA transport, meeting the aggregate throughput demands of concurrent multi-node checkpointing.

**Why the others are wrong:**
- NFS v3 from a single node becomes a bottleneck and lacks parallel I/O scaling.
- Local NVMe doesn't provide a shared namespace, so checkpoints aren't accessible cluster-wide.
- SMB over 1GbE cannot deliver the bandwidth required for large model checkpoints.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Which NVIDIA platform combines Spectrum switches and BlueField DPUs to deliver an Ethernet-based full-stack networking solution for AI clusters?

A) Spectrum-X
B) Quantum InfiniBand platform
C) ConnectX-6 standalone adapters
D) Unified Fabric Manager (UFM) alone

<details>
<summary>Answer</summary>

**Correct: A**

Spectrum-X pairs Spectrum switches (e.g., Spectrum-4) with BlueField DPUs (e.g., BlueField-3) to deliver adaptive routing and congestion control over Ethernet for AI workloads.

**Why the others are wrong:**
- Quantum is NVIDIA's InfiniBand switch line, not Ethernet-based.
- ConnectX-6 is a NIC/HCA product, not a full platform.
- UFM is InfiniBand fabric management software, not an Ethernet AI networking platform.

</details>

---

**Q6.** Which component actively computes routing tables, topology, and partition configuration for an InfiniBand fabric?

A) HCA firmware on each compute node
B) DOCA Flow running on a BlueField DPU
C) The Subnet Manager (SM)
D) Priority Flow Control (PFC) daemon

<details>
<summary>Answer</summary>

**Correct: C**

The Subnet Manager discovers topology, assigns LIDs, computes routing tables, and manages partitions — without an active SM, an InfiniBand fabric cannot forward traffic.

**Why the others are wrong:**
- HCA firmware handles the host-side adapter function, not fabric-wide routing.
- DOCA Flow programs packet processing pipelines on BlueField, unrelated to InfiniBand SM function.
- PFC is an Ethernet lossless mechanism; it has no role in InfiniBand fabric management.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** Which congestion control mechanism combines ECN marking with PFC pause frames to provide lossless, low-latency behavior for RoCEv2 at scale?

A) DCQCN
B) TCP Reno
C) Random Early Detection (RED) alone
D) Spanning Tree Protocol (STP)

<details>
<summary>Answer</summary>

**Correct: A**

DCQCN (Data Center Quantized Congestion Notification) uses ECN for end-to-end rate signaling while relying on PFC as a lossless backstop, making it the standard congestion scheme for RoCEv2 GPU clusters.

**Why the others are wrong:**
- TCP Reno is a legacy TCP congestion algorithm, not designed for RDMA lossless fabrics.
- RED alone provides queue management but does not integrate with PFC pause signaling.
- STP is a Layer 2 loop-prevention protocol with no congestion control function.

</details>

---

**Q8.** Which statement BEST describes the role of DOCA in a BlueField DPU deployment?

A) It is a physical top-of-rack switch product
B) It is a drop-in replacement for the InfiniBand Subnet Manager
C) It is an SDK/framework providing APIs, libraries, and drivers to build and deploy services on BlueField DPUs
D) It is a storage protocol equivalent to NVMe-oF

<details>
<summary>Answer</summary>

**Correct: C**

DOCA (Data Center Infrastructure-on-a-Chip Architecture) is the software development framework used to program and deploy accelerated services on BlueField DPUs.

**Why the others are wrong:**
- DOCA is software, not a switch hardware product.
- DOCA has no role in InfiniBand subnet management; that is handled by the SM/UFM.
- DOCA is not a storage transport protocol; NVMe-oF traffic can be accelerated by DOCA-based services but DOCA itself is the framework, not the protocol.

</details>

---

