## Chapter 1: DGX H100/H200 Hardware and Initial Power-On — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | DGX H100: number of GPUs and per-GPU HBM3 memory? | 8x H100 SXM5 GPUs, 80GB HBM3 each = 640GB total GPU memory. Exam tip: compare against DGX H200's 141GB per GPU. |
| 2 | DGX H200: per-GPU memory capacity and type? | 141GB HBM3e per GPU = 1128GB total across 8 GPUs. Exam tip: H200's key differentiator vs H100 is memory capacity/bandwidth, not NVLink generation. |
| 3 | How many NVSwitches are on a DGX H100/H200 GPU baseboard? | 4 NVSwitches (3rd/4th gen NVSwitch) interconnecting all 8 GPUs for full intra-node all-to-all communication. |
| 4 | Per-GPU NVLink bandwidth on DGX H100/H200? | 900GB/s bidirectional per GPU via NVLink4. Exam tip: total system bisection bandwidth = 8 x 900GB/s = 7.2TB/s. |
| 5 | DGX H100 PSU configuration and redundancy level? | 6x 3300W PSUs in N+2 redundancy (4 needed for full load, 2 spare). Up to 2 PSUs can fail without power loss. |
| 6 | DGX H100 rated max system power draw? | ~10.2kW. Exam tip: use this number for rack power budgeting, NOT the sum of installed PSU wattages. |
| 7 | How do you access DGX H100 health status before the OS boots? | Via the BMC's dedicated management port using Redfish or IPMI — fully out-of-band, independent of host OS state. |
| 8 | What in-OS command shows GPU status after boot, and why can't it be used pre-boot? | nvidia-smi shows GPU status but requires the NVIDIA driver stack running under a booted OS — unavailable during pre-boot health checks, unlike BMC/Redfish. |
| 9 | Can you mix DGX H100 and DGX H200 nodes in the same NCCL collective job? | Technically possible (same NVLink/NVSwitch generation) but causes performance imbalance and requires driver/firmware compatibility matrix validation across nodes. |
| 10 | DGX H100/H200 CPU configuration? | Dual Intel Xeon Platinum 8480C CPUs. Exam tip: same CPU family used across both H100 and H200 chassis. |
| 11 | Total NVLink bisection bandwidth for a full DGX H100/H200 node? | 7.2TB/s (8 GPUs x 900GB/s per-GPU NVLink bandwidth), enabled by the 4-NVSwitch baseboard fabric. |
| 12 | What is the correct pre-boot bring-up sequence for a DGX H100 node? | 1) Power-on/POST, 2) BMC-reported component health check (PSU, GPU baseboard, NVSwitch), 3) Firmware/compatibility validation, 4) OS boot. Exam tip: health checks happen BEFORE OS boot via BMC, not after. |
