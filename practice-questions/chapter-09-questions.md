## Chapter 9: Slurm Scheduling and Training-vs-Inference Resource Planning — Practice Questions

**Q1.** A research lab runs a 512-GPU distributed pretraining job requiring gang scheduling (all-or-nothing allocation), tight MPI synchronization across nodes, and checkpoint/restart on preemption. Which scheduler is the BEST fit for this workload?

A) Kubernetes with the default scheduler and HPA
B) Slurm with exclusive node allocation and a dedicated training partition
C) Kubernetes with KEDA-based event autoscaling
D) Slurm configured only with a single shared debug partition

<details>
<summary>Answer</summary>

**Correct: B**

Slurm natively supports gang scheduling, exclusive node allocation, backfill, and checkpoint/requeue semantics that tightly-coupled MPI training jobs require; a dedicated training partition isolates these long-running batch jobs from other workloads.

**Why the others are wrong:**
- Kubernetes' default scheduler and HPA are built for scaling independent stateless replicas, not gang-scheduled MPI ranks that must start simultaneously.
- KEDA scales pods based on event/metric triggers for services, not for coordinated all-or-nothing HPC job launches.
- A single shared debug partition lacks resource isolation and priority/QOS controls needed for large production training runs.

</details>

---

**Q2.** An inference platform team must serve a real-time recommendation model where traffic spikes 10x during peak hours and drops to near-zero overnight. Which combination is the BEST approach?

A) Slurm with a high-priority QOS and manual salloc during peak hours
B) A single static Slurm partition sized for peak load, always running
C) Kubernetes with no autoscaler, using a fixed replica count sized for average traffic
D) Kubernetes with an inference-serving framework (e.g., Triton/KServe) behind a Horizontal Pod Autoscaler or KEDA

<details>
<summary>Answer</summary>

**Correct: D**

Kubernetes with HPA/KEDA driven by request rate, queue depth, or GPU utilization metrics dynamically scales inference replicas to match bursty traffic, minimizing idle GPU cost while meeting latency SLAs.

**Why the others are wrong:**
- Slurm is designed for batch/HPC job scheduling, not continuous low-latency request serving with elastic scale-out/in.
- A statically sized partition for peak load wastes GPU spend during off-peak hours and doesn't align with cost-efficient inference planning.
- Fixed replica counts sized for average traffic will violate latency SLAs during 10x spikes.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** A cluster admin wants to run both large-scale training jobs (Slurm) and microservice-based inference (Kubernetes) on the same physical GPU nodes without maintaining two separate hardware pools. What is the recommended approach for this coexistence?

A) Manually reboot nodes between Slurm and Kubernetes images each time workload type changes
B) Permanently partition the cluster in half, with no ability to reassign nodes
C) Use a hybrid orchestration layer (e.g., SchedMD's Slinky or NVIDIA Run:ai) that lets Slurm workloads run as Kubernetes-managed jobs on shared nodes
D) Run Kubernetes only, and simulate Slurm behavior using Kubernetes CronJobs

<details>
<summary>Answer</summary>

**Correct: C**

Hybrid schedulers such as Slinky (Slurm-on-Kubernetes) or Run:ai integrate Slurm's HPC scheduling semantics with Kubernetes' resource management, allowing dynamic node sharing/reassignment between training and inference without separate hardware silos.

**Why the others are wrong:**
- Manual rebooting between images is operationally infeasible and causes downtime, defeating the purpose of shared infrastructure.
- Permanent 50/50 partitioning removes the flexibility needed for mixed, fluctuating training vs inference demand.
- CronJobs cannot replicate Slurm's gang scheduling, backfill, or MPI job semantics required for large training runs.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which scheduler is generally preferred for tightly-coupled, fixed-duration batch AI training jobs on HPC-style clusters?

A) Kubernetes
B) Docker Swarm
C) A load balancer with round-robin routing
D) Slurm

<details>
<summary>Answer</summary>

**Correct: D**

Slurm is purpose-built for HPC batch scheduling: it provides gang scheduling, backfill, exclusive allocation, and job accounting suited to large synchronized training runs.

**Why the others are wrong:**
- Kubernetes excels at long-running, independently scalable services rather than gang-scheduled batch jobs.
- Docker Swarm lacks HPC-grade scheduling features like backfill and gang scheduling.
- A load balancer routes traffic to services; it is not a job scheduler.

</details>

---

**Q5.** In Slurm, which command submits a batch script requesting 8 GPUs on a single node with exclusive access?

A) kubectl apply -f job.yaml --gpus=8
B) srun --nodes=8 --replicas=1 train.sh
C) sbatch --gres=gpu:8 --exclusive train.sh
D) docker run --gpus 8 train.sh

<details>
<summary>Answer</summary>

**Correct: C**

sbatch submits a batch job; --gres=gpu:8 requests 8 generic GPU resources on the node, and --exclusive ensures the job does not share the node with other jobs.

**Why the others are wrong:**
- kubectl is a Kubernetes command, not part of Slurm; it also doesn't use a '--gpus' flag on 'apply'.
- srun's --nodes flag specifies node count, not GPU count per node, and there is no '--replicas' flag in Slurm.
- docker run is a container runtime command unrelated to Slurm scheduling.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q6.** A cluster admin creates two Slurm partitions: 'train' with a 7-day max walltime and lower priority preemption rules, and 'infer' with a 1-hour max walltime and highest QOS priority. What is the PRIMARY reason for this partition split?

A) To physically separate the network switches used by each workload type
B) To reduce the total number of GPUs required in the cluster
C) To enforce different scheduling policies (walltime limits, priority, preemption) appropriate to long training jobs versus short, latency-sensitive inference jobs
D) Because Slurm requires a minimum of two partitions to function

<details>
<summary>Answer</summary>

**Correct: C**

Partitions in Slurm let admins apply distinct scheduling policies (max time, QOS, preemption) so long-running training doesn't block or delay short, time-sensitive inference/interactive jobs and vice versa.

**Why the others are wrong:**
- Partitions are a logical/scheduling construct in Slurm config, not a network topology definition.
- Partitioning doesn't change total GPU count; it changes how existing GPUs are scheduled and prioritized.
- Slurm can run with a single default partition; multiple partitions are optional, used for policy differentiation.

</details>

---

**Q7.** For a mixed-workload cluster planning exercise, which factor is MOST important when sizing GPU memory allocation for inference pods versus training jobs?

A) Inference typically needs large batch sizes for high throughput, so allocate the same GPU memory as training
B) GPU memory sizing is identical between training and inference and requires no separate planning
C) Inference workloads often use smaller memory footprints per request (single or small-batch forward pass) and can leverage MIG or time-slicing, while training requires full-GPU memory for activations, gradients, and optimizer states
D) Only CPU memory matters for both training and inference planning

<details>
<summary>Answer</summary>

**Correct: C**

Training needs memory for weights, activations, gradients, and optimizer states (often requiring full GPUs or multi-GPU sharding), while inference forward passes have a much smaller memory footprint, making MIG partitioning or time-slicing viable for higher GPU utilization density.

**Why the others are wrong:**
- Large batch sizes for throughput don't require the same memory profile as training's gradient/optimizer state overhead.
- Memory sizing differs significantly between training and inference; treating them identically leads to wasted or insufficient GPU allocation.
- GPU memory, not just CPU memory, is the primary constraint for both training and inference planning.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A company needs to run distributed training on a fixed set of on-prem GPU nodes for 6-hour job windows, then repurpose the same nodes for autoscaled inference the rest of the day. Which architecture BEST meets these requirements?

A) Two permanently separate clusters, one Slurm-only and one Kubernetes-only, with no node sharing
B) A hybrid setup using a scheduler bridge (e.g., Slinky) so Slurm training jobs and Kubernetes inference services can time-share the same node pool based on schedule
C) Run only Kubernetes and simulate training with long-running Jobs and no gang scheduling
D) Run only Slurm and use cron-based scripts to serve inference requests

<details>
<summary>Answer</summary>

**Correct: B**

A hybrid bridge (Slinky-style) lets the same physical node pool be dynamically allocated to Slurm-managed training during defined windows and to Kubernetes-managed inference otherwise, maximizing utilization without duplicating hardware.

**Why the others are wrong:**
- Two fully separate clusters double hardware costs and prevent the required node repurposing between training and inference windows.
- Kubernetes Jobs lack Slurm's gang scheduling and backfill needed for efficient tightly-coupled distributed training.
- Serving low-latency inference via cron scripts on Slurm lacks autoscaling, load balancing, and health-check features needed for production inference.

</details>

---

