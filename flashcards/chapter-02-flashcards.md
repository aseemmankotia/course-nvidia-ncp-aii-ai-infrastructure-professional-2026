## Chapter 2: Firmware, BIOS, Driver, and CUDA Stack Validation — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Correct GPU server bring-up order? | 1) Update BIOS/firmware, 2) Reboot, 3) Install kernel GPU driver, 4) Install CUDA toolkit. Exam tip: CUDA-before-driver ordering is a common wrong-answer trap. |
| 2 | nvidia-smi topo -m: what does 'NV4' mean between two GPUs? | 4 direct NVLink connections between the GPU pair. NV# = link count, not generation. Exam tip: higher NV# = more NVLink bandwidth between that pair. |
| 3 | nvidia-smi topo -m: meaning of PHB vs PIX vs SYS? | PHB = path through a PCIe Host Bridge (no NVLink); PIX = single PCIe switch hop; SYS = crosses CPU/NUMA/socket boundary (slowest path). Exam tip: SYS connections are the least performant. |
| 4 | Minimum NVIDIA driver version required for CUDA 12.0 on Linux? | 525.60.13. Exam tip: always check the CUDA Toolkit Release Notes compatibility matrix before upgrading either component. |
| 5 | dcgmi diag run levels 1-3 — what do they mean? | -r 1 = quick (seconds), -r 2 = medium (~2 min), -r 3 = long/extended (~15+ min, includes stress test). Exam tip: use -r 3 for pre-production node acceptance testing. |
| 6 | What service must be running for GPU-to-GPU NVLink traffic on NVSwitch (HGX) systems? | NVIDIA Fabric Manager. Exam tip: correct topo -m output does NOT guarantee working NVLink if Fabric Manager isn't running. |
| 7 | Command to check installed NVIDIA driver version? | nvidia-smi --query-gpu=driver_version --format=csv. Exam tip: the nvidia-smi header also shows the MAX CUDA version the driver supports, not the installed toolkit version. |
| 8 | Difference between the CUDA version shown in the nvidia-smi header vs. nvcc --version? | nvidia-smi header shows the max CUDA version the DRIVER supports; nvcc --version shows the installed CUDA TOOLKIT version. They can legitimately differ. Exam tip: this mismatch is a common exam trap question. |
| 9 | nvidia-smi vs DCGM — primary use case difference? | nvidia-smi = lightweight per-GPU status/monitoring/config. DCGM = deep diagnostics, health checks, and cluster-scale GPU management. Exam tip: dcgmi diag is the correct tool for pre-production validation, not nvidia-smi. |
| 10 | Why must BIOS/firmware be updated BEFORE driver installation? | Firmware defines PCIe/NVLink topology and platform configuration the driver probes at load time. Exam tip: installing the driver first can lock in stale topology info, requiring a reinstall after firmware update. |
| 11 | Multi-node firmware validation best practice before cluster bring-up? | Apply and validate an identical firmware bundle version across ALL nodes before driver install, confirmed via per-node version report. Exam tip: mismatched firmware causes inconsistent NVLink/PCIe behavior even with identical CUDA versions. |
| 12 | What does a DCGM diag PASS result confirm for a new node? | GPU health across memory, NVLink, thermal, and stress categories. Exam tip: a node should show DCGM diag PASS before being accepted into a production cluster. |
