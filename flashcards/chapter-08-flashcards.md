## Chapter 8: Kubernetes and the NVIDIA GPU Operator — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Which GPU Operator component exposes GPU metrics for Prometheus scraping? | DCGM Exporter — exposes utilization, memory, temperature, and power via a /metrics endpoint. Exam tip: don't confuse it with the Device Plugin, which advertises schedulable resources, not metrics. |
| 2 | Which component advertises the nvidia.com/gpu resource to kubelet? | NVIDIA Device Plugin (DaemonSet). If it's not running/registered, the scheduler reports 'Insufficient nvidia.com/gpu' even though hardware is present. |
| 3 | Which component injects the host NVIDIA driver/CUDA libraries into containers at runtime? | NVIDIA Container Toolkit — hooks into containerd/CRI-O to mount driver libraries. Exam tip: container images don't need drivers baked in. |
| 4 | Max number of MIG compute slices on an A100 GPU? | 7 slices total. Profiles: 1g.5gb (x7), 2g.10gb (x3), 3g.20gb (x2), 4g.20gb (x1), 7g.40gb (x1). |
| 5 | Can nvidia.com/gpu resource requests be fractional (e.g., 0.5) by default? | No. Extended resources must be whole integers. Fractional sharing requires MIG or time-slicing configured via the device plugin ConfigMap. |
| 6 | Kubernetes rule for GPU pod requests vs limits? | requests.nvidia.com/gpu must equal limits.nvidia.com/gpu — GPUs are non-overcommittable, non-compressible resources. |
| 7 | MIG vs time-slicing — which provides hardware-level memory/fault isolation? | MIG. Time-slicing shares SMs via context switching with no memory isolation — a memory leak or crash in one workload can impact co-scheduled workloads. |
| 8 | Command to check GPU allocatable resources on a node? | kubectl describe node <node> \| grep -A5 Allocatable — look for nvidia.com/gpu count. Exam tip: 0 means the device plugin isn't running/registered. |
| 9 | Typical taint used to reserve nodes for GPU workloads? | nvidia.com/gpu=present:NoSchedule (convention, not automatic). GPU pods must add a matching toleration plus nodeSelector/label to land there. |
| 10 | GPU Operator vs manual driver install — key exam differentiator? | GPU Operator manages the driver as a containerized DaemonSet with automated lifecycle/version management cluster-wide; manual install requires per-node OS package management and manual upgrades. |
| 11 | Which sub-components discover node GPU hardware/MIG capability and apply labels for scheduling? | Node Feature Discovery (NFD) + GPU Feature Discovery (GFD). They produce labels like nvidia.com/gpu.product used by scheduling and MIG configuration. |
| 12 | How is GPU consumption limited per team in namespace-based multi-tenancy? | Apply a ResourceQuota on requests.nvidia.com/gpu and limits.nvidia.com/gpu per namespace, combined with a LimitRange to set default request/limit values. |
