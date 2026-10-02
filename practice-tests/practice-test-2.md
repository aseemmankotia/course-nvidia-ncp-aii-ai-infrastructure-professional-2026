# Practice Test 2: NCP-AII: NVIDIA AI Infrastructure Professional Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-2.md`. Track results in `score-tracker.md`.

---

**Q1.** A company just racked a new DGX H100 node. After OS and driver installation, engineers must verify all 8 GPUs have full NVLink connectivity through the 4 onboard NVSwitches before moving the node into production. Which command should they run to confirm the NVLink topology matches the expected DGX H100 all-to-all connectivity?

A) nvidia-smi -q -d PERFORMANCE
B) ibstat
C) dcgmi discovery -l
D) nvidia-smi topo -m

---

**Q2.** After initial bring-up of an NVIDIA-Certified server with 8x H100 GPUs, the deployment team wants to run an extended hardware diagnostic that stresses the GPUs and validates NVLink bandwidth before handing the system to production workloads. Which command correctly performs this validation?

A) dcgmi diag -r 3
B) nvidia-smi -pm 1
C) dcgmi diag -r 1
D) nvidia-bug-report.sh

---

**Q3.** During BIOS validation on a new NVIDIA-Certified server intended for multi-GPU training with GPUDirect RDMA over InfiniBand, engineers observe that GPU peer-to-peer communication and RDMA throughput are far below expected values. Which BIOS/PCIe setting should be checked and corrected first?

A) Enable Access Control Services (ACS) on all PCIe root ports
B) Enable SR-IOV on the GPU PCIe slots
C) Disable Above 4G Decoding
D) Disable Access Control Services (ACS) on the PCIe root ports used by GPUs and NICs

---

**Q4.** While bringing up a new 4-node DGX H100 SuperPOD segment, NCCL all-reduce tests intermittently fail with topology errors on one specific node while the other three nodes complete successfully. All four nodes were imaged from the same OS and driver template. What should be investigated FIRST as the most likely root cause?

A) Insufficient host DRAM capacity on the failing node
B) Mismatched GPU VBIOS or NVSwitch firmware versions on the failing node
C) A different Linux kernel scheduler configuration
D) The NCCL_DEBUG environment variable not being set

---

**Q5.** During bring-up documentation review for a DGX H100 system, a new engineer asks how NVLink and NVSwitch relate to each other. Which statement is correct?

A) NVLink and NVSwitch are two names for the same physical interconnect standard
B) NVSwitch replaces NVLink entirely in H100 systems, so no NVLink connections exist
C) NVLink is the point-to-point interconnect protocol/links, while NVSwitch is the switching ASIC that interconnects multiple NVLinks to provide all-to-all GPU connectivity
D) NVLink connects GPUs to storage, while NVSwitch connects GPUs to the CPU

---

**Q6.** A newly racked DGX H100 fails to complete a normal power-on sequence: the chassis fans spin up, but the system never POSTs and no output appears on console redirection. Following standard bring-up troubleshooting procedure, what should be checked FIRST via the BMC before escalating to hardware replacement?

A) Reflash the GPU VBIOS remotely via the BMC
B) Reinstall the DGX OS from the recovery ISO
C) Review the BMC System Event Log (SEL) and sensor readings for power-supply, voltage-rail, or CPU/GPU fault events
D) Run nvidia-smi -q from the host OS

---

**Q7.** A technician completes hardware installation of a new DGX H100 system and runs `nvidia-smi topo -m` to verify NVLink topology before production cutover. NVIDIA's reference topology for DGX H100 shows every GPU pair connected through NVSwitch with 18 NVLink connections (NV18), but the matrix output shows one GPU pair connected with only NV12. What should the technician do NEXT?

A) Check the nvidia-fabricmanager logs and physically inspect/reseat the NVLink/NVSwitch baseboard connections for the affected GPU pair, since NV12 indicates a degraded or partially failed NVLink path
B) Reinstall the CUDA toolkit, since a reduced link count indicates a software stack compatibility problem
C) Ignore the discrepancy, since NV12 and NV18 both represent fully functional GPU-to-GPU peer connectivity
D) Downgrade the GPU driver so it matches the number of active NVLink connections reported

---

**Q8.** After installing the NVIDIA driver and CUDA toolkit on a new DGX H100 node, an engineer runs a GPU peer-to-peer bandwidth test and finds that GPUs cannot communicate over NVLink even though `nvidia-smi` correctly lists all 8 GPUs. What is the MOST likely cause?

A) The nv-fabricmanager (NVIDIA Fabric Manager) service is not running, so the NVSwitch fabric has not been initialized
B) The GPUs must be power-cycled individually through IPMI before NVLink becomes active
C) The installed CUDA toolkit version is incompatible with NVLink peer-to-peer communication
D) NVLink on H100 requires a separate license key that has not been activated

---

**Q9.** An infrastructure engineer is bringing up a new NVIDIA-Certified server before OS installation and must confirm that BIOS, BMC, GPU VBIOS, and NVSwitch firmware versions all match NVIDIA's officially validated firmware bundle for the target driver/CUDA release. Which tool should be used to perform this validation?

A) NVIDIA Firmware Update Tool (nvfwupd)
B) DCGM diagnostics at run level 3
C) Base Command Manager's node imaging profile
D) nvidia-smi --query-gpu=driver_version

---

**Q10.** A team is preparing to run CUDA 12.4 workloads across a cluster of DGX H100 nodes. According to NVIDIA's CUDA compatibility matrix, what is the minimum NVIDIA Linux driver branch required to support CUDA 12.4?

A) R470 (470.xx)
B) R525 (525.xx)
C) R550 (>=550.54.14)
D) R535 (535.xx)

---

**Q11.** A DGX H100 SuperPOD scalable unit uses a rail-optimized InfiniBand fat-tree. Each DGX H100 node has 8 ConnectX-7 adapters dedicated to the compute fabric. To preserve rail optimization, how should these 8 adapters be cabled to the leaf switch layer?

A) All 8 adapters from a single node connect to the same leaf switch to minimize switch count
B) Each of the 8 adapters connects to a different leaf switch, so adapter N from every node in the SU lands on leaf switch N
C) The 8 adapters are bonded into a single logical link using LACP before connecting to any leaf switch
D) Adapters are split evenly across only 2 leaf switches, with 4 adapters per switch

---

**Q12.** A security team requires that the BlueField-3 DPU's embedded Arm subsystem independently own and enforce all NIC configuration and network policy, so that the compromised or untrusted x86 host cannot alter networking behavior. Which BlueField operating mode should be configured on the DPU?

A) NIC mode
B) Legacy passthrough mode
C) SR-IOV bridged mode
D) DPU mode (embedded function ownership)

---

**Q13.** A cluster uses 400GbE RoCEv2 for GPU-to-GPU NCCL traffic instead of InfiniBand. Engineers observe intermittent packet drops on the switches during large all-reduce operations, causing NCCL collective timeouts. The switches currently have flow control disabled entirely. Which change is MOST likely to resolve the timeouts?

A) Increase the MTU on all switch interfaces to 9000 bytes
B) Disable RDMA and fall back to TCP sockets for NCCL transport
C) Replace the DAC copper cables with longer AOC cables
D) Enable Priority Flow Control (PFC) on the RoCE traffic class and configure ECN/WRED marking for congestion notification

---

**Q14.** An AI training cluster needs to store and retrieve model checkpoints at the highest possible throughput with the lowest latency, and wants to leverage GPUDirect Storage to move data directly between NVMe storage and GPU memory, bypassing the CPU. Which storage connectivity approach should be deployed?

A) NFS over TCP/IP to a general-purpose file server
B) iSCSI over a standard Ethernet management network
C) NVMe-oF over RDMA-capable fabric (InfiniBand or RoCE)
D) Fibre Channel SAN using traditional HBAs

---

**Q15.** A shared A100 GPU node will host several small, isolated inference workloads that each require guaranteed compute and memory partitions with hardware-level fault isolation. After confirming the GPU supports MIG, which command correctly places the GPU into MIG mode before creating instances?

A) nvidia-smi -c EXCLUSIVE_PROCESS
B) nvidia-smi mig -cgi 1g.5gb
C) nvidia-smi -mig 1
D) nvidia-smi -pm 1

---

**Q16.** You are planning intra-rack InfiniBand NDR 400Gb/s links where some runs exceed 3 meters between a DGX H100 and a leaf switch in an adjacent rack. Which cabling choice is appropriate for this run length at NDR speeds?

A) Passive DAC copper cable rated for NDR
B) Active optical cable (AOC) or optical transceivers with MPO fiber
C) Standard Cat6a Ethernet patch cable
D) Passive copper cable with a signal repeater spliced mid-run

---

**Q17.** A customer deploys a cluster of DGX H100 nodes (8 GPUs each, 8x ConnectX-7 NDR/400GbE NICs dedicated exclusively to the GPU compute fabric) and requires a rail-optimized, non-blocking topology to minimize NCCL collective latency. How many leaf switches should be provisioned for the GPU compute fabric?

A) 1 shared leaf switch handling all GPU NICs from every node
B) 4 leaf switches, with each leaf serving two GPU rails
C) 8 leaf switches, one dedicated rail per GPU position across all nodes
D) 16 leaf switches, two redundant switches per GPU rail

---

**Q18.** An architect wants BlueField-3 DPUs to run the virtual switch, firewall, and storage-offload infrastructure services such that the host x86 CPU has no ability to configure or observe this networking datapath, achieving a zero-trust isolation model. Which BlueField operating mode must be configured?

A) NIC mode, where the DPU behaves as a standard NIC and the host OS owns all network configuration
B) Separated mode, where the host and DPU each independently manage their own portion of the network stack
C) Restricted mode, a legacy ConnectX-only configuration state with no DPU offload capability
D) DPU mode, where the embedded Arm cores act as the trusted control point and the host is isolated from configuring the datapath

---

**Q19.** During installation of a RoCEv2 fabric for GPU-to-GPU traffic, engineers observe intermittent packet drops under sustained load, causing NCCL operation timeouts. Which switch-level configuration must be validated to make the Ethernet fabric lossless for RoCEv2?

A) Enabling jumbo frames (MTU 9000) alone on all switch ports
B) Disabling all flow control mechanisms to reduce switch buffer overhead
C) Configuring Priority Flow Control (PFC) alongside ECN with DCQCN congestion control
D) Enabling Spanning Tree Protocol on every switch port in the fabric

---

**Q20.** A cluster needs to stream training datasets directly from NVMe-oF storage targets into GPU memory, bypassing the CPU host memory bounce buffer to reduce both latency and CPU utilization. Which technology combination must be validated during storage connectivity installation?

A) GPUDirect Storage (GDS) with the nvidia-fs kernel driver over an RDMA-capable NIC/DPU path to the storage target
B) NFS over TCP relying on the standard kernel page cache
C) iSCSI over a standard TCP/IP software initiator
D) SMB Direct with RDMA on a Windows Server storage target

---

**Q21.** A leaf switch in an InfiniBand fat-tree fabric has 40 total ports: 32 connected to compute node NICs and 8 uplinks to spine switches. What is the oversubscription ratio of this leaf switch, and what does it indicate?

A) 1:1 — the fabric is non-blocking and suitable for full bisection bandwidth workloads
B) 8:1 — the fabric requires additional spine switches to reach non-blocking status
C) 1:4 — the fabric has more uplink capacity than needed, indicating overprovisioning
D) 4:1 — the fabric is oversubscribed and may bottleneck bandwidth-intensive collective operations

---

**Q22.** In a rail-optimized fat-tree design for a DGX H100 SuperPOD, each of the 8 GPU NICs (one per rail) on a node connects to a dedicated leaf switch reserved solely for that rail. What is the primary benefit of this design for distributed training workloads?

A) It reduces the total number of leaf switches required in the fabric
B) It eliminates the need for NVLink within a DGX node
C) It allows GPUs occupying the same rail position across different nodes to communicate without traversing the spine layer
D) It guarantees a 1:1 oversubscription ratio regardless of spine switch count

---

**Q23.** A GPU cluster's spine switches are located in a separate row of racks 50 meters from the compute racks. The fabric uses NDR (400Gb/s) InfiniBand. Which cabling solution should be used for these inter-row links?

A) Passive copper DAC cables, rated for reliable operation up to 3 meters at NDR speeds
B) Active copper cables (ACC), which extend copper reach to approximately 50-70 meters at NDR speeds
C) Standard Cat6a Ethernet copper cabling, supporting 400Gb/s at distances up to 100 meters
D) Active optical cables (AOC), which integrate transceivers and support signal integrity over distances up to 100 meters

---

**Q24.** During physical-layer troubleshooting of a ConnectX-7 port suspected of marginal signal quality, which command-line utility provides RX/TX optical power levels, transceiver temperature, and per-lane symbol error counts for the specific cable?

A) mlxlink
B) nvidia-smi
C) ibdiagnet
D) ethtool

---

**Q25.** A data center rack is fed by a 3-phase 208V PDU circuit rated at 60A per phase. Each DGX H100 system has a maximum power draw of 10.2kW. Applying the NEC 80% continuous-load derating rule, how many DGX H100 systems can this single circuit safely support?

A) 1
B) 2
C) 3
D) 4

---

**Q26.** Which physical-layer monitoring approach correctly identifies a degrading fiber cable in an InfiniBand fabric before it causes a fatal link failure?

A) Reviewing periodic ibdiagnet cable-info reports for rising symbol error counts and abnormal transceiver temperature on specific links
B) Monitoring GPU utilization metrics via DCGM to detect a drop in training throughput
C) Checking Base Command Manager job scheduler logs for failed job submissions
D) Running nvidia-smi topo -m to view the GPU topology matrix

---

**Q27.** You are cabling a rail-optimized fabric for 32 DGX H100 nodes. GPU-to-leaf-switch links stay within the rack (under 3 m), but leaf-to-spine links must span 30 m across rows at full NDR 400 Gb/s InfiniBand rate. Which cabling approach correctly meets this requirement?

A) Use passive copper DAC (direct-attach copper) cables for all leaf-to-spine links to minimize latency and cost
B) Use 400G NDR active optical cables (AOC) with OSFP connectors rated for the 30 m spine run, reserving passive copper DAC only for the short GPU-to-leaf connections within the rack
C) Use 100G QSFP28 breakout cables, aggregating four 100G lanes per NDR port to reach the spine
D) Use standard Ethernet SFP+ 10G fiber cables and rely on switch auto-negotiation to reach 400G

---

**Q28.** During pre-production validation of a DGX H100 SuperPOD, you run mlxlink on a suspect NDR 400G InfiniBand link. Over successive polls, raw BER trends upward from 1e-8 to 1e-6 while effective (post-FEC) BER stays at 0 and no symbol errors are logged at the port. What is the correct interpretation and action?

A) The link is healthy and FEC is functioning as designed, so no action is needed since effective BER is 0
B) The counters indicate a ConnectX-7 firmware bug; update the NIC driver to clear the reported BER
C) The rising raw BER is a normal artifact of periodic link retraining and will self-correct once the subnet manager reruns discovery
D) The rising raw BER indicates a degrading physical link that Reed-Solomon FEC is currently masking; proactively replace the cable or transceiver before it exceeds the FEC correction ceiling and causes packet loss

---

**Q29.** A company runs large-scale LLM training jobs (multi-node, multi-GPU, requiring full un-partitioned GPU access per job) and also serves many small inference microservices that each need only a fraction of a GPU. They want to run both workload types on the same physical GPU cluster managed by Kubernetes with the NVIDIA GPU Operator, without under-utilizing GPUs for inference. Which configuration BEST meets these requirements?

A) Enable MIG on all GPUs cluster-wide and schedule both training and inference jobs against MIG instances
B) Enable time-slicing on all nodes and let the Kubernetes scheduler oversubscribe GPUs for both training and inference
C) Abandon Kubernetes entirely and run all workloads under Slurm instead
D) Partition the cluster into node pools: MIG-enabled nodes for inference workloads and non-partitioned nodes for training workloads, using node labels/taints to steer workload placement

---

**Q30.** A platform team needs strict per-tenant fair-share scheduling with backfill, gang scheduling for tightly-coupled multi-node MPI training jobs, and detailed job accounting across departments, on a bare-metal DGX SuperPOD. Which orchestration approach is MOST appropriate?

A) Slurm workload manager integrated with Base Command Manager, using partitions, QOS, and fairshare accounting
B) Kubernetes with the default kube-scheduler and per-namespace ResourceQuotas
C) Docker Swarm with GPU resource reservations
D) Kubernetes with GPU Operator and a Horizontal Pod Autoscaler

---

**Q31.** An administrator wants to enable GPU time-slicing on a Kubernetes cluster using the NVIDIA GPU Operator so that 4 pods can share a single physical GPU for lightweight inference. Which is the correct configuration approach?

A) Set the GPU Operator's MIG strategy to 'mixed' in the ClusterPolicy
B) Manually edit the containerd runtime class to inject fractional GPU limits
C) Create a ConfigMap defining a time-slicing configuration with replicas set to 4, then reference it in the ClusterPolicy's device plugin configuration and label the target nodes accordingly
D) Set nvidia.com/gpu.count=4 as a resource limit in the pod spec

---

**Q32.** An organization operates a cluster containing both DGX H100 and DGX A100 nodes managed under a single Base Command Manager head node. They need different software images, driver versions, and Slurm partitions for each hardware generation while maintaining a single pane of management. What is the BEST approach in BCM?

A) Install a single software image with the newest driver version for both node types
B) Create separate BCM categories for H100 and A100 nodes, each with its own software image, and map categories to distinct Slurm partitions
C) Deploy two separate BCM head nodes, one for each hardware generation
D) Use identical node categories but override driver packages manually via cron jobs on each node

---

**Q33.** Which statement correctly differentiates MIG (Multi-Instance GPU) from time-slicing for GPU sharing on NVIDIA GPUs in a Kubernetes environment?

A) Time-slicing requires an Ampere or newer GPU architecture, while MIG works on any GPU generation
B) MIG provides hardware-level isolation of compute and memory into fixed instances, while time-slicing shares the full GPU's compute in a round-robin fashion with no memory isolation
C) MIG allows an unlimited number of dynamically resizable partitions, while time-slicing supports at most 2 slices per GPU
D) Both MIG and time-slicing provide identical memory isolation guarantees, differing only in the scheduling algorithm used

---

**Q34.** A shared GPU Kubernetes cluster serves two tenants: Tenant A runs sensitive fine-tuning jobs that must run exclusively on H100 nodes, and Tenant B runs general inference workloads that must never be scheduled onto those H100 nodes. Both tenants use separate namespaces. Which combination of controls BEST enforces this isolation while still allowing tenant-based scheduling?

A) Rely solely on per-namespace ResourceQuotas to limit each tenant's total GPU count
B) Apply node taints on the H100 nodes, grant the corresponding toleration only to Tenant A's pods via an admission control policy, and add node affinity rules in Tenant A's pod specs
C) Use only NetworkPolicies to isolate traffic between the two tenant namespaces
D) Rely on the default Kubernetes scheduler's bin-packing behavior to naturally separate workloads by resource size

---

**Q35.** A DGX A100 cluster running Kubernetes with the NVIDIA GPU Operator must support two workload types simultaneously: large multi-GPU training jobs that require exclusive access to full physical GPUs, and dozens of small inference microservices that each need roughly 1/7 of a GPU's compute with hard memory isolation between tenants. Which GPU Operator configuration BEST satisfies both requirements on the same cluster?

A) Set the GPU Operator's MIG strategy to "mixed" and label/taint a subset of nodes for MIG-partitioned inference GPUs while leaving training nodes as non-MIG, full-GPU allocations
B) Enable GPU time-slicing uniformly across every GPU in the cluster so all workloads share compute cycles
C) Enable CUDA MPS on all GPUs in the cluster so training and inference processes share GPU contexts concurrently
D) Set the GPU Operator's MIG strategy to "single" with one MIG profile applied cluster-wide so all nodes present identical GPU resources

---

**Q36.** A research organization runs large multi-node training jobs that require gang scheduling, backfill, fair-share allocation across departments, and topology-aware placement on NVLink/InfiniBand domains, while also operating long-running, auto-scaling inference microservices. Which orchestration approach is the BEST fit for this mixed workload profile?

A) Run all training and inference workloads on Kubernetes using only the default kube-scheduler
B) Eliminate Slurm entirely and represent all training jobs as Kubernetes Job objects, using namespace ResourceQuotas to approximate fair-share
C) Deploy Slurm for the training/HPC batch workloads and Kubernetes for the inference microservices, with BCM managing and integrating both environments across the shared hardware
D) Deploy Slurm cluster-wide, including the inference microservices, to keep scheduling logic in a single system

---

**Q37.** A BCM-managed cluster of 8 DGX H100 nodes must shift its workload allocation on a schedule: during business hours, 6 nodes serve low-latency inference and 2 serve training; overnight, the ratio flips to 6 training / 2 inference for a nightly retraining job. The solution must avoid re-imaging nodes on every cycle. Which approach BEST implements this in BCM?

A) Reimage the affected nodes via PXE boot twice daily, switching each node's OS image between a Kubernetes-configured image and a Slurm-configured image
B) Define "training" and "inference" node categories in BCM with their respective Slurm/Kubernetes role configurations, and use a scheduled cmsh-driven script to move nodes between categories at the required times
C) Statically partition every GPU in the cluster into a fixed 50/50 MIG split and leave the split unchanged across the day/night cycle
D) Rely solely on Kubernetes taints and tolerations to shift workloads, since BCM does not support reassigning a node's role or configuration after initial provisioning

---

**Q38.** During routine health checks on a DGX H100 node, `nvidia-smi -q -d XID` reports repeated Xid 79 events on GPU 3, and the GPU subsequently disappears from `nvidia-smi -L` output until the node is rebooted. What is the correct diagnostic and remediation path?

A) Reinstall the NVIDIA driver stack and reboot the node, since Xid 79 indicates a corrupted kernel module
B) Enable persistence mode and disable ECC on GPU 3 to suppress the error, then resume production workloads
C) Update NCCL to the latest release, since Xid 79 is caused by a collective communication library incompatibility
D) Confirm Xid 79 ('GPU has fallen off the bus') via system logs, then treat it as a hardware fault requiring GPU reseating or RMA rather than a software fix

---

**Q39.** On an 8-GPU NVLink-connected node, `nvidia-smi nvlink --status` shows seven links reporting 'NVLink Speed: 25 GB/s' but one link between GPU0 and GPU4 reporting 'NVLink Speed: 6.25 GB/s'. All-reduce jobs using that GPU pair show measurable slowdowns. What should be checked next to isolate the cause?

A) Assume this is expected variance and re-run the benchmark multiple times to average out the result
B) Reduce the NCCL_ALGO setting to Ring for all jobs to avoid using the affected link entirely
C) Increase the GPU power limit on GPU0 and GPU4 to force the link to renegotiate at full speed
D) Check `nvidia-smi nvlink -e` for that specific link's error/replay counters and inspect dmesg for Xid 74 (NVLink error) events before considering GPU or baseboard replacement

---

**Q40.** A GPU cluster's multi-node NCCL all-reduce throughput measured with `all_reduce_perf` is 40% below the vendor-published expected bandwidth. Which command should be run FIRST to determine whether the bottleneck is in the physical InfiniBand fabric rather than the NCCL software stack?

A) Re-run all_reduce_perf with a larger message size across all nodes
B) ib_write_bw between the two endpoint HCAs to measure raw RDMA bandwidth independent of NCCL
C) dcgmi diag -r 3 on each node to run the full GPU diagnostic level
D) nvidia-smi topo -m to print the PCIe/NVLink topology matrix

---

**Q41.** During sustained LLM training, `nvidia-smi -q -d PERFORMANCE` shows 'Clocks Throttle Reasons: SW Power Cap = Active' on all GPUs in a node, while GPU temperature stays 15°C below the thermal slowdown threshold. The rack's PDU still has 20% headroom before its circuit limit. Which action MOST directly resolves the throttling without violating power constraints?

A) Replace the thermal paste on each GPU heatsink to improve cooling
B) Manually lower the GPU application clocks to reduce throttling frequency
C) Increase the GPU power limit with `nvidia-smi -pl` up toward the board's max TDP, within the available rack power headroom
D) Enable ECC memory on all GPUs to reduce overall power draw

---

**Q42.** Which NVIDIA Xid error code specifically indicates an NVLink error, as opposed to a GPU memory ECC or general driver fault?

A) Xid 48 (double-bit ECC error)
B) Xid 74 (NVLink error)
C) Xid 63 (row remapping event)
D) Xid 31 (GPU memory page fault)

---

**Q43.** A distributed training job scales nearly linearly up to 4 nodes but throughput plateaus beyond 8 nodes. Per-GPU SM utilization remains at 95%+ throughout, and per-node network interface counters show links are not saturated. What is the MOST likely root cause to investigate first?

A) The NCCL collective algorithm selection or spine-switch congestion during all-reduce is degrading effective collective bandwidth at larger scale, even though per-link utilization looks fine
B) GPU memory is exhausted, causing repeated out-of-memory recovery on larger node counts
C) Data loader CPU throughput is the bottleneck starving the GPUs of input batches
D) A driver version mismatch exists between the newly added nodes and the original cluster nodes

---

**Q44.** A DGX H100 cluster shows NCCL all-reduce bandwidth degraded by roughly 40% compared to the established baseline, immediately following a routine BIOS update on all nodes. nvidia-smi reports all GPUs healthy, NVLink status is nominal, and no Xid errors appear in dmesg. Which diagnostic action BEST identifies the root cause?

A) Check PCIe Access Control Services (ACS) status with `lspci -vvv` on the PCIe switches/root ports and disable ACS if it was re-enabled by the BIOS update
B) Run `nvidia-smi topo -m` to re-verify the GPU-to-GPU NVLink and PCIe topology map
C) Reinstall the NCCL library to the latest version to rule out a software regression
D) Increase the NCCL_BUFFSIZE environment variable to compensate for reduced throughput

---

**Q45.** During a multi-day LLM pretraining job on a DGX A100 node, dmesg repeatedly logs 'NVRM: Xid (PCI:0000:07:00): 79, GPU has fallen off the bus,' and training crashes intermittently. A `dcgmi diag -r 3` run before the job started completed with no failures. What is the MOST likely root cause and correct remediation?

A) GPU driver corruption; reinstall the NVIDIA driver stack
B) Insufficient or unstable power delivery to the GPU baseboard (e.g., degraded PSU or loose power connector) causing the GPU to disconnect from the PCIe bus; inspect and reseat power connections, and replace faulty PSU/riser hardware as needed
C) NVLink bridge misconfiguration; reseat the NVLink bridges between GPU pairs
D) Accumulating ECC memory errors on the GPU; reset the device with `nvidia-smi -r`

---

