# Answer Key — Practice Test 1

**Q1. Correct: A**

nvidia-smi nvlink -s reports the status (active/inactive) and speed of every NVLink connection on each GPU, which is the standard bring-up check for NVLink health.

---

**Q2. Correct: B**

nvfwupd is NVIDIA's supported firmware update utility for DGX and NVIDIA-Certified Systems, used to update BIOS, BMC, GPU VBIOS, and other system firmware components as part of bring-up.

---

**Q3. Correct: A**

nv-fabricmanager configures the NVSwitch interconnect fabric and must be running and healthy for GPUs to communicate over NVLink on multi-GPU NVSwitch-based systems like the DGX H100.

---

**Q4. Correct: B**

DCGM's diagnostic levels, particularly level 3 (dcgmi diag -r 3), run extended stress tests covering memory, thermal behavior, and NVLink bandwidth, making it the standard tool for certifying node health at bring-up.

---

**Q5. Correct: A**

In nvidia-smi topo -m output, 'NVx' denotes direct NVLink connectivity with x links, while 'PHB' denotes that traffic must traverse a PCIe Host Bridge rather than NVLink — a key distinction when validating expected interconnect topology at bring-up.

---

**Q6. Correct: D**

NVIDIA GPU driver versions must be consistent across all nodes in a GPU cluster for NCCL and CUDA compatibility during distributed jobs; the correct remediation during bring-up is to reimage or update the outlier node to the fleet's validated driver/CUDA baseline.

---

**Q7. Correct: A**

`nvidia-smi topo -m` prints the full GPU-to-GPU and GPU-to-NIC topology matrix, showing NVLink (NV#), PIX, PXB, or SYS connection types and is the standard command used during bring-up to confirm all GPUs are fully NVLink-connected through the NVSwitch fabric.

---

**Q8. Correct: D**

PCIe ACS (Access Control Services) forces peer-to-peer traffic between devices to be redirected through the root complex for isolation, which blocks direct GPU-to-GPU or GPU-to-DPU peer-to-peer transfers. Disabling ACS on the relevant PCIe switches/bridges is required during bring-up to allow GPUDirect RDMA and P2P DMA to function correctly.

---

**Q9. Correct: A**

A 'driver/library version mismatch' error specifically indicates the loaded nvidia.ko kernel module version does not match the installed user-space driver/NVML libraries, typically caused by a partial or interrupted driver upgrade. The fix is to cleanly reinstall the matching NVIDIA driver package and reload the kernel module or reboot so the correct module loads.

---

**Q10. Correct: A**

nvsm (NVIDIA System Management) is the DGX-specific tool designed to run comprehensive, system-wide health checks spanning GPUs, NVLink, storage, network adapters, and platform components, producing a consolidated health/diagnostic report suitable for post-bring-up certification.

---

**Q11. Correct: C**

Rail-optimized design dedicates one leaf switch per GPU index (rail) so that all-reduce/collective traffic for a given GPU rank across nodes stays on a single switch, minimizing hops and maximizing bisection bandwidth.

---

**Q12. Correct: C**

RoCEv2 (RDMA over Converged Ethernet v2) enables RDMA over standard Ethernet infrastructure but requires a lossless fabric, achieved by configuring Priority Flow Control (802.1Qbb) and ECN/DCQCN congestion control end-to-end on switches and NICs.

---

**Q13. Correct: C**

Zero Trust mode isolates the DPU's Arm subsystem and control-plane functions from the host, preventing host administrators from managing or tampering with the DPU's security, network, and storage services; management is restricted to a separate trusted entity.

---

**Q14. Correct: D**

NVMe-oF with RDMA transport (e.g., NVMe/IB) lets storage traffic use the existing RDMA-capable InfiniBand fabric directly, bypassing the CPU for data movement and delivering the low-latency, high-throughput access needed for AI training I/O.

---

**Q15. Correct: A**

PCIe ACS (Access Control Services), when enabled, forces peer-to-peer transactions between devices on a PCIe switch to be redirected through the root complex, breaking the direct GPU-to-NIC DMA paths required for GPUDirect RDMA. Disabling ACS on the relevant PCIe bridges restores direct peer-to-peer transfers.

---

**Q16. Correct: A**

Passive copper DAC cables for NDR 400Gb/s InfiniBand are only reliable up to roughly 2 meters due to signal loss at these speeds; beyond that distance, active optical cables (AOC) or optical transceivers with fiber are required to maintain signal integrity.

---

**Q17. Correct: D**

InfiniBand NDR with Quantum-2 switches supports NVIDIA SHARP (Scalable Hierarchical Aggregation and Reduction Protocol), which performs in-network reduction for NCCL collective operations, cutting latency and freeing GPU/CPU cycles — the standard choice for DGX SuperPOD-class compute fabrics.

---

**Q18. Correct: A**

BlueField-3 DPU combines Arm cores, hardware accelerators, and network connectivity, and it is the target platform for DOCA-based services such as virtual switching (OVS offload), firewalling, and storage virtualization, offloading these functions from the host x86 CPU.

---

**Q19. Correct: B**

RoCEv2 requires a lossless Ethernet fabric; this is achieved by enabling PFC on the specific priority/traffic class carrying RoCE traffic combined with ECN marking on switches and DCQCN (Data Center Quantized Congestion Notification) on the NICs to proactively throttle senders before buffers overflow and packets are dropped.

---

**Q20. Correct: D**

GPUDirect RDMA allows network adapters (like ConnectX) to read/write GPU memory directly for inter-node GPU communication, avoiding CPU staging; GPUDirect Storage extends this concept to storage I/O, creating a direct DMA path between NVMe/storage and GPU memory, avoiding CPU bounce buffers — these are distinct data paths for different traffic types.

---

**Q21. Correct: B**

At NDR (400Gb/s) signal rates, passive copper DAC is only reliable for very short runs (roughly 1.5-2m) due to signal attenuation. Runs of 50m require active optical cables or pluggable transceivers over fiber to maintain signal integrity.

---

**Q22. Correct: A**

Elevated raw BER combined with a link auto-downtraining to a lower speed is a classic physical layer symptom of a dirty/damaged optical connector, degraded fiber, or a cable exceeding its rated length/signal budget — the link firmware falls back to a lower, more reliable speed.

---

**Q23. Correct: D**

This design, where each GPU index is aligned to a dedicated leaf switch (a 'rail') across all nodes, is called rail-optimized topology. It minimizes hop count for GPU-to-GPU collective communication (e.g., NCCL all-reduce) by keeping same-rank GPU traffic on a single switch.

---

**Q24. Correct: D**

Base load = (8 x 10.2kW) + (2 x 0.5kW) = 81.6kW + 1kW = 82.6kW. Applying 20% headroom: 82.6 x 1.2 = 99.12kW, rounding to approximately 99kW minimum provisioned capacity.

---

**Q25. Correct: C**

mlxlink is the Mellanox/NVIDIA diagnostic tool that provides detailed PHY-level information including effective and raw BER, per-lane signal quality, module (transceiver) status, and link training details — essential for physical layer troubleshooting.

---

**Q26. Correct: B**

A truly non-blocking fat-tree requires a 1:1 ratio of uplink ports (to the spine) to downlink ports (to compute nodes) on each leaf switch, ensuring full bisection bandwidth with no oversubscription bottleneck.

---

**Q27. Correct: C**

At NDR (400Gb/s) signaling rates, passive copper DAC is only reliable up to roughly 1.5-2m due to signal attenuation. Active Copper Cables (ACC) use embedded signal-boosting electronics to extend reliable copper reach to about 3-5m at a fraction of the cost and power draw of optical solutions, making ACC the correct choice for this exact distance.

---

**Q28. Correct: D**

In rail-optimized design, each of the 8 GPUs on a DGX H100 has its own NIC, and each NIC number (rail 0 through rail 7) is wired to a dedicated leaf switch shared only with the corresponding NIC index on every other node. This requires exactly 8 leaf switches to give each GPU's traffic an isolated, non-blocking path.

---

**Q29. Correct: B**

MIG creates hardware-partitioned instances with separate memory, cache, and compute slices, and GPU Operator exposes each MIG instance as a distinct nvidia.com/gpu resource, giving tenants real isolation.

---

**Q30. Correct: C**

DCGM Exporter, deployed as part of the GPU Operator, wraps NVIDIA Data Center GPU Manager to expose Prometheus-format metrics including utilization, memory, temperature, power, and ECC/Xid errors per GPU.

---

**Q31. Correct: B**

GPU resources are non-compressible and non-shareable by default; Kubernetes requires the identical value in both requests and limits for nvidia.com/gpu so the device plugin allocates exactly one whole GPU exclusively to the pod.

---

**Q32. Correct: B**

Slurm excels at gang scheduling, backfill, and fair-share allocation needed for large synchronized training jobs, and BCM natively integrates Slurm for HPC-style batch scheduling, while Kubernetes' rolling updates, service discovery, and autoscaling are purpose-built for continuously running inference services.

---

**Q33. Correct: C**

BCM organizes nodes into categories, each associated with a software image; new or reinstalled nodes PXE-boot and automatically receive the category's image, kernel modules, and configuration without manual intervention.

---

**Q34. Correct: C**

MIG, supported on Ampere-generation and later NVIDIA data center GPUs, physically partitions the GPU into isolated instances each with their own memory, cache, and streaming multiprocessors; time-slicing instead rapidly context-switches the full GPU among pods with no memory partitioning or fault isolation.

---

**Q35. Correct: B**

Slurm provides gang scheduling and topology-aware placement optimized for tightly-coupled batch HPC/training jobs, while Kubernetes with the GPU Operator is optimized for elastic, service-oriented autoscaling needed by inference workloads. BCM sits underneath both, provisioning and managing the physical/virtual nodes regardless of which workload manager runs on top.

---

**Q36. Correct: C**

The MIG Manager component of the GPU Operator applies the requested MIG configuration profiles to GPUs and reconfigures them as needed, then coordinates with device plugin components to expose the resulting MIG instances as schedulable Kubernetes resources.

---

**Q37. Correct: B**

MIG statically partitions an A100 into hardware-isolated instances with dedicated compute cores and memory, and the GPU Operator's MIG Manager and device plugin expose these instances as individually schedulable Kubernetes resources — ideal for small, isolated inference workloads shared across tenants.

---

**Q38. Correct: D**

nvidia-smi nvlink -s (and nvlink -e for error counts) directly reports the state and error counters of each NVLink connection per GPU, which is the fastest way to isolate a degraded intra-node link when compute utilization is normal but throughput is down.

---

**Q39. Correct: A**

DCGM's dcgmi diag command offers multiple diagnostic run levels (r1 through r4/r3) covering deployment checks, integration tests, medium stress tests, and extended stress/NVLink validation, and it integrates natively with Base Command Manager for cluster-wide scheduled health checks.

---

**Q40. Correct: B**

Persistence mode keeps the NVIDIA driver loaded and GPU state initialized even when no client is actively connected, eliminating the driver reload/initialization overhead each time a new job starts — a well-known best practice for job scheduling latency on DGX nodes.

---

**Q41. Correct: D**

'SW Power Cap' as the active throttle reason explicitly indicates the GPU is being clocked down to stay within its configured software power limit, not a thermal or hardware fault; the fix is to verify available power headroom and raise the per-GPU power limit with nvidia-smi -pl (or investigate node-level power budget constraints) if capacity permits.

---

**Q42. Correct: C**

Volatile ECC error counts are reset whenever the driver is reloaded or the GPU is reset, reflecting errors since the last reset event, while Aggregate counts are the cumulative lifetime error total stored on the GPU and persist across driver reloads and reboots, making them useful for tracking long-term hardware degradation trends.

---

**Q43. Correct: C**

Since the symptom is isolated to multiple ports on a single leaf switch, ibdiagnet is the correct fabric diagnostic tool to scan the InfiniBand topology, report per-port symbol/error counters, and pinpoint whether the root cause is a specific bad cable, transceiver, or switch port — the appropriate first step before any workaround or reconfiguration.

---

**Q44. Correct: D**

Since intra-node NVLink bandwidth is already confirmed healthy, the bottleneck must be in the inter-node InfiniBand fabric. Point-to-point ib_write_bw tests isolate exactly which rail, cable, HCA, or port is underperforming, giving a precise diagnosis before any corrective action is taken.

---

**Q45. Correct: A**

Xid 79 indicates the GPU has physically dropped off the PCIe bus, which is a hardware-level failure typically caused by a poor seating, connector, or board fault. The correct first step is physical inspection/reseating, followed by RMA escalation if the error persists after reseating.

---

