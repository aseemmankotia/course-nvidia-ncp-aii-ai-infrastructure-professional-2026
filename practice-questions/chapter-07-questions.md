## Chapter 7: Provisioning with Base Command Manager (BCM) — Practice Questions

**Q1.** A research lab is standing up a new 64-node GPU cluster and needs every compute node to boot with an identical, centrally-managed OS image, drivers, and CUDA stack, with the ability to push updates to all nodes at once. Which provisioning approach BEST fits this requirement?

A) Manually install the OS and CUDA toolkit on each node using a checklist
B) Use BCM head node with a shared software image and category-based node groups
C) Have each node administrator independently configure their own node
D) Use a generic cloud-init script with no central management

<details>
<summary>Answer</summary>

**Correct: B**

BCM's core value proposition is centralized image management from the head node, distributed to compute nodes grouped into categories, enabling one-shot updates across the fleet.

**Why the others are wrong:**
- Manual per-node installs do not scale to 64 nodes and introduce configuration drift, which is the exact problem BCM solves
- Independent per-admin configuration guarantees inconsistency across nodes, which the exam treats as an anti-pattern
- Generic cloud-init lacks BCM's health monitoring, image versioning, and workload manager integration

</details>

---

**Q2.** A single-node development workstation with one GPU is used by one engineer for ad-hoc experimentation. Which statement about provisioning is correct?

A) Manual provisioning is preferred here since the overhead of a head node and cluster management stack is not justified for a single node
B) BCM must be installed because it is required for all NVIDIA GPU workloads
C) BCM is mandatory whenever CUDA is installed
D) Kubernetes must be deployed via BCM even for a single workstation

<details>
<summary>Answer</summary>

**Correct: A**

The exam explicitly tests recognizing when manual provisioning is preferred: small scale, single-node, or ad-hoc setups where BCM's head-node/category/image overhead provides no operational benefit.

**Why the others are wrong:**
- BCM is not a requirement for running GPU workloads; it is an optional cluster management layer
- CUDA installation has no dependency on BCM
- Kubernetes can be deployed without BCM; BCM is one optional deployment path, not mandatory

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An administrator notices that several compute nodes in a BCM-managed cluster are reporting degraded health status on the monitoring dashboard. Which BCM component is responsible for collecting node health metrics and surfacing this status?

A) The Slurm controller (slurmctld)
B) The node-installer PXE boot process
C) CMDaemon running on the head node and compute nodes
D) The software image repository

<details>
<summary>Answer</summary>

**Correct: C**

CMDaemon is the BCM service running on both head and compute nodes that collects metrics, runs health checks, and reports status to the monitoring/health dashboard.

**Why the others are wrong:**
- slurmctld manages job scheduling, not BCM node health monitoring
- The node-installer only runs during boot/provisioning, not for ongoing health monitoring
- The software image repository stores OS images; it does not perform health checks

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which of the following BEST describes how BCM relates to Slurm and Kubernetes in a GPU cluster deployment?

A) BCM replaces Slurm and Kubernetes entirely, acting as the sole workload orchestrator
B) BCM can only be used with Slurm and has no Kubernetes support
C) BCM and Kubernetes are mutually exclusive and cannot coexist on the same cluster
D) BCM provisions and manages the underlying nodes/OS/software stack and includes modules to deploy and integrate Slurm or Kubernetes on top

<details>
<summary>Answer</summary>

**Correct: D**

BCM operates at the infrastructure/provisioning layer, and separately provides setup modules (e.g., cm-kubernetes-setup, workload manager integration) to deploy Slurm or Kubernetes on top of the managed nodes.

**Why the others are wrong:**
- BCM does not replace workload orchestrators; it provisions the infrastructure they run on
- BCM includes Kubernetes deployment tooling, so this is factually incorrect
- BCM supports deploying Kubernetes on BCM-managed nodes, they are not mutually exclusive

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A company needs to update the CUDA driver version across 200 GPU compute nodes with minimal manual intervention and wants a rollback option if the update causes issues. Which BCM feature BEST supports this?

A) Manually SSH into each node and run driver installers
B) A shell script distributed via email to each node owner
C) Software image versioning and category-based image assignment on the head node
D) Reinstalling the entire OS from scratch on every node

<details>
<summary>Answer</summary>

**Correct: C**

BCM maintains versioned software images per category; updating the image and reassigning/re-syncing nodes to it provides centralized rollout and the ability to revert to a prior image version.

**Why the others are wrong:**
- Manual SSH per node does not scale to 200 nodes and has no built-in rollback
- Emailing scripts is not a managed, auditable, or scalable provisioning method
- Full OS reinstall is unnecessary overhead when BCM image sync can push just the changed components

</details>

---

**Q6.** In BCM terminology, what is the purpose of grouping compute nodes into 'categories'?

A) Categories associate nodes with a specific software image, roles, and configuration so they can be managed as a set
B) Categories determine the physical rack location of nodes
C) Categories are only used for billing purposes
D) Categories replace the need for a head node

<details>
<summary>Answer</summary>

**Correct: A**

A BCM category is a configuration grouping that ties a set of nodes to a common software image, roles, and settings, enabling bulk configuration and updates.

**Why the others are wrong:**
- Categories are a logical/configuration grouping, not a physical location construct
- Categories are unrelated to billing; that is not a BCM concept
- Categories are a management abstraction that still requires a head node to function

</details>

---

**Q7.** During BCM node provisioning, a new compute node PXE-boots and contacts the head node before loading its full OS image. What is this initial boot process called?

A) The node-installer phase
B) The kube-apiserver bootstrap
C) The slurmd registration phase
D) The CUDA toolkit install phase

<details>
<summary>Answer</summary>

**Correct: A**

In BCM, a compute node PXE-boots into the node-installer, which fetches and applies the assigned software image from the head node before switching to the local OS.

**Why the others are wrong:**
- kube-apiserver bootstrap relates to Kubernetes control plane setup, not BCM's node provisioning boot stage
- slurmd registration happens after the OS and Slurm client are running, not during initial provisioning boot
- CUDA toolkit installation is part of the software image content, not the name of the boot process itself

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which statement accurately distinguishes BCM-based provisioning from manual provisioning?

A) BCM provides centralized image management, health monitoring, and workload manager integration, while manual provisioning requires configuring each node individually with no built-in consistency guarantees
B) Manual provisioning always results in faster cluster deployment for any cluster size
C) BCM cannot be used for clusters larger than 10 nodes
D) Manual provisioning includes built-in health dashboards identical to BCM

<details>
<summary>Answer</summary>

**Correct: A**

This captures the core exam distinction: BCM centralizes image/config/health management and integrates with Slurm/Kubernetes, whereas manual provisioning is node-by-node with no built-in consistency or monitoring tooling.

**Why the others are wrong:**
- Manual provisioning is typically slower and less consistent as cluster size grows, not faster
- BCM is designed to scale to large clusters, not limited to 10 nodes
- Manual provisioning has no built-in dashboard; that tooling must be built or added separately

</details>

---

