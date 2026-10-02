# Ch.11 Cheat Sheet: Benchmarking, Tuning, Monitoring

## NCCL Benchmarking
| Metric | Meaning |
|---|---|
| algbw | msg size / time |
| busbw | algbw × collective factor (all-reduce: 2*(n-1)/n) — compare to peak link BW |

**Key env vars:** `NCCL_DEBUG=INFO`, `NCCL_IB_HCA`, `NCCL_ALGO` (Ring/Tree), `NCCL_PROTO` (Simple/LL/LL128), `NCCL_SOCKET_IFNAME`

**Common tool:** `all_reduce_perf -b 8 -e 128M -f 2 -g <ngpus>`

## DCGM Diagnostics (`dcgmi diag -r`)
| Level | Name | Duration |
|---|---|---|
| 1 | Quick | seconds |
| 2 | Medium | ~2 min |
| 3 | Long | several min (stress/NVLink/mem) |
| 4 | Extended | 1+ hour |

**Pre-job validation → use -r 3**

## Key DCGM Fields
| Field | Purpose |
|---|---|
| DCGM_FI_DEV_GPU_TEMP | Temperature |
| DCGM_FI_DEV_THERMAL_VIOLATION | Thermal throttle counter |
| DCGM_FI_DEV_POWER_VIOLATION | Power-cap throttle counter |
| DCGM_FI_DEV_POWER_USAGE | Watts drawn |
| DCGM_FI_DEV_GPU_UTIL | Compute utilization |
| DCGM_FI_DEV_SM_CLOCK | SM clock (drops = throttle) |
| DCGM_FI_DEV_XID_ERRORS | HW/driver fault codes |

**Diagnosis rule:** THERMAL_VIOLATION↑ & POWER_VIOLATION=0 → cooling issue. POWER_VIOLATION↑ → raise/adjust power cap.

## Power & Thermal
- H100 SXM slowdown threshold ≈ 83°C
- H100 SXM TDP up to 700W
- Set power cap: `nvidia-smi -i <id> -pl <watts>`
- Persistence mode: `nvidia-smi -pm 1`

## Fabric Tuning
- Rail-optimized topology: 1 dedicated NIC per GPU; verify with `nvidia-smi topo -m`
- Congestion control: ECN + Adaptive Routing (preferred) vs PFC (can cause HOL blocking)
- SHARP: in-network all-reduce offload/aggregation, reduces host-side traffic
- NVLink 4 per-GPU aggregate: ~900 GB/s (H100 SXM)
- IB NDR port bandwidth: 400 Gb/s

## Bottleneck Isolation Workflow
1. Check DCGM GPU_UTIL — near 100%? compute likely not the limiter.
2. Run nccl-tests, compare busbw to theoretical fabric peak.
3. Check NCCL_DEBUG=INFO for rail/HCA discovery issues (partial rail count = misconfig).
4. Check THERMAL_VIOLATION / POWER_VIOLATION to rule out throttling before blaming network.
5. Validate GPU-NIC PCIe/NUMA affinity via `nvidia-smi topo -m`.

## Common Exam Traps
- Confusing algbw with busbw.
- Blaming power cap when POWER_VIOLATION=0 (it's thermal).
- Assuming high GPU_UTIL rules out network bottlenecks (util can be high while waiting on NCCL comm kernels).
- Forgetting NCCL_IB_HCA filtering as cause of missing rails.
