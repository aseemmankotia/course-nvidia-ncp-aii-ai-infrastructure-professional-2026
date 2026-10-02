## Chapter 3: Rack, Cable, and Configure Compute Servers — Practice Questions

**Q1.** A technician is bringing up a brand-new DGX node that will run GPU workloads inside containers. Which sequence is correct?

A) Install DGX OS, install the base NVIDIA driver, install a container engine (Docker/containerd), then install and configure NVIDIA Container Toolkit
B) Install NVIDIA Container Toolkit, then install the driver, then install DGX OS
C) Install the container engine, then DGX OS, then the driver
D) Install the driver, then DGX OS, then the NGC CLI, skipping the container toolkit entirely

<details>
<summary>Answer</summary>

**Correct: A**

The container toolkit is a shim that exposes GPUs to an already-running container engine using an already-installed driver, so OS → driver → container engine → NVIDIA Container Toolkit is the only order that lets each layer detect its dependency.

**Why the others are wrong:**
- The toolkit has nothing to attach to without a driver or container engine already present — it will fail to configure
- A container engine installed before the OS is complete is not a valid installation state
- Skipping the toolkit means containers cannot see the GPU at all; nvidia-smi inside the container will fail

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Why does NVIDIA recommend DGX OS over a stock Ubuntu installation for a DGX server that must join a validated multi-node cluster?

A) DGX OS is cheaper to license than standard Ubuntu
B) DGX OS is required because standard Ubuntu cannot install any NVIDIA driver
C) DGX OS removes the need for the NVIDIA Container Toolkit
D) DGX OS ships with NVIDIA-validated kernel, driver, and networking (e.g., OFED) stack versions pre-tested together for that hardware generation

<details>
<summary>Answer</summary>

**Correct: D**

DGX OS is a customized, NVIDIA-tested Linux distribution with pinned, compatible versions of the kernel, GPU driver, and networking drivers, reducing the risk of version drift across cluster nodes.

**Why the others are wrong:**
- Licensing cost is not the deciding factor NVIDIA documentation cites
- Standard Ubuntu can absolutely run NVIDIA drivers — that's not the reason
- The Container Toolkit is still required on DGX OS; it is not eliminated

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** When planning rack unit layout for a row of NVIDIA-Certified servers, which airflow configuration should the servers and hot/cold aisles follow?

A) Side-to-side airflow with servers facing each other
B) Bottom-to-top airflow only, aisle direction is irrelevant
C) Rear-to-front airflow with cold aisle behind the rack
D) Front-to-back airflow, with cold aisle at the intake (front) and hot aisle at the exhaust (rear)

<details>
<summary>Answer</summary>

**Correct: D**

NVIDIA-Certified and DGX servers use front-to-back airflow; racks must be arranged so cold aisles face intake fronts and hot aisles collect rear exhaust to prevent recirculation and thermal throttling.

**Why the others are wrong:**
- Side-to-side airflow is not how these rack-mount servers are designed to cool
- Airflow direction absolutely matters for thermal design and is explicitly tested
- Reversing cold aisle to the rear causes exhaust heat to be re-ingested, which is the exact mistake the exam flags

</details>

---

**Q4.** A multi-node GPU cluster runs NCCL-based distributed training. Jobs intermittently hang or fail only on two of eight nodes. What should be checked first per the node compatibility checklist?

A) Confirm the two nodes have a different chassis paint color
B) Increase the rack unit spacing around the two nodes
C) Swap the PDUs feeding those two nodes
D) Confirm the two nodes have matching driver, CUDA toolkit, and NIC/OFED firmware versions to the rest of the cluster

<details>
<summary>Answer</summary>

**Correct: D**

Version mismatches in driver, CUDA, or network firmware between nodes are the leading cause of intermittent NCCL collective communication failures in a cluster; the compatibility checklist exists specifically to catch this before workloads run.

**Why the others are wrong:**
- Cosmetic differences have no effect on cluster behavior
- Rack spacing affects cooling, not intermittent job hangs tied to specific nodes
- PDU assignment affects power redundancy, not NCCL communication errors

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** For power redundancy best practice when cabling a DGX server's dual power supplies to facility power, what is the correct approach?

A) Connect each power supply to a separate PDU fed from independent electrical circuits (A/B feed)
B) Connect both power supplies to the same PDU on the same electrical circuit
C) Leave the second power supply uncabled until a failure occurs
D) Connect both power supplies to a single UPS with no PDU

<details>
<summary>Answer</summary>

**Correct: A**

Redundant PSUs are only meaningful for uptime if each is fed from an independent PDU/circuit (A/B feed), so a single circuit or PDU failure does not take the server down.

**Why the others are wrong:**
- Using the same PDU defeats the purpose of dual power supplies — a single PDU failure still takes the node offline
- Leaving a PSU uncabled removes redundancy entirely and is not a supported configuration
- A single UPS with no separate feeds still represents a single point of failure

</details>

---

**Q6.** What is NGC (NVIDIA GPU Cloud) in the context of preparing a newly configured server for AI workloads?

A) A physical GPU expansion chassis for DGX systems
B) The BIOS/firmware update utility for DGX servers
C) NVIDIA's container registry providing pre-built, GPU-optimized, security-scanned containers for frameworks and applications
D) A licensing server required to activate the NVIDIA driver

<details>
<summary>Answer</summary>

**Correct: C**

NGC (nvcr.io) is NVIDIA's registry of GPU-optimized, validated containers (frameworks, HPC apps, pretrained models) that admins pull after the container runtime and toolkit are configured.

**Why the others are wrong:**
- NGC is software, not a hardware expansion chassis
- Firmware updates are handled by separate tools (e.g., FW update packages/BMC), not NGC
- The NVIDIA driver does not require an NGC license server to activate

</details>

---

**Q7.** Which command correctly configures Docker to use the NVIDIA runtime after the NVIDIA Container Toolkit has been installed?

A) nvidia-smi --configure-docker
B) docker install nvidia-runtime
C) apt install nvidia-docker-runtime --auto-configure
D) nvidia-ctk runtime configure --runtime=docker

<details>
<summary>Answer</summary>

**Correct: D**

The nvidia-ctk CLI (part of NVIDIA Container Toolkit) is the correct tool to register the NVIDIA runtime with Docker's daemon.json; the Docker daemon must then be restarted for it to take effect.

**Why the others are wrong:**
- nvidia-smi is a monitoring tool and has no runtime configuration flag
- There is no such docker subcommand to install a runtime this way
- This package name and flag do not exist in the current toolkit workflow

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** A team is planning rack space for a row of DGX H100 systems in a 42U rack. Which factor is most commonly overlooked and heavily tested regarding rack unit planning?

A) Only counting the physical U height of each server and ignoring power draw and cooling capacity per rack
B) The color of cage nuts used to mount the server
C) The brand of network cables used
D) The number of USB ports on the front of the chassis

<details>
<summary>Answer</summary>

**Correct: A**

Rack unit planning must account for per-rack power budget and cooling capacity, not just how many U slots are physically available — a rack can run out of power/cooling headroom long before it runs out of U space, which is the classic exam trap.

**Why the others are wrong:**
- Cage nut color has no technical impact
- Cable brand doesn't factor into rack capacity planning
- USB port count is irrelevant to power/cooling capacity planning

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

