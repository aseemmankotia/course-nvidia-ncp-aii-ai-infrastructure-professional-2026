## Chapter 4 Cheat Sheet: InfiniBand, RoCE, BlueField DPU

### InfiniBand Fabric Basics
| Component | Role |
|---|---|
| HCA (Host Channel Adapter) | Server-side InfiniBand NIC with RDMA offload |
| Subnet Manager (SM) | Discovers topology, computes routing tables, assigns LIDs, manages partitions; fabric is inoperable without an active SM |
| UFM | NVIDIA's fabric management software hosting the SM at scale |

**Speed generations:** NDR = 400Gb/s/port, XDR = 800Gb/s/port.

### RoCEv2 Lossless Requirements
| Mechanism | Standard | Function |
|---|---|---|
| PFC | IEEE 802.1Qbb | Per-priority pause to prevent buffer overflow |
| ECN | RFC 3168 | Marks packets under congestion (no drop) |
| DCQCN | NVIDIA/Mellanox | Combines PFC + ECN for lossless, rate-adaptive RoCEv2 |

**Exam trap:** Disabling PFC/ECN does NOT improve throughput — it breaks RDMA reliability by allowing packet loss.

### Fabric Selection Criteria
| Requirement | Choose |
|---|---|
| Lowest latency, largest scale (100s-1000s GPUs), native adaptive routing/SHARP | InfiniBand |
| Ethernet compatibility, existing DC integration, multi-tenant flexibility | RoCEv2 / Spectrum-X |

### Spectrum-X Platform
- Combines **Spectrum switches** (e.g., Spectrum-4) + **BlueField DPUs** (e.g., BlueField-3)
- Delivers InfiniBand-like adaptive routing and congestion control over standard Ethernet

### BlueField DPU Offload Use Cases
- Storage virtualization / NVMe-oF acceleration
- Network offload (OVS, RoCE processing) — frees host CPU
- Security & multi-tenant isolation at line rate

### DOCA Framework
- SDK providing APIs, libraries, drivers to build/deploy services on BlueField DPUs
- Analogy: DOCA is to BlueField what CUDA is to GPUs
- Not a switch, not a storage protocol, not an SM replacement

### Storage Fabric Options
| Option | Transport | Best For |
|---|---|---|
| NVMe-oF | RoCEv2 or InfiniBand | Low-latency remote block storage |
| Parallel filesystem (Lustre, GPFS/Spectrum Scale, WEKA, VAST) | RDMA over IB/RoCE | Multi-node training checkpoint I/O at scale |
| NFS/SMB | TCP/Ethernet | NOT suitable for large-scale distributed training I/O |

**Common exam traps:**
1. Assuming PFC alone makes Ethernet lossless (ECN also required).
2. Choosing standard NFS for multi-node AI checkpoint storage.
3. Confusing DOCA (framework) with DPU hardware or with the InfiniBand SM.
