## Chapter 8 Cheat Sheet: Kubernetes & NVIDIA GPU Operator

### GPU Operator Core Components
| Component | Purpose | Exam Note |
|---|---|---|
| NVIDIA Driver (containerized) | Runs driver as a DaemonSet on GPU nodes | No host OS driver install needed |
| Container Toolkit | Injects driver/CUDA libs into containers at runtime | Hooks containerd/CRI-O |
| Device Plugin | Advertises `nvidia.com/gpu` as allocatable resource to kubelet | Missing = 'Insufficient nvidia.com/gpu' Pending errors |
| DCGM Exporter | Exposes GPU metrics (util, mem, temp, power) for Prometheus | Monitoring only, no scheduling role |
| Node Feature Discovery (NFD) + GPU Feature Discovery (GFD) | Labels nodes with GPU model/MIG capability | Drives scheduling and MIG config decisions |
| MIG Manager | Applies/reconfigures MIG profiles on supported GPUs | Requires node reboot/drain in some configs |

### Kubernetes GPU Resource Rules
- Request via `resources.limits.nvidia.com/gpu: N` (integers only by default).
- `requests` MUST equal `limits` — non-overcompressible, non-overcommitted resource.
- Fractional GPU sharing only via MIG (hardware partition) or time-slicing (software, ConfigMap-based, no isolation).

### MIG Profiles (A100 40GB example — 7 compute slices total)
| Profile | Max Instances | Memory per Instance |
|---|---|---|
| 1g.5gb | 7 | 5GB |
| 2g.10gb | 3 | 10GB |
| 3g.20gb | 2 | 20GB |
| 4g.20gb | 1 | 20GB |
| 7g.40gb | 1 | 40GB (full GPU) |

### MIG vs Time-Slicing
| Feature | MIG | Time-Slicing |
|---|---|---|
| Isolation | Hardware (memory, cache, compute) | None — shared SM context switching |
| Fault containment | Yes | No |
| Supported hardware | A100/A30/H100-class only | Any CUDA GPU |
| Use case | Strict multi-tenant isolation | Best-effort dev/test sharing |

### Node Scheduling for GPU Workloads
- Taint GPU nodes: `nvidia.com/gpu=present:NoSchedule`
- GPU pods add: matching `toleration` + `nodeSelector`/label (e.g., `nvidia.com/gpu.product`)
- Prevents non-GPU pods from occupying expensive GPU nodes.

### Multi-Tenancy via Namespaces
- `ResourceQuota` on `requests.nvidia.com/gpu` / `limits.nvidia.com/gpu` per namespace.
- `LimitRange` sets default request/limit if pod omits them.

### Useful Commands
```bash
kubectl describe node <node> | grep -A5 Allocatable   # check GPU count exposed
kubectl get pods -n gpu-operator                       # verify operator components running
kubectl logs -n gpu-operator -l app=nvidia-device-plugin-daemonset
```

### GPU Operator vs Manual Install
| | GPU Operator | Manual Install |
|---|---|---|
| Driver lifecycle | Automated, containerized, cluster-wide | Per-node OS packages, manual upgrades |
| Consistency | Enforced across all nodes | Prone to version drift |
| Rollback | Managed via operator config | Manual per-node |
