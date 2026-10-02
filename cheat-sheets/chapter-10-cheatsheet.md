## Chapter 10 Cheat Sheet: GPU / NVLink / Network Failure Diagnosis

### Node Triage Sequence (bottom-up, ALWAYS in this order)
| Order | Layer | Tool/Log |
|---|---|---|
| 1 | Hardware (pre-boot) | BMC/IPMI SEL (`ipmitool sel list`, Redfish) |
| 2 | Kernel/OS | `dmesg -T` (Xid codes, PCIe/link errors) |
| 3 | Driver/GPU | `nvidia-smi`, DCGM |
| 4 | Application | Job/framework logs (NCCL, PyTorch) |

### DCGM Diagnostic Levels
| Level | Duration | Scope |
|---|---|---|
| `dcgmi diag -r 1` | Seconds | Quick health check |
| `dcgmi diag -r 2` | ~2 min | Medium diagnostic |
| `dcgmi diag -r 3` | ~12+ min | Extended stress: memory bandwidth, NVLink, thermal |

Use r3 before returning a suspect node to production.

### Key Commands
| Command | Purpose |
|---|---|
| `nvidia-smi -q -d ECC` | Correctable/uncorrectable ECC error counts |
| `nvidia-smi -q -d ROW_REMAPPER` | Pending/completed row remaps (memory degradation) |
| `nvidia-smi nvlink -s` | NVLink link status (Active/Inactive) |
| `nvidia-smi nvlink -e` | NVLink CRC/replay error counters |
| `dcgmi health -c` | GPU subsystem health check |
| `dcgmi discovery -l` | List discovered GPUs/entities |
| `nvidia-bug-report.sh` | Full diagnostic bundle for support cases |
| `ibstat` / `ibstatus` | InfiniBand port/link state |
| `perfquery` | IB port counters (symbol errors) — single port |
| `ibdiagnet` | Fabric-wide IB diagnostic scan |
| `ibnetdiscover` | Discover IB fabric topology |
| `rdma link show` | RoCE link state |
| `ipmitool sel list` | BMC hardware event log |

### Common Xid Codes (dmesg/syslog)
| Xid | Meaning | Likely Cause |
|---|---|---|
| 13 | Graphics engine exception | Driver/app bug |
| 48 | Double-bit ECC error (uncorrectable) | Failing memory — check ROW_REMAPPER |
| 62 | Internal micro-controller halt | Firmware/driver issue |
| 79 | GPU has fallen off the bus | PCIe/power fault OR driver/VBIOS firmware mismatch |

### Firmware/Driver Mismatch — Root Cause Pattern
- Intermittent Xid errors starting right after a driver update
- No corresponding VBIOS/firmware update applied
- Exam pattern: always check driver-firmware version alignment before assuming hardware failure

### NVSwitch Systems (DGX/HGX)
- Requires `nvidia-fabricmanager` service running for NVLink fabric initialization
- FM crash/failure = NVLink fabric fails to come up — check FM logs first

### Exam Notes
- BMC/IPMI logs are checked FIRST — they don't require OS or driver.
- DCGM r1 passing does NOT rule out link-level faults — escalate to r3 if `nvlink -s` shows Inactive.
- perfquery = single IB port; ibdiagnet = whole fabric.
