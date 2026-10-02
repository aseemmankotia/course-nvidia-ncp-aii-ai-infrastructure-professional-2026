## Chapter 11: Benchmarking, Performance Tuning, and Monitoring — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | H100 SXM approximate GPU thermal slowdown threshold? | ~83°C. Exceeding this triggers clock throttling. Check DCGM_FI_DEV_THERMAL_VIOLATION to confirm thermal (vs power) cause. |
| 2 | H100 SXM5 max TDP / power cap range? | Up to 700W TDP; configurable lower via nvidia-smi -pl. Exam tip: reducing power cap trades performance for thermal/power headroom. |
| 3 | H100 SXM NVLink 4 aggregate per-GPU bandwidth? | ~900 GB/s. Exam tip: compare NCCL busbw against this ceiling to detect NVLink-layer bottlenecks. |
| 4 | InfiniBand NDR single-port bandwidth? | 400 Gb/s per port. Exam tip: rail-optimized designs assign one NIC/port per GPU at this rate. |
| 5 | DCGM diagnostic levels (dcgmi diag -r)? | 1=Quick(seconds), 2=Medium(~2min), 3=Long(several min, deep stress/NVLink/memory), 4=Extended(1+ hour). Exam tip: use -r 3 for pre-job validation. |
| 6 | Command to set a persistent GPU power cap of 500W on GPU 0? | nvidia-smi -i 0 -pl 500 (pair with nvidia-smi -pm 1 for persistence mode). Exam tip: -pl sets watts, -pm toggles persistence mode (0/1). |
| 7 | Env var to get verbose NCCL topology/path logging? | NCCL_DEBUG=INFO (combine with NCCL_DEBUG_SUBSYS=ALL). Exam tip: reveals which NICs/rails and ring/tree structure NCCL selected. |
| 8 | Env var that restricts/filters which InfiniBand HCAs NCCL uses? | NCCL_IB_HCA. Exam tip: a misconfigured filter is a classic cause of only partial rails being discovered/used. |
| 9 | algbw vs busbw in nccl-tests? | algbw = message size / time. busbw = algbw × collective-specific factor (e.g., 2*(n-1)/n for all-reduce), representing actual per-link data movement — compare busbw to theoretical peak link bandwidth. |
| 10 | ECN+Adaptive Routing vs PFC for IB congestion control? | ECN + Adaptive Routing mitigates incast/congestion by rerouting and signaling flows; PFC (pause-based) can cause head-of-line blocking. Exam tip: prefer ECN+AR for collective-heavy AI fabrics. |
| 11 | Rail-optimized topology definition? | Each GPU is paired with a dedicated NIC ('rail') so GPU-to-GPU traffic across nodes stays on a consistent, non-oversubscribed path. Exam tip: verify with nvidia-smi topo -m that GPU-NIC PCIe affinity matches the rail design. |
| 12 | DCGM_FI_DEV_THERMAL_VIOLATION nonzero + DCGM_FI_DEV_POWER_VIOLATION zero — meaning? | GPU is throttling due to temperature, not power limit. Remediate cooling, not power cap. Exam tip: always check both violation fields together before deciding remediation. |
