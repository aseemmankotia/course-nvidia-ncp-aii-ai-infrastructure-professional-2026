## Chapter 2: Firmware, BIOS, Driver, and CUDA Stack Validation — Practice Questions

**Q1.** A data center technician is bringing up a new GPU server. Which sequence is correct?

A) Install CUDA toolkit, then GPU driver, then update BIOS/firmware, then reboot
B) Update system BIOS/firmware, reboot, install the GPU kernel driver, then install the CUDA toolkit
C) Install GPU driver, update BIOS/firmware, install CUDA toolkit, reboot
D) Install CUDA toolkit and GPU driver simultaneously, then update BIOS/firmware

<details>
<summary>Answer</summary>

**Correct: B**

Firmware/BIOS establishes platform configuration (PCIe/NVLink topology) that the driver relies on; the reboot applies firmware; the kernel driver must be loaded before CUDA toolkit installation since CUDA depends on the driver's kernel module.

**Why the others are wrong:**
- Reverses the dependency chain — CUDA installation will mismatch or fail without a compatible driver already loaded.
- Skips the required reboot after firmware update, leaving stale PCIe/NVLink topology in effect when the driver loads.
- Simultaneous installation risks version conflicts and firmware settings not being applied before the driver probes hardware.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** After running nvidia-smi topo -m on an 8-GPU NVLink server, two GPUs show 'PHB' instead of the expected 'NV#' connection type between them. What does this indicate?

A) Those two GPUs are connected via NVLink through a shared PCIe host bridge
B) Those two GPUs are connected through a PCIe host bridge rather than NVLink — no direct NVLink path exists between them
C) The GPUs have a firmware mismatch and need re-flashing
D) DCGM diagnostics failed on those GPUs

<details>
<summary>Answer</summary>

**Correct: B**

PHB indicates the connection traverses PCIe through a Host Bridge — meaning no direct NVLink path exists between the GPU pair.

**Why the others are wrong:**
- Incorrectly conflates PCIe path types with NVLink connectivity — PHB means NVLink is absent, not present.
- Firmware mismatch is not what topo -m reports; this is a topology classification, not a health/version issue.
- topo -m does not run or reflect DCGM diagnostic results; it only reports interconnect topology.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** An administrator needs to run a comprehensive pre-production hardware validation on a new GPU node, including memory, NVLink bandwidth, and thermal stress tests, before accepting it into the cluster. Which tool BEST fits this?

A) dcgmi diag -r 3
B) nvidia-smi -q
C) nvcc --version
D) nvidia-smi topo -m

<details>
<summary>Answer</summary>

**Correct: A**

DCGM diagnostics run level 3 (long/extended test) performs stress-based health checks covering memory, NVLink, and thermal behavior, appropriate for pre-production node acceptance.

**Why the others are wrong:**
- Only reports a current status/config snapshot with no stress testing.
- Only reports the CUDA compiler/toolkit version; unrelated to hardware validation.
- Only shows interconnect topology, not health or stress test results.

</details>

---

**Q4.** A team installed NVIDIA driver version 525.60.13 on a Linux GPU server. Which CUDA toolkit version(s) does this driver support per the compatibility matrix?

A) Only CUDA 11.8 and earlier
B) CUDA 13.x exclusively
C) Any CUDA version, since drivers are always fully backward and forward compatible
D) CUDA 12.0 and later CUDA 12.x releases within the same major version compatibility window

<details>
<summary>Answer</summary>

**Correct: D**

Driver 525.60.13 is the documented minimum required driver for CUDA 12.0 on Linux, and minor version compatibility extends support to later CUDA 12.x toolkit releases.

**Why the others are wrong:**
- Understates capability — the driver supports CUDA 12.x, not just older 11.8.
- Overstates capability — a new CUDA major version (13.x) requires a newer minimum driver.
- False — compatibility is bounded by the documented minimum driver version per CUDA release, not universal.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Before deploying a cluster of 32 identical GPU servers, what is the recommended firmware validation practice?

A) Install drivers first, then apply firmware updates individually only as issues arise
B) Only update firmware on nodes that fail initial GPU tests
C) Validate and apply the same firmware bundle version across all nodes before driver installation, confirming via a per-node version report
D) Firmware versions can differ between nodes as long as CUDA versions match

<details>
<summary>Answer</summary>

**Correct: C**

Consistent firmware across all nodes prevents topology and NVLink behavior discrepancies, and should be confirmed before driver/CUDA layers are installed.

**Why the others are wrong:**
- Installing drivers against inconsistent or outdated firmware configs risks instability discovered late.
- A reactive approach misses latent inconsistencies that don't cause immediate test failures.
- Firmware inconsistency can cause differing NVLink/PCIe behavior even when CUDA versions match across nodes.

</details>

---

**Q6.** In nvidia-smi topo -m output, what does 'NV4' between two GPUs indicate?

A) The GPUs are on NVLink generation 4
B) The GPUs are 4 PCIe hops apart
C) The GPUs share 4 GB of NVLink memory
D) The GPUs have 4 direct NVLink connections between them

<details>
<summary>Answer</summary>

**Correct: D**

NV# denotes the number of direct NVLink connections linking the GPU pair, not generation or distance.

**Why the others are wrong:**
- Misreads NV# as an NVLink generation indicator, which topo -m does not report this way.
- Confuses NVLink connection count with PCIe hop distance, which is represented separately as PXB/PHB/NODE/SYS.
- Fabricated interpretation — topo -m does not report shared memory amounts.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** What is the primary functional difference between nvidia-smi and DCGM (Data Center GPU Manager)?

A) nvidia-smi is used only for driver installation, while DCGM handles CUDA toolkit installation
B) nvidia-smi provides quick per-GPU status/monitoring queries, while DCGM provides deeper diagnostics, health monitoring, and cluster-scale management
C) DCGM is a deprecated tool replaced entirely by nvidia-smi
D) nvidia-smi manages NVSwitch fabric configuration, while DCGM only reports temperature

<details>
<summary>Answer</summary>

**Correct: B**

nvidia-smi is a lightweight CLI tool for per-GPU status and configuration; DCGM (via dcgmi diag) provides deeper diagnostics, health checks, and telemetry at data-center scale.

**Why the others are wrong:**
- Neither tool installs drivers or the CUDA toolkit.
- DCGM is actively maintained and not deprecated.
- NVSwitch fabric configuration is handled by NVIDIA Fabric Manager, not either of these tools, and DCGM reports far more than temperature.

</details>

---

**Q8.** On an NVSwitch-based (HGX) server, GPUs fail to communicate over NVLink after driver installation despite nvidia-smi topo -m showing correct NV# connections. What is the MOST likely missing step?

A) The CUDA toolkit was not reinstalled after the driver update
B) The NVIDIA Fabric Manager service was not started to configure the NVSwitch fabric
C) The BIOS needs to be downgraded to an earlier version
D) nvidia-smi needs to be run with a special flag to activate NVLink

<details>
<summary>Answer</summary>

**Correct: B**

On NVSwitch systems, Fabric Manager must be running to initialize and configure the switch fabric for actual GPU-to-GPU NVLink communication, even when topology reporting looks correct.

**Why the others are wrong:**
- CUDA reinstallation does not affect fabric initialization.
- Downgrading BIOS is not a standard fix and unrelated to the fabric manager requirement.
- nvidia-smi has no such flag to 'activate' NVLink — it only reports status, it does not configure the fabric.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

