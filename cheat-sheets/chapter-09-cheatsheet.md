## Chapter 9 Cheat Sheet: Slurm Scheduling & Training vs Inference Planning

### Slurm vs Kubernetes Decision Table
| Criterion | Slurm | Kubernetes |
|---|---|---|
| Workload type | Batch, HPC, tightly-coupled MPI | Long-running services, microservices |
| Scheduling style | Gang scheduling, backfill, exclusive allocation | Independent replica scheduling |
| Best for | Large distributed training | Autoscaled inference serving |
| Scaling | Fixed at submission (nodes/GPUs) | Dynamic (HPA/KEDA) |
| Job duration | Fixed, often long (hours-days) | Continuous/long-running |
| Networking | High-perf interconnect aware (topology) | Service mesh, load balancing |

### Core Slurm Commands
| Command | Purpose |
|---|---|
| `sbatch script.sh` | Submit batch job |
| `srun cmd` | Run job step interactively/blocking |
| `salloc` | Allocate resources for interactive shell |
| `squeue` | View job queue |
| `sinfo` | View partition/node status |
| `scontrol show job <id>` | Detailed job info |
| `--gres=gpu:N` | Request N GPUs |
| `--exclusive` | No node sharing with other jobs |
| `--partition=<name>` | Target a specific partition |

### Partition/QOS Design Pattern
- `train` partition: long max walltime (e.g., days), lower priority, preemptible.
- `infer` / `interactive` partition: short max walltime, highest QOS priority, low preemption tolerance.
- Partitions can share the same underlying nodes; QOS + priority differentiate access.

### Training vs Inference Resource Planning
| Aspect | Training | Inference |
|---|---|---|
| GPU memory need | High (weights + activations + gradients + optimizer states) | Lower (weights + small activation footprint) |
| GPU sharing | Rare — usually exclusive/full GPU or multi-GPU sharding | Common — MIG or time-slicing for density |
| Scaling pattern | Fixed at job submission | Dynamic, traffic-driven (HPA/KEDA) |
| Latency sensitivity | Low (batch, async) | High (real-time SLA) |
| Typical scheduler | Slurm | Kubernetes |

### Autoscaling for Inference
- HPA: scales on CPU/memory or custom metrics.
- KEDA: scales on external event sources (queue depth, request rate, GPU utilization).
- Combine with Triton Inference Server or KServe for model serving + autoscaling integration.

### Coexistence Patterns (Shared Infrastructure)
- **Slinky** (SchedMD + NVIDIA): runs Slurm jobs as Kubernetes-managed workloads — enables shared node pools between training and inference.
- **Run:ai**: GPU orchestration layer sitting atop Kubernetes, providing fractional GPU scheduling and fair-share policies across training/inference.
- Avoid: permanently split hardware silos when workload demand fluctuates — reduces utilization and increases cost.

### Exam Traps
- Assuming Kubernetes HPA alone handles gang scheduling — it does NOT.
- Assuming Slurm partitions imply separate physical hardware — they don't; they're logical/policy constructs.
- Assuming inference always needs full GPU memory like training — usually false; MIG/time-slicing is common for inference density.
