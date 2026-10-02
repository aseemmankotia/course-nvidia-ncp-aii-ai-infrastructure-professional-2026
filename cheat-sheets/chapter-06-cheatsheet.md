## Chapter 6 Cheat Sheet — Power, Cooling, Link Validation, Signal Monitoring

### Rack Power Budget
| Item | Key Fact |
|---|---|
| DGX H100/H200 max draw | ~10.2 kW per system |
| Redundancy model | A/B dual-corded feeds — EACH PDU sized for 100% load, not half |
| N+1 meaning | One extra active unit beyond minimum required (N); NOT a cold spare in storage |
| Budget planning | Account for PDU circuit derating, redundancy overhead, peak transient draw — not just steady-state average |

### Cooling: Air vs Liquid
| Cooling Type | When Used |
|---|---|
| Air (CRAC, hot/cold aisle) | Standard/lower density racks |
| Liquid (direct-to-chip, rear-door heat exchanger) | High-density multi-DGX racks exceeding air-cooling heat removal capacity |
| Common trap | Assuming more fans/CRAC airflow scales indefinitely — it doesn't at high density |

### Link Validation & BER
| Concept | Value/Note |
|---|---|
| BER pass/fail threshold | 1e-12 commonly used for cable/link certification |
| Test approach | Statistical BER over a test window, not single-bit-error fail |
| Applies to | Both copper (DAC/ACC) and optical links |

### Physical-Layer Monitoring
| Counter | Meaning |
|---|---|
| Symbol error | Corruption at encoding/signaling level (link training/transmission) |
| CRC error | Frame received but checksum failed (post-signal-recovery corruption) |
| Tool | `ibqueryerrors` / `perfquery` — InfiniBand port error counters |
| Diagnostic tool | `mlxlink` or `ethtool -m` — transceiver DDM (Tx/Rx power, temp, voltage) |
| Trap | Rising CRC with zero symbol errors → suspect cable/transceiver, not config |

### Thermal Monitoring
| Threshold | Behavior |
|---|---|
| Slowdown/throttle threshold | GPU reduces clocks to cut heat, stays operational |
| Shutdown threshold (higher) | GPU powers off to protect hardware |
| Trap | Confusing throttling (still running, degraded perf) with shutdown (offline) |

### Quick Command Reference
| Task | Command |
|---|---|
| IB fabric error scan | `ibqueryerrors -k` |
| Per-port counters | `perfquery` |
| Transceiver DDM diag | `mlxlink` / `ethtool -m <iface>` |
| GPU thermal check | `nvidia-smi -q -d TEMPERATURE` |

### Top Gotchas
1. PDU sized for half-load per feed = NOT true redundancy — must be 100% each.
2. Cold spare ≠ N+1 — must be active and automatic.
3. Throttling is not the same as shutdown — know both thresholds separately.
