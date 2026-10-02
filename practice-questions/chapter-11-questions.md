## Chapter 11: Benchmarking, Performance Tuning, and Monitoring — Practice Questions

**Q1.** During NCCL all_reduce_perf testing across 8 nodes with NVLink and NDR InfiniBand fabric, GPU utilization measured by DCGM shows 97-99% but achieved bus bandwidth is only 40% of theoretical peak. Which action BEST identifies whether the bottleneck is network fabric or compute?

A) Increase batch size in the workload to raise GPU utilization further
B) Run nccl-tests with NCCL_DEBUG=INFO and compare algorithm bandwidth (algbw) against bus bandwidth (busbw) while checking NCCL topology detection for rail/NIC assignment
C) Enable power capping via nvidia-smi -pl to reduce GPU power draw
D) Increase the DCGM diagnostic level to -r 4 to run an extended GPU stress test

<details>
<summary>Answer</summary>

**Correct: B**

NCCL_DEBUG=INFO exposes which NICs/rails and ring/tree paths NCCL selected. Comparing algbw vs busbw reveals whether the achieved throughput matches fabric topology expectations, isolating whether the limiter is network path/topology misdetection rather than the GPU (already near-saturated at 97-99% util, so compute is unlikely the limiter).

**Why the others are wrong:**
- Increasing batch size does not diagnose the existing bottleneck; GPU util is already near-max.
- Power capping reduces performance and does not diagnose a fabric bottleneck.
- DCGM diag -r 4 validates GPU hardware health, not network/fabric throughput issues between compute and network layers.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** DCGM monitoring shows periodic SM clock throttling on H100 SXM GPUs during sustained training. DCGM_FI_DEV_THERMAL_VIOLATION is incrementing while DCGM_FI_DEV_POWER_VIOLATION remains zero, and GPU temperatures approach the vendor slowdown threshold (~83°C). What is the correct remediation?

A) Lower the GPU power limit using nvidia-smi -pl to reduce heat output
B) Increase NCCL_ALGO to Tree to reduce GPU compute load
C) Switch nccl-tests to use the LL128 protocol to lower GPU utilization
D) Improve rack/chassis airflow or liquid cooling capacity, since the GPUs are hitting the thermal slowdown threshold, not the power limit

<details>
<summary>Answer</summary>

**Correct: D**

THERMAL_VIOLATION nonzero with POWER_VIOLATION at zero indicates the GPU is temperature-bound, not power-bound. The fix is improving cooling capacity so temperatures stay below the slowdown threshold.

**Why the others are wrong:**
- Lowering the power limit reduces performance further but does not address the root thermal cause, and power was not the limiting factor here.
- NCCL_ALGO choice does not affect GPU thermal behavior.
- NCCL protocol selection does not resolve a cooling/thermal problem.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q3.** In nccl-tests output for an all_reduce_perf benchmark, what does 'busbw' represent compared to 'algbw'?

A) busbw is algbw multiplied by a collective-specific correction factor (e.g., 2*(n-1)/n for all-reduce), reflecting actual data moved per link versus the algorithmic problem size
B) busbw is always identical to algbw for all collective operations
C) busbw is algbw divided by the total number of GPUs in the job
D) busbw measures PCIe bandwidth while algbw measures NVLink bandwidth

<details>
<summary>Answer</summary>

**Correct: A**

busbw normalizes algbw by a collective-specific factor so it can be directly compared against the theoretical peak link bandwidth, since ring-based all-reduce moves more bytes per link than the raw problem size implies.

**Why the others are wrong:**
- busbw and algbw differ by the correction factor and are not identical.
- Dividing by GPU count is not the correction formula NCCL uses.
- Both metrics are link-agnostic; they don't distinguish PCIe vs NVLink specifically.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** Which DCGM diagnostic level provides comprehensive GPU/NVLink/memory stress testing suitable for pre-job validation, completing in a few minutes rather than requiring an hour or more?

A) dcgmi diag -r 1 (quick)
B) dcgmi diag -r 4 (extended)
C) dcgmi diag -r 3 (long)
D) dcgmi discovery -l

<details>
<summary>Answer</summary>

**Correct: C**

The 'long' diagnostic level (-r 3) runs deeper stress, NVLink, and memory tests within several minutes, balancing thoroughness with time — appropriate for pre-job validation.

**Why the others are wrong:**
- The quick level (-r 1) runs only shallow checks in seconds, insufficient for pre-job validation.
- The extended level (-r 4) can take over an hour, too long for routine pre-job checks.
- dcgmi discovery -l only lists detected GPUs; it performs no health testing.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A rail-optimized InfiniBand fabric shows congestion and increased tail latency during large all-to-all collective operations from multiple GPU nodes converging on shared switch uplinks. Which fabric-level configuration BEST addresses this?

A) Disable adaptive routing to keep traffic on static deterministic paths
B) Increase the NCCL_BUFFSIZE environment variable on all GPU nodes
C) Enable ECN-based congestion control along with adaptive routing on the InfiniBand switches to spread flows and react to congestion signals
D) Set NCCL_ALGO=Tree exclusively to eliminate ring-based communication

<details>
<summary>Answer</summary>

**Correct: C**

ECN combined with adaptive routing is the standard InfiniBand fabric-level mitigation for incast congestion, redistributing flows across paths and reacting to congestion signals at the switch layer.

**Why the others are wrong:**
- Disabling adaptive routing removes path diversity and worsens hotspot congestion.
- Increasing NCCL buffer size is a host-side tuning knob that does not resolve switch-level congestion.
- Forcing Tree algorithm changes communication pattern but does not address fabric congestion at the switch/uplink layer.

</details>

---

**Q6.** Which nvidia-smi command correctly sets a power cap of 500 watts on GPU index 0?

A) nvidia-smi --power-limit=500 --all
B) nvidia-smi -i 0 -ac 500,500
C) nvidia-smi -i 0 -pm 500
D) nvidia-smi -i 0 -pl 500

<details>
<summary>Answer</summary>

**Correct: D**

The -pl flag with -i (index) sets the power limit in watts for a specific GPU.

**Why the others are wrong:**
- This is not valid nvidia-smi syntax for setting a power limit.
- -ac sets application clocks (memory,graphics), not power limit.
- -pm toggles persistence mode (0/1); 500 is not a valid persistence mode value.

</details>

---

**Q7.** A rail-optimized 8-GPU-per-node cluster is designed so each GPU has a dedicated 400Gb/s NIC. NCCL all_reduce_perf shows bandwidth consistent with only 4 of 8 rails being used. What is the MOST likely cause and fix?

A) The GPUs are thermally throttling, which reduces effective compute and halves bandwidth
B) NCCL_IB_HCA is misconfigured or filtering out half the NICs; verify GPU-NIC mapping with nvidia-smi topo -m and correct the HCA list so NCCL discovers all 8 rails
C) The switch fabric requires SHARP to be disabled in order to use all rails
D) DCGM_FI_DEV_POWER_USAGE indicates a power cap is limiting NIC bandwidth

<details>
<summary>Answer</summary>

**Correct: B**

A classic cause of only a subset of rails being used is an NCCL_IB_HCA filter or PCIe/NUMA affinity misconfiguration that excludes some NICs from discovery; nvidia-smi topo -m verifies actual GPU-to-NIC mapping.

**Why the others are wrong:**
- Thermal throttling affects GPU compute clocks, not the number of NICs/rails NCCL discovers and uses.
- SHARP relates to in-network reduction offload, not rail/NIC discovery count.
- GPU power usage does not control NIC bandwidth or rail selection.

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** Which DCGM field should be monitored to detect GPU hardware/driver faults (e.g., Xid events) that could silently degrade distributed training performance?

A) DCGM_FI_DEV_GPU_UTIL
B) DCGM_FI_DEV_XID_ERRORS
C) DCGM_FI_DEV_SM_CLOCK
D) DCGM_FI_DEV_MEM_COPY_UTIL

<details>
<summary>Answer</summary>

**Correct: B**

DCGM_FI_DEV_XID_ERRORS surfaces driver-reported Xid fault codes indicating hardware or driver-level GPU issues that can silently degrade or crash training jobs.

**Why the others are wrong:**
- GPU_UTIL measures compute utilization, not fault conditions.
- SM_CLOCK measures clock speed, useful for throttling detection but not fault codes.
- MEM_COPY_UTIL measures memory copy engine utilization, not hardware faults.

</details>

---

