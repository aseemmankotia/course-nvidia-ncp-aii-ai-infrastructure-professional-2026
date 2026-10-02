## Chapter 4: InfiniBand, RoCE, and BlueField DPU Deployment — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What does HCA stand for in InfiniBand? | Host Channel Adapter — the InfiniBand NIC installed in servers connecting them to the fabric. Exam tip: functionally similar to a NIC but with built-in RDMA offload. |
| 2 | What component manages InfiniBand routing, topology, and partitions? | Subnet Manager (SM) — runs on a switch or host (e.g., via UFM). Exam tip: without an active SM, the fabric will not pass traffic. |
| 3 | IEEE standard for Priority Flow Control (PFC) used in lossless RoCEv2 Ethernet? | IEEE 802.1Qbb — pauses traffic per priority class to prevent buffer overflow. Exam tip: PFC alone can cause head-of-line blocking; must pair with ECN. |
| 4 | What RFC defines Explicit Congestion Notification (ECN) used with RoCEv2? | RFC 3168 — marks packets under congestion instead of dropping them. Exam tip: ECN marking triggers sender rate reduction via DCQCN. |
| 5 | What algorithm combines PFC and ECN for RoCEv2 congestion control? | DCQCN (Data Center Quantized Congestion Notification) — ECN provides end-to-end signaling, PFC is the lossless fallback. Exam tip: DCQCN is the standard scheme for RoCEv2 at GPU-cluster scale. |
| 6 | What two hardware components form the NVIDIA Spectrum-X platform? | Spectrum switches (e.g., Spectrum-4) + BlueField DPUs (e.g., BlueField-3) — delivers Ethernet-based AI networking with adaptive routing and congestion control. Exam tip: Spectrum-X targets large GPU clusters needing Ethernet compatibility. |
| 7 | Name 3 BlueField DPU offload use cases tested on the exam. | 1) Storage virtualization / NVMe-oF offload, 2) Network offload (OVS, RoCE processing), 3) Security/multi-tenant isolation at line rate. Exam tip: DPU offload frees host CPU cycles for AI workloads. |
| 8 | What is DOCA in the BlueField deployment stack? | Data Center Infrastructure-on-a-Chip Architecture — the SDK/framework providing APIs, libraries, and drivers to build and deploy services on BlueField DPUs. Exam tip: DOCA is to BlueField what CUDA is to GPUs. |
| 9 | What two transports can carry NVMe-oF traffic in AI clusters? | RoCEv2 (Ethernet) and InfiniBand — both provide RDMA transport for remote NVMe access. Exam tip: match transport to existing fabric type. |
| 10 | Name common parallel filesystems used for AI training storage. | Lustre, GPFS/Spectrum Scale, WEKA, VAST — provide high-throughput parallel access for multi-node GPU training checkpoints. Exam tip: single-node NFS doesn't scale for this workload. |
| 11 | When does the exam favor InfiniBand over RoCEv2 Ethernet? | Large-scale GPU training clusters needing lowest latency, native adaptive routing, and SHARP in-network compute. RoCEv2 favored for Ethernet compatibility and easier multi-tenant DC integration. Exam keyword: 'lowest possible latency at massive scale' = InfiniBand. |
| 12 | Current InfiniBand generations NDR and XDR per-port speeds? | NDR = 400Gb/s per port, XDR = 800Gb/s per port. Exam tip: be able to match generation name to throughput tier. |
