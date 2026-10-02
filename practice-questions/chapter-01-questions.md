## Chapter 1: DGX H100/H200 Hardware and Initial Power-On — Practice Questions

**Q1.** A cluster architect is racking 4x DGX H100 nodes in a single rack and must confirm the PDU can support worst-case load. Each DGX H100 has 6x 3300W PSUs in an N+2 redundant configuration with a rated system max power draw of approximately 10.2kW per node. Which power budget calculation is correct for the rack?

A) Budget 4 x (6 x 3300W) = 79.2kW because all PSUs can draw full power simultaneously
B) Budget 4 x (4 x 3300W) = 52.8kW since only 4 of 6 PSUs are ever active
C) Budget 4 x 10.2kW = 40.8kW based on rated max system power draw, not raw PSU wattage
D) Budget is irrelevant since N+2 redundancy guarantees the PDU will auto-throttle

<details>
<summary>Answer</summary>

**Correct: C**

Rack power budgeting must use the system's rated max power draw (~10.2kW for DGX H100), not the sum of installed PSU capacities, because PSUs are sized for redundancy (N+2) not for simultaneous full-load output.

**Why the others are wrong:**
- Summing all 6 PSU wattages ignores redundancy design and massively overestimates actual draw
- 4x3300W assumes only 4 PSUs are load-bearing but still uses raw PSU wattage instead of the actual rated system draw, giving an inflated number
- PDUs do not auto-throttle DGX systems; power budgeting is a manual planning responsibility

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** During pre-boot bring-up of a new DGX H100 node, a technician wants to verify GPU baseboard health before the OS has loaded. What is the correct approach?

A) SSH into the OS and run nvidia-smi
B) Wait for the OS to boot, then check dmesg for GPU errors
C) Access the BMC out-of-band via its dedicated management port and review pre-boot health status
D) Run a CUDA sample application to trigger a GPU self-test

<details>
<summary>Answer</summary>

**Correct: C**

The BMC is accessible out-of-band before OS boot via its dedicated management NIC (Redfish/IPMI) and reports POST results, PSU status, and component health independent of the host OS.

**Why the others are wrong:**
- nvidia-smi requires a running OS with drivers loaded, which is not available pre-boot
- dmesg also requires the OS to be running and does not represent a pre-boot check
- CUDA applications require a fully booted OS and driver stack, unavailable at this stage

</details>

---

**Q3.** A cluster operator is integrating a mixed rack: 6 nodes are DGX H100 and 2 nodes are DGX H200, all intended to participate in the same NCCL collective communication job across an NVLink/InfiniBand fabric. What is the primary compatibility concern?

A) H200 nodes cannot use NVSwitch fabric at all and must be isolated
B) DGX H200 requires a different NVLink protocol version incompatible with H100 entirely
C) There is no compatibility concern since both use identical NVSwitch hardware
D) Mixed GPU generations in the same node are supported but mixed models across nodes in the same job can cause performance imbalance and driver/firmware mismatch issues

<details>
<summary>Answer</summary>

**Correct: D**

H100 and H200 share the same NVLink4/NVSwitch architecture, but differing GPU memory capacity, compute throughput, and required driver/firmware versions across nodes in the same collective job can cause synchronization stalls and require careful compatibility matrix validation before job scheduling.

**Why the others are wrong:**
- H200 fully supports NVSwitch fabric; isolation is not required by hardware limitation
- NVLink protocol version is shared between H100 and H200 (same generation), so full incompatibility is false
- While NVSwitch hardware family is shared, performance and firmware differences still create real compatibility concerns, so 'no concern' is incorrect

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which interface should be used to check DGX H100 component health status before the operating system has booted?

A) The BMC via Redfish/IPMI over its dedicated management port
B) The host OS's systemd journal
C) nvidia-smi over SSH
D) The NVSwitch driver log in /var/log

<details>
<summary>Answer</summary>

**Correct: A**

The BMC (Baseboard Management Controller) operates independently of the host OS and provides out-of-band access via Redfish/IPMI, making it the only option available before OS boot.

**Why the others are wrong:**
- systemd journal only exists once the OS kernel has booted
- nvidia-smi requires the NVIDIA driver stack running under a booted OS
- NVSwitch driver logs are written by the OS-level driver, unavailable pre-boot

</details>

---

**Q5.** Which component provides the high-bandwidth intra-node GPU-to-GPU communication fabric inside a DGX H100 GPU baseboard?

A) PCIe Gen5 switch
B) ConnectX-7 InfiniBand adapter
C) Top-of-rack Ethernet switch
D) NVSwitch

<details>
<summary>Answer</summary>

**Correct: D**

NVSwitch (4 units per DGX H100 baseboard) interconnects all 8 GPUs at full NVLink bandwidth, forming the intra-node all-to-all fabric.

**Why the others are wrong:**
- PCIe Gen5 is used for CPU-GPU and I/O connectivity, not the primary GPU-to-GPU fabric
- ConnectX-7 adapters handle inter-node (cluster-level) networking, not intra-node GPU communication
- Top-of-rack Ethernet switches operate at the rack/cluster level, not inside the node

</details>

---

**Q6.** A DGX H100 node ships with 6 power supply units in an N+2 redundant configuration. How many PSUs can fail simultaneously while the system continues operating at full rated power?

A) 0 — any PSU failure causes shutdown
B) 2
C) 1
D) 3

<details>
<summary>Answer</summary>

**Correct: B**

N+2 redundancy means the system requires only N (4) PSUs for full-rated operation, with 2 additional PSUs as redundant capacity, so up to 2 can fail without loss of full power delivery.

**Why the others are wrong:**
- This ignores the entire purpose of N+2 redundancy design
- Underestimates the redundancy level; N+2 tolerates two failures, not one
- Overestimates the redundancy; the system needs 4 active PSUs (N), so losing 3 would drop below the required N

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** When configuring initial BMC network access on a newly racked DGX H100 for out-of-band management, which statement is correct?

A) The BMC shares the host OS's primary network interface and IP address
B) The BMC uses a dedicated physical management port and can be configured with its own static IP independent of the host OS network
C) The BMC can only be accessed after the OS has booted and enabled remote management services
D) The BMC requires the same ConnectX-7 InfiniBand fabric used for NCCL traffic

<details>
<summary>Answer</summary>

**Correct: B**

The BMC has its own dedicated NIC and IP configuration, fully independent of the host OS, allowing out-of-band access even when the OS is not installed or not booted.

**Why the others are wrong:**
- The BMC network is physically and logically separate from the host OS network interfaces
- This is backwards — BMC access is specifically designed to work before/without OS boot
- The BMC uses its own dedicated Ethernet management port, not the compute/InfiniBand fabric

</details>

---

**Q8.** What is the primary hardware difference between DGX H100 and DGX H200 that affects cluster compatibility planning?

A) DGX H200 uses a completely different NVSwitch generation incompatible with H100
B) DGX H200 removes NVSwitch entirely in favor of direct PCIe GPU peering
C) DGX H200 uses AMD EPYC CPUs instead of Intel Xeon
D) DGX H200 GPUs provide significantly higher HBM3e memory capacity per GPU (141GB) compared to H100's 80GB HBM3, while sharing the same NVLink/NVSwitch fabric generation

<details>
<summary>Answer</summary>

**Correct: D**

The core architectural difference is GPU memory capacity (H200's 141GB HBM3e vs H100's 80GB HBM3), while both share the same NVLink4/NVSwitch fabric generation, which is why mixed clusters are technically feasible but require compatibility validation.

**Why the others are wrong:**
- H100 and H200 share the same NVSwitch/NVLink generation; there is no incompatible switch generation change
- Both systems retain NVSwitch as the intra-node fabric; PCIe peering is not the primary interconnect
- Both DGX H100 and H200 use dual Intel Xeon Platinum CPUs, not AMD EPYC

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

