# Practice Test 1: NCP-AII: NVIDIA AI Infrastructure Professional Prep

- **Questions:** 45
- **Time limit:** 90 minutes
- **Passing score:** 70%%

> Answers are in `answer-key-1.md`. Track results in `score-tracker.md`.

---

**Q1.** A newly racked DGX H100 system passes POST and boots into its OS, but before joining the cluster the bring-up engineer must confirm that all NVLink connections between the eight GPUs are active. Which command should be used to validate NVLink link status?

A) nvidia-smi nvlink -s
B) nvidia-smi -q -d MEMORY
C) ibstat
D) dmidecode -t processor

---

**Q2.** An administrator must update the BIOS, BMC, and GPU VBIOS firmware on a DGX H100 as part of initial bring-up, before OS re-installation, using NVIDIA's officially supported firmware update mechanism for DGX and NVIDIA-Certified Systems. Which tool should be used?

A) DCGM (dcgmi)
B) nvfwupd
C) nv-fabricmanager
D) MLNX_OFED installer script

---

**Q3.** On a DGX H100 system with NVSwitch, GPU-to-GPU communication over NVLink will not initialize correctly if a particular system service fails to start. Which service must be running to properly bring up the NVSwitch fabric before workloads are launched?

A) nv-fabricmanager
B) rdma-core
C) nvidia-persistenced
D) nvidia-dcgm

---

**Q4.** Immediately after powering on a new NVIDIA-Certified server with 8x H100 GPUs, an engineer needs to run an extended diagnostic that exercises GPU memory, thermal thresholds, and NVLink bandwidth to certify the node is healthy before joining it to the cluster. Which tool is BEST suited for this validation?

A) nvidia-settings
B) DCGM diagnostics (dcgmi diag -r 3)
C) lspci -vvv
D) nvidia-smi -L

---

**Q5.** When running nvidia-smi topo -m on a DGX system during bring-up validation, the output matrix shows connection types between GPU pairs. What does an 'NV4' designation between two GPUs indicate, as opposed to 'PHB'?

A) NV4 indicates the GPU pair is connected via 4 NVLink connections, while PHB indicates the GPUs communicate through a PCIe Host Bridge
B) NV4 indicates PCIe Gen4 link speed, while PHB indicates PCIe Gen3 link speed
C) NV4 indicates 4 NVSwitches are installed in the chassis, while PHB indicates no NVSwitches are present
D) NV4 indicates the GPUs belong to the same 4-GPU NUMA group, while PHB indicates a single-GPU system

---

**Q6.** During bring-up of a new 4-node H100 GPU cluster, three nodes report CUDA driver version 550.90.07 while the fourth node, imaged from a different template, reports 535.129.03. Distributed training jobs spanning all four nodes fail with NCCL initialization errors during initial validation. What is the MOST likely root cause and correct remediation?

A) The mismatch is expected and NCCL automatically negotiates a compatible driver ABI across nodes; investigate the network fabric instead
B) The failure is unrelated to drivers and is caused solely by insufficient InfiniBand bandwidth on the cluster
C) The fourth node needs a newer driver than the others, so all three matching nodes should be downgraded to 535.129.03 instead
D) Driver version mismatch across cluster nodes; reimage or update the fourth node's driver to match the validated version used across the fleet before rejoining the cluster

---

**Q7.** A DGX H100 system was just brought up and cabled. Before certifying the node for production, the administrator needs a single command that displays a full matrix showing the NVLink connection type and bandwidth between every pair of GPUs in the system. Which command should be used?

A) nvidia-smi topo -m
B) lspci -vvv
C) nvidia-smi -L
D) nvidia-smi nvlink -s

---

**Q8.** During initial bring-up of a DGX H100 node that will use GPUDirect RDMA between GPUs and BlueField DPUs, which BIOS/UEFI setting must be verified to allow peer-to-peer PCIe transfers between devices behind the same PCIe switch?

A) Enable Secure Boot
B) Enable PCIe ACS (Access Control Services)
C) Enable Legacy BIOS Compatibility Mode
D) Disable PCIe ACS (Access Control Services)

---

**Q9.** After imaging a new NVIDIA-Certified server with the standard OS image, the administrator runs a health check and `nvidia-smi` fails with a driver/library version mismatch error, preventing GPU enumeration even though the GPUs are physically seated and powered correctly. What is the most likely cause and correct remediation?

A) The nvidia kernel module currently loaded is from an older driver install while the user-space NVML/CUDA libraries were updated; reinstall the driver package and reload the kernel module (or reboot)
B) The GPU firmware is corrupted and the GPUs must be physically replaced
C) An NVLink bridge cable has become disconnected between two GPUs
D) The BMC firmware is outdated and must be flashed before the OS can detect the GPUs

---

**Q10.** Immediately after initial power-on and OS installation on a new DGX system, which NVIDIA tool should be run to perform an automated, system-wide health check covering GPUs, NVLink, local storage, and networking, producing a consolidated pass/fail report before the node is certified for production?

A) nvsm (NVIDIA System Management)
B) dcgmi diag
C) nvidia-smi -q
D) Base Command Manager cmsh

---

**Q11.** A customer is deploying a DGX H100 SuperPOD using a rail-optimized InfiniBand network design. In this topology, how should the ConnectX-7 adapter connected to GPU 0 on every node in the pod be cabled?

A) Each GPU 0 NIC connects to a different leaf switch, load-balanced across all switches in the pod
B) All GPU 0 NICs connect directly to the spine switches, bypassing the leaf layer entirely
C) All GPU 0 NICs from every node connect to the same dedicated leaf switch (rail), and each GPU index maps to its own rail
D) GPU 0 NICs are cabled to the storage fabric switches, while GPU 1-7 NICs connect to the compute fabric

---

**Q12.** A customer's data center standardized on Ethernet switches and wants to build a lossless RDMA fabric for GPU-to-GPU communication without deploying a separate InfiniBand network. Which technology should be configured to meet this requirement?

A) TCP/IP with standard Ethernet flow control
B) InfiniBand EDR overlaid on the existing Ethernet switches
C) RoCEv2 with Priority Flow Control (PFC) and ECN configured end-to-end
D) NVIDIA GPUDirect Storage over NFS

---

**Q13.** An enterprise wants strict separation of duties so that host server administrators cannot alter the network, security, or storage virtualization functions running on a BlueField-3 DPU. Which BlueField deployment mode should be configured?

A) NIC mode
B) DPU mode (default, host has full control)
C) Zero Trust mode
D) Host mode with the Arm subsystem exposed to the host OS

---

**Q14.** A GPU cluster already has a non-blocking InfiniBand fabric for compute traffic. The customer wants their AI training jobs to read/write training data and checkpoints to a shared storage array using RDMA over the same fabric, minimizing CPU overhead. Which storage access method should be deployed?

A) NFS over TCP/IP mounted through the management network
B) iSCSI over a dedicated Ethernet SAN
C) SMB/CIFS file shares over the InfiniBand IPoIB interface
D) NVMe over Fabrics (NVMe-oF) using RDMA transport over the InfiniBand fabric

---

**Q15.** During validation of a DGX cluster, GPUDirect RDMA benchmarks between the GPU and a ConnectX NIC across a PCIe switch show unexpectedly low throughput and fall back to a staged copy through system memory. Which PCIe setting is the most likely cause and should be disabled?

A) PCIe ACS (Access Control Services)
B) PCIe Relaxed Ordering
C) PCIe ASPM (Active State Power Management)
D) PCIe Resizable BAR

---

**Q16.** An architect is planning intra-rack cabling for an NDR (400Gb/s) InfiniBand fabric between DGX systems and top-of-rack switches. For runs longer than approximately 2 meters, which cable type should be specified instead of passive copper DAC?

A) Active Optical Cable (AOC) or optical transceivers with fiber
B) Standard Cat6a Ethernet copper cable
C) A longer passive copper DAC rated for up to 10 meters
D) A USB-C to OSFP adapter cable

---

**Q17.** You are designing the compute fabric for a new 32-node DGX H100 cluster that will run large-scale distributed training jobs using NCCL collectives across all GPUs. The customer requires the lowest possible latency and wants to offload collective operations (like all-reduce) into the network itself. Which fabric technology should you select?

A) Standard 25GbE Ethernet with TCP/IP
B) RoCEv2 over 100GbE switches without SHARP support
C) Fibre Channel SAN fabric at 32Gb/s
D) InfiniBand NDR (400Gb/s) with SHARP-capable Quantum-2 switches

---

**Q18.** A cluster architecture calls for offloading virtual switching, firewalling, and storage virtualization services from host CPUs onto a programmable data-processing accelerator, using NVIDIA's DOCA SDK to build the accelerated services. Which hardware component should be deployed to host these DOCA services?

A) NVIDIA BlueField-3 DPU
B) NVIDIA ConnectX-7 SmartNIC
C) NVIDIA Grace CPU Superchip
D) NVIDIA A100 Tensor Core GPU

---

**Q19.** During RoCEv2 fabric configuration for a GPU cluster's storage and east-west traffic, engineers must establish a lossless network to prevent packet drops that would otherwise stall RDMA transfers. Which configuration approach correctly establishes this lossless behavior?

A) Disable all flow control and rely on TCP retransmission timers to recover any dropped RDMA packets
B) Enable Priority Flow Control (PFC) on the RoCE traffic class and configure ECN marking on switches with DCQCN enabled on the NICs
C) Enable Spanning Tree Protocol on every switch port carrying RoCE traffic to prevent loops
D) Configure jumbo frames only, without enabling PFC or ECN, since larger frames eliminate congestion

---

**Q20.** What is the primary functional difference between GPUDirect RDMA and GPUDirect Storage in an NVIDIA AI infrastructure deployment?

A) GPUDirect RDMA is used only within BlueField DPUs, while GPUDirect Storage is exclusive to DGX systems
B) There is no functional difference; they are two marketing names for the identical data path technology
C) GPUDirect RDMA moves data between storage and GPU memory, while GPUDirect Storage moves data between GPUs over the network
D) GPUDirect RDMA enables direct memory access between GPUs across the network bypassing the CPU, while GPUDirect Storage enables a direct data path between storage devices and GPU memory bypassing CPU bounce buffers

---

**Q21.** A technician is cabling a rail-optimized DGX H100 SuperPOD fabric using NDR InfiniBand (400Gb/s per port). The distance between the compute rack and the NDR leaf switch rack is 50 meters. Which cable/transceiver type should be used for this link?

A) Passive copper direct-attach cable (DAC)
B) Active optical cable (AOC) or fiber transceiver
C) Passive copper splitter/breakout cable
D) RJ45 Cat6a copper patch cable

---

**Q22.** During bring-up validation of a new NDR InfiniBand leaf-spine fabric, a port that should negotiate at 400Gb/s instead links up at 200Gb/s (HDR speed), and `mlxlink` output shows elevated raw bit error rate (BER) on that port. What is the MOST likely physical layer cause?

A) A faulty or contaminated transceiver/cable causing signal integrity degradation, forcing auto-downtraining
B) The OpenSM subnet manager is using an incorrect routing algorithm
C) The host channel adapter (HCA) has an incorrect MTU configured
D) The node is running an outdated MLNX_OFED driver version

---

**Q23.** In a DGX H100 SuperPOD, each GPU's NIC across every node is connected to the same corresponding leaf switch (e.g., all GPU0 NICs go to Leaf Switch 1, all GPU1 NICs go to Leaf Switch 2, etc.). What is this physical topology design called?

A) Spine-leaf mesh topology
B) Ring topology
C) Dragonfly+ topology
D) Rail-optimized fat-tree topology

---

**Q24.** A rack contains 8 DGX H100 systems (each drawing approximately 10.2kW under full AI training load) plus 2 NVLink switch trays (0.5kW each). The facility power design must include 20% headroom for peak/transient loads. What minimum total rack power capacity should be provisioned?

A) 82.6 kW
B) 68 kW
C) 120 kW
D) 99 kW

---

**Q25.** Which command-line utility is used on NVIDIA ConnectX/BlueField adapters to inspect detailed physical layer diagnostics such as effective BER, eye opening, and per-lane error counters on an InfiniBand or Ethernet port?

A) ibstat
B) ethtool
C) mlxlink
D) nvidia-smi

---

**Q26.** When designing a non-blocking (fully non-oversubscribed) fat-tree fabric using NVIDIA Quantum-2 switches for a large GPU cluster, what uplink-to-downlink port ratio must be maintained on each leaf switch?

A) 2:1
B) 1:1
C) 4:1
D) 8:1

---

**Q27.** A data center team is racking DGX H100 systems and connecting each server's NDR InfiniBand ports to leaf switches in the same rack. The required cable run is exactly 3 meters. Which cable type BEST meets this requirement while minimizing cost and power draw?

A) Passive copper Direct Attach Cable (DAC)
B) Active Optical Cable (AOC)
C) Active Copper Cable (ACC)
D) Single-mode fiber with pluggable transceivers

---

**Q28.** A cluster architect is designing a rail-optimized fat-tree fabric to interconnect DGX H100 nodes (8 GPUs, 8 NDR InfiniBand NICs each) so that every GPU's NIC lands on its own dedicated leaf switch across the cluster, eliminating cross-GPU contention on any single leaf. How many leaf switches (rails) are required per rail group?

A) 16
B) 4
C) 2
D) 8

---

**Q29.** A cloud provider runs a shared DGX-based Kubernetes cluster serving multiple untrusted tenants. Each tenant must receive a hardware-isolated GPU partition with dedicated memory and fault isolation, and no tenant may observe another's workload activity. Which GPU Operator feature BEST satisfies these requirements?

A) CUDA time-slicing configured via the GPU Operator ConfigMap
B) Multi-Instance GPU (MIG) partitioning exposing each instance as a separate schedulable resource
C) NVIDIA Multi-Process Service (MPS) shared contexts
D) Assigning the entire physical GPU node exclusively per tenant using node taints only

---

**Q30.** Which NVIDIA GPU Operator component continuously exports per-GPU utilization, memory, temperature, and ECC error metrics for consumption by Prometheus in a BCM-managed Kubernetes cluster?

A) Node Feature Discovery (NFD)
B) GPU Feature Discovery (GFD)
C) DCGM Exporter
D) NVIDIA Container Toolkit

---

**Q31.** An administrator wants a Kubernetes Pod on a GPU Operator-managed cluster to be scheduled with exclusive access to exactly one full physical GPU, with no other pod able to share it. Which resource specification accomplishes this?

A) resources.requests: nvidia.com/gpu: 1 with limits omitted
B) resources.limits: nvidia.com/gpu: 1 and resources.requests: nvidia.com/gpu: 1
C) resources.limits: cpu: 1 combined with nodeSelector: gpu=true
D) resources.limits: nvidia.com/gpu: 0.5 with time-slicing replicas set to 2

---

**Q32.** A team runs large-scale multi-node distributed training jobs on a DGX SuperPOD requiring gang scheduling, backfill, and fair-share quotas across research groups, alongside a separate set of low-latency, always-on inference microservices that need continuous autoscaling and rolling updates. Which orchestration split BEST fits this workload mix?

A) Run both training and inference exclusively on Kubernetes with the default kube-scheduler
B) Use Slurm (via BCM) for the batch training workloads and Kubernetes for the inference microservices
C) Use Kubernetes for training with static pod affinity rules and Slurm for inference to guarantee low latency
D) Consolidate everything onto Slurm, including inference, using srun for each inference request

---

**Q33.** In a Base Command Manager-managed cluster, which mechanism is used to automatically provision new DGX compute nodes with the correct OS image, drivers, and configuration when they are added to a category?

A) Manual SSH configuration scripts run by the administrator after boot
B) Kubernetes DaemonSets deployed after node registration
C) BCM's node-category-based software image with PXE boot provisioning
D) Slurm's node_features plugin

---

**Q34.** Which statement correctly differentiates MIG partitioning from GPU time-slicing when both are configured through the NVIDIA GPU Operator?

A) Time-slicing provides dedicated hardware memory and compute partitions, while MIG only provides scheduling-level context switching
B) Both MIG and time-slicing provide identical hardware-level fault isolation and are interchangeable for multi-tenant security
C) MIG creates hardware-isolated instances with dedicated memory and compute slices, while time-slicing shares the full GPU context across pods without memory isolation
D) MIG is only available for inference workloads, while time-slicing is only available for training workloads

---

**Q35.** A cluster runs two workload types: large-scale distributed training jobs requiring tightly-coupled, topology-aware MPI communication across hundreds of GPUs, and inference microservices that must autoscale rapidly based on fluctuating request traffic. Base Command Manager provisions the underlying nodes. Which orchestration approach BEST fits these requirements?

A) Run Kubernetes with the GPU Operator uniformly for both the training jobs and the inference services
B) Run Slurm for the training jobs and Kubernetes with the GPU Operator for the inference services, with BCM managing the underlying node provisioning for both
C) Run Slurm for both the training jobs and the inference services
D) Rely on BCM node groups alone without deploying Slurm or Kubernetes

---

**Q36.** Which NVIDIA GPU Operator component is responsible for configuring Multi-Instance GPU (MIG) partitions on supported GPUs within a Kubernetes cluster?

A) NVIDIA Container Toolkit
B) Node Feature Discovery (NFD)
C) MIG Manager
D) DCGM Exporter

---

**Q37.** Multiple teams share a Kubernetes cluster built on A100 GPUs. The cluster admin must guarantee hardware-level compute and memory isolation for each team's workloads, including small inference jobs that don't need a full GPU. Which configuration BEST meets this requirement?

A) Enable GPU time-slicing so all pods share full GPUs concurrently without dedicated partitions
B) Configure MIG on the A100 GPUs to create isolated GPU instances, and expose them as schedulable resources through the GPU Operator
C) Assign each team an entire physical GPU using node affinity and taints
D) Schedule all workloads on CPU-only nodes and let GPU access be arbitrated implicitly at the driver level

---

**Q38.** A DGX A100 node's NCCL-based multi-GPU training job shows a 40% throughput drop compared to baseline. GPU SM utilization looks normal, and nvidia-smi shows no thermal or power throttling. Which command should the administrator run FIRST to check for a degraded NVLink connection between GPU pairs?

A) dmesg | grep -i gpu to search kernel logs for GPU messages
B) ibstat to check InfiniBand port state
C) nvidia-smi -q -d POWER to review power draw
D) nvidia-smi nvlink -s to check per-link status and error counters

---

**Q39.** An administrator managing a Base Command Manager-controlled GPU cluster wants a single tool that can run multi-level GPU health diagnostics — including memory bandwidth tests, NVLink validation, and extended stress tests — and can be scheduled cluster-wide for proactive health monitoring. Which tool should be used?

A) NVIDIA Data Center GPU Manager (dcgmi diag)
B) nvtop
C) nvidia-bug-report.sh
D) nvidia-smi topo -m

---

**Q40.** To minimize GPU driver initialization latency between successive job launches on a DGX system, which configuration should an administrator enable?

A) MIG mode partitioning on all GPUs
B) Persistence mode via 'nvidia-smi -pm 1'
C) Exclusive process compute mode
D) Disabling ECC memory protection

---

**Q41.** During a sustained large-batch training run, GPUs on a DGX node are observed running below their rated base clock. Running 'nvidia-smi -q -d PERFORMANCE' shows 'SW Power Cap: Active' as the current throttle reason, while temperatures remain well within limits. What is the correct diagnostic conclusion and remediation?

A) Driver/firmware mismatch — reinstall the GPU driver and update firmware
B) NVLink degradation — replace the NVLink bridge or cable
C) ECC errors accumulating — disable ECC to restore clock speed
D) Power limit is capping performance — raise the limit with 'nvidia-smi -pl' if PSU/rack power budget allows

---

**Q42.** When reviewing GPU error counts with 'nvidia-smi -q -d ECC', the output separates 'Volatile' and 'Aggregate' error counts. What is the correct distinction between these two categories?

A) Volatile applies only to GPU memory errors, while Aggregate applies only to NVLink errors
B) Volatile counts correctable errors, while Aggregate counts uncorrectable errors
C) Volatile counts reset on driver reload or GPU reset, while Aggregate counts persist as a lifetime total across reboots
D) There is no functional difference — both report identical values in a different display format

---

**Q43.** A cluster health check reveals elevated symbol error counts and repeated packet retransmissions on multiple InfiniBand ports, all connected to the same leaf switch, across several GPU compute nodes performing NCCL collective operations. What should the administrator do as the first diagnostic step?

A) Increase the NCCL_IB_TIMEOUT environment variable to mask the retransmissions
B) Disable RDMA and force NCCL to fall back to TCP/IP transport
C) Run ibdiagnet to identify the specific affected links, ports, and cable/switch health on that leaf switch
D) Reinstall the GPU driver on all affected compute nodes

---

**Q44.** A DGX SuperPOD cluster runs an 8-node NCCL all-reduce benchmark and observes only ~60% of the expected aggregate bus bandwidth, even though single-node NVLink bandwidth tests (nvidia-smi nvlink and p2pBandwidthLatencyTest) report full expected throughput on every node. Which action BEST isolates the root cause?

A) Re-run nvidia-smi nvlink -s on each node again to confirm NVLink health
B) Increase the NCCL_BUFFSIZE environment variable and re-run the all-reduce test
C) Power-cycle the leaf switch connecting the affected nodes
D) Run point-to-point ib_write_bw tests between node pairs across the fabric to identify a slow rail, cable, or port

---

**Q45.** On a DGX H100 node, dmesg repeatedly logs 'NVRM: Xid 79: GPU has fallen off the bus' correlated with GPU 3, and the GPU periodically disappears from nvidia-smi output under sustained training load. Which remediation is MOST appropriate?

A) Physically inspect and reseat GPU 3's PCIe and power connections, and escalate to hardware RMA if the Xid 79 recurs
B) Reset ECC error counters using nvidia-smi -r and continue running the workload
C) Raise the GPU power limit with nvidia-smi -pl to prevent throttling-induced disconnects
D) Upgrade the NCCL library to the latest release and retest the workload

---

