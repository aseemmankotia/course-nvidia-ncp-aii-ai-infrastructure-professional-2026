## Chapter 2 Cheat Sheet: Firmware, BIOS, Driver, CUDA Stack Validation

### Bring-Up Order (memorize exactly)
| Step | Action | Why |
|---|---|---|
| 1 | Update system firmware/BIOS | Sets PCIe/NVLink topology & platform config |
| 2 | Reboot | Applies firmware changes |
| 3 | Install GPU kernel driver | Loads kernel module driver depends on |
| 4 | Install CUDA toolkit | Requires matching driver already loaded |

**Exam trap:** CUDA-before-driver or driver-before-firmware ordering is always wrong.

### Driver–CUDA Compatibility
| CUDA Version | Min Linux Driver |
|---|---|
| 12.0 | 525.60.13 |

- Minor version compatibility: one driver supports its CUDA major version and later minor releases.
- `nvidia-smi` header shows **max CUDA driver supports** ≠ installed toolkit version (`nvcc --version`).

### nvidia-smi topo -m Symbols
| Symbol | Meaning |
|---|---|
| NV# | # direct NVLink connections |
| PIX | Single PCIe switch hop |
| PXB | Multiple PCIe switches, no host bridge |
| PHB | Crosses PCIe Host Bridge (no NVLink) |
| NODE | Crosses NUMA node boundary |
| SYS | Crosses CPU/socket boundary (slowest) |

**Exam trap:** PHB/SYS between GPUs that should be NVLink-connected = topology problem, not a health/firmware failure.

### DCGM Diagnostics (`dcgmi diag`)
| Run Level | Duration | Use Case |
|---|---|---|
| -r 1 | Seconds | Quick sanity check |
| -r 2 | ~2 min | Medium validation |
| -r 3 | 15+ min | Extended stress test — pre-production node acceptance |

### Key Commands
```bash
nvidia-smi --query-gpu=driver_version --format=csv
nvidia-smi topo -m
dcgmi diag -r 3
nvcc --version
systemctl status nvidia-fabricmanager
```

### NVSwitch / Fabric Manager
- Required on HGX/NVSwitch systems for actual GPU-to-GPU NVLink traffic.
- Correct `topo -m` output (NV#) does **not** guarantee working NVLink if Fabric Manager service isn't running.

### Multi-Node Firmware Bring-Up
- Validate identical firmware bundle version on ALL nodes **before** driver install.
- Confirm via per-node version report — inconsistent firmware causes inconsistent NVLink/PCIe behavior even with matching CUDA versions.
