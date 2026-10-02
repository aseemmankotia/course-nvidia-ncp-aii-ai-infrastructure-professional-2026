## Chapter 7 Cheat Sheet: BCM Provisioning

### Domain
Cluster Management and Orchestration (~20% of exam, editorial estimate)

### BCM Architecture
| Component | Role |
|---|---|
| Head node | Central management, image repository, monitoring, scheduler config |
| Compute nodes | Run workloads; provisioned/imaged from head node |
| CMDaemon | Service on head + compute nodes; drives health checks, metrics, dashboard |
| Category | Grouping of nodes sharing a software image + config for bulk management |
| Software image | Versioned OS/driver/CUDA stack stored on head node, assigned to categories |

### Provisioning Workflow
1. Compute node PXE-boots
2. Node contacts head node, enters **node-installer** phase
3. Node-installer syncs node against assigned category's software image
4. Node boots into full OS with correct drivers/CUDA/software stack
5. CMDaemon starts on node, reports status to head node dashboard

### BCM vs Manual Provisioning
| Factor | BCM | Manual |
|---|---|---|
| Scale | Multi-node to large clusters | Single node / small handful |
| Consistency | Enforced via shared images/categories | No built-in guarantee, drift-prone |
| Updates | Push image update to category, all nodes updated | Per-node manual steps |
| Monitoring | Built-in health dashboard via CMDaemon | Must build/add separately |
| Overhead | Requires head node + management stack | No extra infra needed |
| Best fit | Production/research clusters, repeated builds | Dev workstation, ad-hoc single-node test |

### BCM Integration Points
- **Slurm**: BCM provisions nodes and includes workload manager modules to deploy/integrate Slurm as scheduler — BCM manages infra, Slurm manages job scheduling.
- **Kubernetes**: BCM includes Kubernetes deployment tooling (e.g., cm-kubernetes-setup) to stand up K8s on BCM-managed nodes — complementary, not exclusive.

### Key Interfaces
- `cmsh` — CLI for device/category/image management from head node
- Bright View / Base Command Manager Web UI — GUI, includes health/monitoring dashboard

### Exam Traps
- Assuming BCM is *mandatory* for any GPU/CUDA workload (it is optional cluster tooling).
- Confusing BCM (infrastructure/provisioning layer) with Slurm/Kubernetes (workload orchestration layer).
- Choosing BCM for a single-node/dev scenario — exam expects manual provisioning there.
- Forgetting that health/monitoring dashboard data comes from CMDaemon, not the scheduler.
