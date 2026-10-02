## Chapter 10: Diagnosing GPU, NVLink, and Network Failures — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | What does `nvidia-smi nvlink -e` show? | NVLink error counters per link (CRC errors, replay errors). Exam tip: use to isolate a physical link fault vs a software/NCCL-layer issue. |
| 2 | dcgmi diag -r 1 vs -r 2 vs -r 3 — difference? | r1 = quick health check (~seconds), r2 = medium diagnostic (~2 min), r3 = extended stress test incl. memory bandwidth/NVLink/thermal (~12+ min). Exam tip: r3 required before returning a suspect node to production. |
| 3 | Xid 79 in dmesg/syslog means? | GPU has fallen off the bus. Common root causes: PCIe/power fault or driver/VBIOS firmware mismatch. Exam tip: check driver and VBIOS version alignment first. |
| 4 | Xid 48 meaning? | Double-bit (uncorrectable) ECC error. Exam tip: cross-check the ROW_REMAPPER field for pending remaps indicating failing memory. |
| 5 | Node failure triage order, bottom to top? | BMC/IPMI (SEL) → dmesg (kernel) → nvidia-smi/DCGM (driver/GPU) → application/job logs. Exam tip: always start at the hardware layer, never the application layer. |
| 6 | perfquery vs ibdiagnet — when to use each? | perfquery: reads live InfiniBand port counters (e.g., symbol errors) on a single port. ibdiagnet: full fabric-wide diagnostic scan across all switches/links. Exam tip: perfquery for one link, ibdiagnet for the whole fabric. |
| 7 | Which command lists NVLink status (active/inactive) per link? | nvidia-smi nvlink -s. Exam tip: an 'Inactive' link status even after DCGM r1 passes should trigger an r3 extended test, not dismissal. |
| 8 | What process must be running for NVLink fabric to initialize on NVSwitch-based systems (DGX/HGX)? | nvidia-fabricmanager (Fabric Manager). Exam tip: if the FM service fails or crashes, the NVLink fabric will not come up — check FM logs first on multi-GPU NVSwitch nodes. |
| 9 | Where are pre-OS-boot hardware faults (power, fan, thermal) logged? | BMC System Event Log (SEL), read via `ipmitool sel list` or Redfish. Exam tip: check this before dmesg since it doesn't require the OS or driver to be running. |
| 10 | What does `nvidia-smi -q -d ROW_REMAPPER` show? | Pending and completed memory row remappings caused by uncorrectable ECC errors. Exam tip: a pending remap requires a GPU reset to apply; a growing count signals failing HBM. |
| 11 | What generates a full diagnostic bundle for NVIDIA GPU support cases? | nvidia-bug-report.sh — collects driver state, kernel logs, dmesg, and nvidia-smi output into one archive. Exam tip: this is the standard first artifact requested by NVIDIA support. |
| 12 | Firmware/driver mismatch as root cause — key exam signal? | Intermittent Xid errors or a GPU 'falling off the bus' shortly after a driver update without a matching VBIOS/firmware update. Exam tip: this is a common distractor vs. outright hardware failure. |
