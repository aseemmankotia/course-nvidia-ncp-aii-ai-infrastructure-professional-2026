## Chapter 8: Kubernetes and the NVIDIA GPU Operator — Practice Questions

**Q1.** A platform team runs a shared Kubernetes cluster across multiple ML teams. Each tenant must receive a guaranteed hardware-isolated slice of a GPU, including dedicated memory and compute cores, so one tenant's workload cannot degrade another's performance or crash its memory space. Which approach BEST meets this requirement?

A) Configure GPU time-slicing in the device plugin ConfigMap
B) Set nvidia.com/gpu resource limits without any partitioning
C) Partition the GPU using Multi-Instance GPU (MIG)
D) Use node affinity rules to co-locate tenant pods

<details>
<summary>Answer</summary>

**Correct: C**

MIG physically partitions an A100/H100 GPU into isolated instances with dedicated memory, cache, and compute cores, giving true hardware-level fault and performance isolation.

**Why the others are wrong:**
- Time-slicing only divides SM time; there is no memory isolation between processes sharing the slice
- Whole-GPU limits allocate the entire device to one pod — no partitioning occurs at all, so multiple tenants can't share
- Node affinity only influences scheduling placement, it provides no isolation of GPU resources

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** A pod requesting `nvidia.com/gpu: 1` remains stuck in Pending state with the event 'Insufficient nvidia.com/gpu', even though the node has a physical GPU and the NVIDIA driver container is running successfully. What is the most likely cause?

A) The DCGM Exporter DaemonSet is not deployed
B) The NVIDIA Container Toolkit is missing from the node
C) Node Feature Discovery labels are outdated
D) The NVIDIA Device Plugin DaemonSet is not running or hasn't registered the resource on that node

<details>
<summary>Answer</summary>

**Correct: D**

The Device Plugin is what advertises nvidia.com/gpu as an allocatable extended resource to kubelet; without it registered, the scheduler sees zero allocatable GPUs and reports Insufficient nvidia.com/gpu at the scheduling phase.

**Why the others are wrong:**
- DCGM Exporter only publishes metrics for monitoring — it plays no role in resource advertisement or scheduling
- A missing Container Toolkit would allow scheduling to succeed but cause a CreateContainerError at runtime, not a Pending/Insufficient scheduling failure
- NFD labels affect feature-discovery labels used for node selection, not the nvidia.com/gpu allocatable count itself

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A company wants to guarantee that non-GPU workloads (e.g., web frontends) never get scheduled onto expensive GPU nodes, while GPU workloads can still land there normally. Which combination correctly enforces this?

A) Apply a ResourceQuota on the namespace only
B) Taint GPU nodes (e.g., nvidia.com/gpu=present:NoSchedule) and add a matching toleration plus nodeSelector/label to GPU workload pod specs
C) Label GPU nodes only; scheduling will respect labels automatically
D) Configure a PodDisruptionBudget on the GPU node pool

<details>
<summary>Answer</summary>

**Correct: B**

Taints repel any pod lacking a matching toleration, keeping non-GPU pods off the node; adding a toleration plus nodeSelector/label on GPU pods ensures they are the only workloads placed there.

**Why the others are wrong:**
- ResourceQuota limits consumption per namespace but has no effect on node placement
- Labels alone are informational — pods must actively select them via nodeSelector/affinity; nothing prevents unrelated pods from still landing there
- PodDisruptionBudget governs voluntary eviction during disruptions, unrelated to node placement

</details>

---

**Q4.** Which GPU Operator component exposes GPU utilization, memory usage, temperature, and power metrics for Prometheus scraping?

A) NVIDIA Container Toolkit
B) DCGM Exporter
C) NVIDIA Device Plugin
D) Node Feature Discovery

<details>
<summary>Answer</summary>

**Correct: B**

DCGM (Data Center GPU Manager) Exporter surfaces per-GPU telemetry as Prometheus metrics.

**Why the others are wrong:**
- Container Toolkit only handles runtime driver/library injection, not metrics
- Device Plugin advertises schedulable GPU resources to kubelet, it does not expose telemetry
- NFD only discovers and labels hardware/software features on nodes

</details>

---

**Q5.** Which GPU Operator component makes the host's NVIDIA driver and CUDA libraries available inside a container at runtime, without requiring the container image itself to bundle GPU drivers?

A) NVIDIA Device Plugin
B) DCGM Exporter
C) MIG Manager
D) NVIDIA Container Toolkit

<details>
<summary>Answer</summary>

**Correct: D**

The Container Toolkit hooks into the container runtime (containerd/CRI-O) to mount the host driver and CUDA libraries into containers at launch.

**Why the others are wrong:**
- Device Plugin only advertises and allocates the nvidia.com/gpu resource, it doesn't inject libraries
- DCGM Exporter is purely a metrics/telemetry component
- MIG Manager configures MIG partitions/profiles, unrelated to library injection

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** On an A100 40GB GPU configured in MIG mode using the 3g.20gb profile, how many instances of that profile can coexist simultaneously on the same physical GPU?

A) 7
B) 4
C) 1
D) 2

<details>
<summary>Answer</summary>

**Correct: D**

A100 has 7 total GPU compute slices; the 3g.20gb profile consumes 3 slices and half the memory, allowing exactly 2 instances to coexist (using 6 of 7 slices, full 40GB memory).

**Why the others are wrong:**
- 7 is the max count only for the smallest 1g.5gb profile
- 4 exceeds available compute slices when using 3-slice profiles (would require 12 slices)
- 1 undercounts — two 3g.20gb instances is the documented valid combination for this profile

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** In a Kubernetes pod spec requesting a GPU via nvidia.com/gpu, which configuration rule does the NVIDIA device plugin's scheduling model require?

A) requests and limits for nvidia.com/gpu must be set to the same value
B) requests.nvidia.com/gpu must be less than limits.nvidia.com/gpu
C) Only requests need to be specified; limits are optional
D) Fractional values like 0.5 are allowed for nvidia.com/gpu without MIG or time-slicing configured

<details>
<summary>Answer</summary>

**Correct: A**

GPUs are treated as non-compressible extended resources — Kubernetes requires requests to equal limits (or you specify only limits and Kubernetes sets requests equal automatically).

**Why the others are wrong:**
- Kubernetes does not allow requests to differ from limits for extended resources like nvidia.com/gpu
- While specifying only limits is valid syntax, it still forces requests=limits — the option's framing that limits are entirely optional in general use is misleading in context of exam wording
- Extended resources must be whole integers by default; fractional GPU sharing requires explicit MIG or time-slicing configuration

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** What is the primary advantage of deploying the NVIDIA GPU Operator instead of manually installing the NVIDIA driver on each Kubernetes node?

A) It eliminates the need for a container runtime on GPU nodes
B) It removes the requirement for Kubernetes RBAC configuration
C) It replaces kube-scheduler entirely with a GPU-aware scheduler
D) It automates driver installation, lifecycle management, and validation consistently across all GPU nodes using Kubernetes-native components

<details>
<summary>Answer</summary>

**Correct: D**

GPU Operator packages the driver, toolkit, device plugin, and monitoring as managed Kubernetes DaemonSets/Deployments, automating install and upgrades cluster-wide instead of manual per-node OS package management.

**Why the others are wrong:**
- A container runtime (containerd/CRI-O) is still required regardless of the Operator
- RBAC configuration is still needed for the Operator's own service accounts and permissions
- The Operator extends resource scheduling via device plugin/extended resources but does not replace kube-scheduler

</details>

---

