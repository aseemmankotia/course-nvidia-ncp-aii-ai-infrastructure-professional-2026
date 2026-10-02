## Chapter 3 Cheat Sheet: Rack, Cable, Configure Compute Servers

### Install Order (heavily tested)
| Step | Action |
|---|---|
| 1 | Rack & cable server (power + network) |
| 2 | Install OS (DGX OS or validated Linux distro) |
| 3 | Install base NVIDIA driver |
| 4 | Install container engine (Docker/containerd) |
| 5 | Install NVIDIA Container Toolkit |
| 6 | Configure runtime: `nvidia-ctk runtime configure --runtime=docker` + restart daemon |
| 7 | Authenticate to NGC: `docker login nvcr.io -u '$oauthtoken' -p <API_KEY>` |
| 8 | Pull/test GPU container, verify GPU visibility |

**Exam trap:** Reversing steps 3–5 (toolkit before driver/engine) is the #1 wrong-order distractor.

### Rack & Airflow
| Element | Correct Value |
|---|---|
| Airflow direction | Front-to-back |
| Cold aisle | Server front (intake) |
| Hot aisle | Server rear (exhaust) |
| Planning constraint | Power & cooling budget per rack, not just U-height |

### Power Cabling
- Dual PSUs → separate PDUs → independent circuits (A/B feed)
- Never cable both PSUs to the same PDU/circuit — defeats redundancy

### OS Selection
| Option | When to Use |
|---|---|
| DGX OS | DGX hardware, validated cluster, NVIDIA-pinned kernel/driver/OFED stack |
| Standard Linux distro | NVIDIA-Certified (non-DGX) servers where vendor/NVIDIA validates that distro |

**Exam trap:** "DGX OS is required because Ubuntu can't run NVIDIA drivers" — false; reason is version validation, not capability.

### NGC Basics
- Registry URL: `nvcr.io`
- Auth username is literally `$oauthtoken`; password is your NGC API key
- Provides pre-optimized, security-scanned containers (frameworks, HPC, pretrained models)

### Multi-Node Compatibility Checklist
- [ ] Driver version matches across all nodes
- [ ] CUDA toolkit version matches across all nodes
- [ ] NIC/OFED firmware matches across all nodes
- [ ] GPU topology (`nvidia-smi topo -m`) matches expected hardware layout
- [ ] Container runtime + toolkit versions aligned cluster-wide

**Exam trap:** Intermittent NCCL hangs on specific nodes → check version drift first, not cabling or rack position.

### Key Commands
```
nvidia-ctk runtime configure --runtime=docker
systemctl restart docker
docker login nvcr.io -u '$oauthtoken' -p <NGC_API_KEY>
nvidia-smi topo -m
```
