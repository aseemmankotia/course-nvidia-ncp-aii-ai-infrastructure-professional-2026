## DGX H100/H200 Hardware & Power-On Cheat Sheet

### GPU & Memory Comparison
| System | GPUs | Memory/GPU | Total GPU Memory | GPU Type |
|---|---|---|---|---|
| DGX H100 | 8x H100 SXM5 | 80GB HBM3 | 640GB | H100 |
| DGX H200 | 8x H200 SXM5 | 141GB HBM3e | 1128GB | H200 |

Both share the same NVLink4/NVSwitch fabric generation — mixing models in one job is possible but requires compatibility validation.

### Intra-Node Fabric
| Item | Spec |
|---|---|
| NVSwitches per baseboard | 4 |
| NVLink bandwidth/GPU | 900GB/s bidirectional |
| Total node bisection bandwidth | 7.2TB/s (8 x 900GB/s) |
| Fabric type | NVLink4 all-to-all via NVSwitch |

### Power & Redundancy
| Item | Spec |
|---|---|
| PSU count | 6x 3300W |
| Redundancy | N+2 (4 required, 2 spare) |
| PSUs that can fail w/o impact | 2 |
| Rated max system power draw | ~10.2kW |

**Rack budgeting rule:** Use rated max power draw per node (10.2kW), never sum raw PSU wattage (6x3300W=19.8kW) — that overestimates actual load.

### CPU / Compute
| Item | Spec |
|---|---|
| CPU | Dual Intel Xeon Platinum 8480C (same across H100/H200) |

### BMC / Pre-Boot Health Checks
| Step | Interface | Notes |
|---|---|---|
| Power-on/POST | BMC | Independent of host OS |
| Component health check | BMC via Redfish/IPMI | PSU, GPU baseboard, NVSwitch status |
| Network access | Dedicated BMC management port | Own static IP, separate from host NIC |
| OS boot | Host OS | Only after BMC-reported health passes |

**Key rule:** nvidia-smi, dmesg, and systemd journal all require a booted OS — they CANNOT be used for pre-boot health verification. Only BMC/Redfish/IPMI works out-of-band.

### Compatibility Matrix Checklist (Cluster Bring-Up)
- GPU model consistency per NVLink domain (H100 vs H200)
- Firmware/driver version parity across nodes
- NVSwitch/NVLink generation match
- PSU redundancy configuration consistency across rack
- BMC firmware version alignment for fleet management tooling

### Common Exam Traps
- Confusing PSU total wattage with rated system power draw
- Assuming nvidia-smi works before OS boot (it does not — use BMC)
- Assuming H100/H200 mixing is fully forbidden (it's allowed but requires validation, not blocked outright)
- Miscounting N+2 redundancy tolerance (2 failures survivable, not 1 or 3)
