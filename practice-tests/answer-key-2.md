# Answer Key — Practice Test 2

**Q1. Correct: D**

nvidia-smi topo -m prints the GPU-to-GPU interconnect matrix, showing NVx entries for NVLink-connected pairs, which lets engineers confirm full NVSwitch fabric connectivity across all 8 GPUs.

---

**Q2. Correct: A**

dcgmi diag -r 3 runs DCGM's 'long' diagnostic level, which includes memory, PCIe, and NVLink bandwidth stress tests appropriate for thorough bring-up validation before production use.

---

**Q3. Correct: D**

ACS enforces isolation between PCIe devices, forcing peer-to-peer GPU traffic and GPUDirect RDMA traffic through the root complex/CPU instead of a direct path. NVIDIA reference bring-up procedures require ACS to be disabled on the root ports serving GPUs and RDMA NICs to restore expected throughput.

---

**Q4. Correct: B**

Firmware inconsistencies such as mismatched GPU VBIOS or NVSwitch firmware between otherwise identically imaged nodes are a well-known bring-up defect that causes NVLink/NVSwitch topology mismatches and intermittent NCCL failures. Firmware versions should be audited and aligned across the cluster during bring-up before deeper network debugging.

---

**Q5. Correct: C**

NVLink is the high-bandwidth GPU-to-GPU interconnect link/protocol, while NVSwitch is the on-board switching ASIC (four per DGX H100 baseboard) that interconnects multiple NVLinks so every GPU can communicate with every other GPU at full bandwidth in an all-to-all fabric.

---

**Q6. Correct: C**

Because the host never completes POST, host OS-based tools cannot run and reflashing firmware is premature. The correct first step is out-of-band troubleshooting through the BMC: review the System Event Log and sensor data for power-supply, voltage rail, or CPU/GPU fault flags to identify the hardware condition preventing POST.

---

**Q7. Correct: A**

On a DGX H100, all GPUs should show NV18 (full 18-link NVLink4 connectivity via NVSwitch). A lower link count like NV12 for a specific pair indicates a hardware-level NVLink degradation (bad connector, tray seating, or NVSwitch baseboard fault), which must be diagnosed via Fabric Manager logs and physical inspection before the node is certified for production.

---

**Q8. Correct: A**

DGX H100 systems use NVSwitch-based fabrics that require the nv-fabricmanager service to initialize and configure NVLink routing between GPUs. If GPUs enumerate correctly in nvidia-smi but peer-to-peer communication fails, the fabric manager service not running (or failing to start) is the most common cause during bring-up.

---

**Q9. Correct: A**

nvfwupd is NVIDIA's supported firmware update and validation utility for checking BIOS, BMC, GPU VBIOS, and NVSwitch firmware against the officially supported firmware bundle for a given driver/CUDA release, and is meant to run before OS/driver installation during bring-up.

---

**Q10. Correct: C**

NVIDIA's CUDA compatibility documentation specifies that CUDA 12.4 requires a minimum Linux driver version of 550.54.14 (R550 branch). Installing an older driver branch alongside CUDA 12.4 during bring-up will result in a driver/toolkit mismatch.

---

**Q11. Correct: B**

Rail-optimized design dedicates one leaf switch per adapter rank (rail) across all nodes in the SU. Adapter 0 on every DGX connects to leaf switch 0, adapter 1 to leaf switch 1, and so on. This keeps GPU-to-GPU traffic for a given rail on a single switch, minimizing hop count and maximizing non-blocking bandwidth for collective operations.

---

**Q12. Correct: D**

In DPU mode, the embedded Arm subsystem on the BlueField card owns the network functions and control plane. The host sees a virtual NIC managed by the DPU, and privileged network configuration is enforced independently of the host OS, which is exactly what an isolation/zero-trust security requirement calls for.

---

**Q13. Correct: D**

RoCEv2 requires a lossless Ethernet fabric to perform reliably. Enabling PFC on the dedicated RoCE priority prevents buffer overflow drops at the switch, while ECN/WRED provides end-to-end congestion signaling so senders back off before drops occur. Without both mechanisms configured together, RoCEv2 traffic is prone to exactly this kind of drop-induced timeout under load.

---

**Q14. Correct: C**

GPUDirect Storage requires an RDMA-capable data path between storage targets and GPU memory. NVMe over Fabrics (NVMe-oF) running over InfiniBand or RoCE provides the low-latency, high-throughput, CPU-bypass connectivity that GDS depends on, making it the standard choice for checkpoint I/O in NVIDIA-validated AI infrastructure designs.

---

**Q15. Correct: C**

`nvidia-smi -mig 1` enables MIG mode on the GPU (typically requiring a GPU reset or reboot to take effect). Only after MIG mode is enabled can GPU instances such as 1g.5gb profiles be created with the `-cgi` flag.

---

**Q16. Correct: B**

At NDR (400Gb/s) speeds, passive copper DAC cables are only reliable for very short reaches (typically around 1.5-2 meters). For runs beyond that distance, such as between adjacent racks, active optical cables or optical transceivers with MPO fiber are required to maintain signal integrity.

---

**Q17. Correct: C**

In a rail-optimized design, each GPU's NIC (GPU0-GPU7) connects to its own dedicated leaf switch, so GPU0 on every node lands on rail/leaf-switch 1, GPU1 on leaf-switch 2, and so on. This keeps same-rank GPU traffic within a single switch hop, minimizing latency for NCCL all-reduce/all-gather operations across nodes.

---

**Q18. Correct: D**

DPU mode makes the BlueField's embedded Arm subsystem (via the ECPF - embedded CPU function) the sole owner of networking, storage, and security configuration. The host server only sees a virtual representor interface and cannot alter the underlying datapath, delivering the zero-trust isolation the scenario requires.

---

**Q19. Correct: C**

RoCEv2 requires a lossless Ethernet fabric. This is achieved by enabling Priority Flow Control (PFC) to pause the specific traffic class carrying RoCE frames before buffers overflow, combined with ECN marking and DCQCN (Data Center Quantized Congestion Notification) end-to-end so senders react to congestion before PFC pause frames are triggered. Both must be configured together for stable, low-drop RoCEv2 performance.

---

**Q20. Correct: A**

GPUDirect Storage establishes a direct DMA data path between NVMe/NVMe-oF storage and GPU memory, bypassing the CPU and system memory bounce buffer. This requires the nvidia-fs kernel module, a compatible RDMA-capable NIC or BlueField DPU path, and storage/filesystem support (e.g., NVMe-oF targets, compatible parallel filesystems). This is the exact stack validated during DGX/AI cluster storage installation for GDS.

---

**Q21. Correct: D**

32 downlink ports divided by 8 uplink ports yields a 4:1 oversubscription ratio, meaning the fabric is oversubscribed and NCCL-style collective operations (all-reduce, all-gather) that saturate node-to-node bandwidth may experience congestion at the spine layer.

---

**Q22. Correct: C**

Rail-optimized topology places same-rail GPUs from different nodes onto the same set of leaf switches, so intra-rail collective communication (e.g., ring/tree NCCL patterns matched to GPU index) stays local to the leaf, reducing hop count and spine congestion.

---

**Q23. Correct: D**

At NDR (400Gb/s), passive DAC is limited to roughly 2 meters and active copper only modestly extends that reach. A 50-meter run requires active optical cables (AOC), which integrate transceivers with the fiber assembly and reliably support distances up to about 100 meters.

---

**Q24. Correct: A**

mlxlink is the NVIDIA/Mellanox utility designed to query detailed per-port physical-layer diagnostics, including optical power levels, transceiver temperature, FEC/symbol error counters, and eye-margin data for a specific cable and port.

---

**Q25. Correct: A**

Available continuous capacity = √3 × 208V × 60A × 0.8 ≈ 17.3kW. A single DGX H100 at 10.2kW fits within that budget, but a second unit would require 20.4kW total, exceeding the derated 17.3kW limit — so only 1 system can be safely powered from this circuit.

---

**Q26. Correct: A**

ibdiagnet's cable-info reporting captures per-link symbol error trends and transceiver telemetry such as temperature and optical power, enabling proactive detection of degrading fiber or optics before a hard link failure occurs.

---

**Q27. Correct: B**

At NDR 400 Gb/s signaling rates, passive copper DAC is electrically limited to ~2-3 m reach, so it works for in-rack GPU-to-leaf runs but not for 30 m spine links. Active optical cables (AOC) with OSFP transceivers are designed for these longer reaches while preserving full 400G bandwidth, making a mixed-media design (copper short, AOC long) the correct fabric approach.

---

**Q28. Correct: D**

A sustained upward trend in raw (pre-FEC) BER signals real physical-layer degradation (marginal cable, contaminated or misaligned optics, connector wear) even though FEC currently corrects all errors to 0 effective BER. RS-FEC has a fixed correction ceiling, so proactively replacing the suspect optic/cable before raw BER approaches that threshold prevents future uncorrectable errors and job-impacting packet loss.

---

**Q29. Correct: D**

Separating node pools lets inference use MIG's hardware-isolated fractional GPUs for efficient sharing, while training jobs retain full, un-partitioned GPUs with full memory/NVLink access needed for multi-node scaling — labels/taints direct each workload type to the correct pool.

---

**Q30. Correct: A**

Slurm, provisioned and managed through BCM, natively supports gang scheduling, backfill, QOS-based fairshare, and granular per-department job accounting — exactly the HPC/MPI training requirements described.

---

**Q31. Correct: C**

GPU Operator time-slicing is configured via a ConfigMap specifying resources and a 'replicas' value, referenced by the ClusterPolicy's devicePlugin.config, and applied to nodes via a label so the device plugin advertises multiple virtual GPU slots.

---

**Q32. Correct: B**

BCM node categories allow per-hardware-generation software images, driver versions, and kernel modules to be provisioned consistently, and categories can be mapped directly to distinct Slurm partitions — preserving a single management pane.

---

**Q33. Correct: B**

MIG physically partitions an Ampere/Hopper-generation GPU's SMs and memory into isolated instances with dedicated bandwidth, while time-slicing simply schedules full-GPU compute time slices across processes with no memory partitioning or isolation.

---

**Q34. Correct: B**

Taints on H100 nodes repel any pod lacking the matching toleration, and enforcing that toleration exclusively through an admission control policy (with Tenant A's node affinity/nodeSelector further targeting those nodes) guarantees only Tenant A's pods can ever land there.

---

**Q35. Correct: A**

The "mixed" MIG strategy allows the GPU Operator to expose MIG-partitioned devices (e.g., nvidia.com/mig-1g.10gb) on some nodes for isolated, fractional inference workloads while other nodes remain non-MIG and expose nvidia.com/gpu for exclusive-use training jobs — satisfying both requirements on one cluster.

---

**Q36. Correct: C**

Slurm natively provides gang scheduling, backfill, fair-share, and topology-aware placement needed for large HPC-style training jobs, while Kubernetes is better suited for long-running, auto-scaling, service-discovery-driven inference workloads. BCM is designed to provision and manage both Slurm and Kubernetes on the same physical cluster, making this hybrid model the standard NVIDIA-recommended approach.

---

**Q37. Correct: B**

BCM organizes nodes into categories that carry role and configuration profiles (e.g., Slurm compute role vs. Kubernetes worker role). Nodes can be moved between categories via cmsh without a full re-image, allowing an automated, scheduled reassignment that matches the changing training/inference ratio.

---

**Q38. Correct: D**

Xid 79 specifically means the GPU has dropped off the PCIe/NVLink bus, which is a hardware-level fault (power, thermal, seating, or board defect). It cannot be resolved by driver or software changes and requires physical inspection, reseating, or GPU replacement.

---

**Q39. Correct: D**

A single NVLink degrading to a fraction of its rated speed while others operate normally points to a link-layer hardware fault. The correct isolation step is checking NVLink error/replay counters and system logs for Xid 74 (NVLink error) to confirm before escalating to hardware replacement.

---

**Q40. Correct: B**

ib_write_bw (perftest) measures raw RDMA throughput directly between HCAs, isolating the physical fabric from NCCL's collective algorithm and software overhead. If raw RDMA bandwidth is also degraded, the problem is in the network; if it's healthy, the issue is in the NCCL/software layer.

---

**Q41. Correct: C**

'SW Power Cap' throttling occurs when the GPU hits its configured software power limit, not a thermal limit. Since temperatures are well within range and the rack still has power headroom, raising the power limit via nvidia-smi -pl is the direct fix that removes the artificial cap causing throttling.

---

**Q42. Correct: B**

Xid 74 is the code specifically documented as indicating an NVLink error, distinct from memory subsystem faults, and is the code an administrator should search logs for when link-level connectivity issues are suspected.

---

**Q43. Correct: A**

High per-GPU utilization with plateauing overall throughput and non-saturated individual links is a classic signature of collective communication inefficiency at scale — either a suboptimal NCCL algorithm (e.g., Tree vs Ring selection) or congestion at the spine/core switch layer during synchronized all-reduce bursts, which per-link average counters can mask.

---

**Q44. Correct: A**

BIOS updates commonly reset PCIe ACS to its enabled default, which forces GPU peer-to-peer and GPUDirect RDMA traffic to route through the CPU root complex instead of taking a direct PCIe path, sharply cutting all-reduce bandwidth without producing any GPU health errors. Checking and disabling ACS on the relevant PCIe switches/root ports is the documented remediation for this exact symptom.

---

**Q45. Correct: B**

Xid 79 ('GPU has fallen off the bus') is documented as a hardware-level fault typically caused by power delivery problems, an unstable PCIe connection, thermal issues, or a defective GPU/riser — not by software. Since dcgmi diag passed cleanly beforehand, the correct diagnostic path is inspecting physical power connections and PSU health before considering component replacement.

---

