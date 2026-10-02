## Chapter 10: Diagnosing GPU, NVLink, and Network Failures — Practice Questions

**Q1.** A multi-GPU training job on an NVSwitch-based system intermittently fails with NCCL timeout errors. nvidia-smi reports all GPUs as healthy, but NVLink throughput is degraded on specific links. What is the BEST first diagnostic step?

A) Check nvidia-smi nvlink -e for CRC/replay error counters on the affected links
B) Reinstall the NCCL library
C) Restart the training job using fewer GPUs
D) Replace the NVSwitch baseboard

<details>
<summary>Answer</summary>

**Correct: A**

NVLink error counters (CRC/replay errors) directly indicate whether a physical link is faulty before escalating to hardware replacement or software workarounds.

**Why the others are wrong:**
- Reinstalling NCCL does not address uncorrected hardware-level link errors.
- Reducing GPU count is a workaround, not root-cause diagnosis.
- Replacing hardware without error-counter evidence is premature and costly.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Which command should be run to perform a comprehensive multi-level stress diagnostic (including memory bandwidth, NVLink topology, and thermal validation) to confirm a suspect node is fit to return to production?

A) dcgmi diag -r 3
B) nvidia-smi -q
C) ibdiagnet
D) ipmitool sensor list

<details>
<summary>Answer</summary>

**Correct: A**

DCGM diagnostic level 3 (r3) runs the full extended test suite covering stress, memory, and NVLink validation, the standard pre-production confirmation check.

**Why the others are wrong:**
- nvidia-smi -q only reports current static state, no active stress test.
- ibdiagnet is an InfiniBand fabric diagnostic tool, not a GPU stress test.
- ipmitool sensor list reports BMC sensor readings, not GPU compute diagnostics.

</details>

---

**Q3.** An engineer needs to check whether an InfiniBand link has physical-layer symbol errors, without involving GPU-level diagnostics. Which command is correct?

A) perfquery
B) nvidia-smi nvlink -e
C) dcgmi health -c
D) dmesg -T

<details>
<summary>Answer</summary>

**Correct: A**

perfquery reads IB port performance counters directly, including symbol errors, on a specific port.

**Why the others are wrong:**
- nvlink -e reports NVLink error counters, not InfiniBand.
- dcgmi health -c checks GPU subsystem health, not fabric ports.
- dmesg shows kernel messages generally, not specific IB port counters.

</details>

---

**Q4.** A GPU node experiences repeated random job failures under heavy load. dmesg logs show 'Xid 79: GPU has fallen off the bus' errors that began shortly after a driver update, with no corresponding VBIOS update applied. What is the MOST LIKELY root cause?

A) Faulty PCIe riser cable
B) Insufficient power supply capacity
C) Overheated NVLink bridge
D) Firmware/driver version mismatch

<details>
<summary>Answer</summary>

**Correct: D**

The timing correlation with a driver update lacking a matching VBIOS/firmware update is the classic signature of firmware/driver mismatch causing intermittent Xid 79 faults.

**Why the others are wrong:**
- A riser cable fault is possible but the scenario specifically points to the driver update timing.
- Power capacity issues typically present with different Xid signatures and are not implied here.
- NVLink bridge heat is unrelated to the described bus-drop symptom and update timing.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** Which log source should be checked FIRST when a GPU server fails to power on or reports a hardware fault before the OS has booted?

A) dmesg
B) DCGM diagnostic logs
C) nvidia-smi query log
D) BMC/IPMI System Event Log (SEL)

<details>
<summary>Answer</summary>

**Correct: D**

The BMC operates independently of the OS and GPU driver stack, capturing pre-boot power, thermal, and fan faults.

**Why the others are wrong:**
- dmesg requires the OS kernel to be running.
- DCGM requires the driver and GPU stack to be initialized.
- nvidia-smi also requires the driver stack to be loaded.

</details>

---

**Q6.** What is the correct node failure triage sequence for isolating hardware faults, from the lowest to the highest software layer?

A) BMC/IPMI → dmesg → nvidia-smi/DCGM → application/job logs
B) Application logs → nvidia-smi → dmesg → BMC/IPMI
C) DCGM → BMC/IPMI → dmesg → nvidia-smi
D) dmesg → BMC/IPMI → application logs → nvidia-smi

<details>
<summary>Answer</summary>

**Correct: A**

The correct bottom-up triage sequence is hardware layer (BMC) → kernel/OS (dmesg) → driver/GPU (nvidia-smi/DCGM) → application layer.

**Why the others are wrong:**
- Starting at the application layer skips lower-level hardware root causes.
- Placing DCGM before BMC ignores that DCGM requires the driver stack already loaded.
- This ordering mixes kernel and application layers incorrectly, skipping the hardware layer first.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** `nvidia-smi nvlink -s` shows one of eight NVLink connections as 'Inactive' while all others show 'Active'. DCGM diag level 1 passes with no errors. What should be the NEXT troubleshooting step?

A) Run dcgmi diag -r 3 to trigger an extended NVLink/topology-specific test
B) Ignore it since level 1 diagnostics passed
C) Immediately RMA the GPU
D) Downgrade the NVIDIA driver

<details>
<summary>Answer</summary>

**Correct: A**

Level 1 diagnostics are a quick health check and may not surface link-level issues; level 3 runs deeper topology and bandwidth-specific tests that can confirm or clear the inactive link.

**Why the others are wrong:**
- An inactive link out of eight is abnormal and must be investigated further, not ignored.
- RMA is premature without deeper diagnostic confirmation from an extended test.
- No evidence points to the driver version as the cause at this stage.

</details>

---

**Q8.** Which nvidia-smi query field indicates whether uncorrectable ECC memory errors have triggered row remapping on a GPU, an early indicator of failing memory?

A) nvidia-smi -q -d POWER
B) nvidia-smi -q -d CLOCK
C) nvidia-smi -q -d ROW_REMAPPER
D) nvidia-smi -q -d PERFORMANCE

<details>
<summary>Answer</summary>

**Correct: C**

The ROW_REMAPPER field reports pending and completed row remappings caused by uncorrectable ECC errors, an early warning of degrading HBM memory.

**Why the others are wrong:**
- POWER reports power draw and limits, unrelated to memory errors.
- CLOCK reports clock speeds, unrelated to ECC remapping.
- PERFORMANCE reports throttle reasons/state, not memory remap status.

</details>

---

