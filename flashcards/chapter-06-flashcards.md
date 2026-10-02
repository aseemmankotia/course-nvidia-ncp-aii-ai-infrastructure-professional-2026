## Chapter 6: Power, Cooling, Link Validation, and Signal Monitoring — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | DGX H100 system max power draw? | ~10.2 kW per system. Exam tip: use this figure for rack power budget calculations, not per-GPU TDP alone. |
| 2 | What BER value is commonly used as the pass/fail threshold during physical-layer link cable validation? | 1e-12. A measured BER worse than this fails validation and indicates a marginal/faulty cable or transceiver. |
| 3 | Air cooling vs liquid cooling — when do you need liquid cooling for DGX racks? | Once rack heat density exceeds what standard hot/cold aisle air cooling can remove (typically high-density multi-DGX racks in the tens of kW), use direct-to-chip or rear-door heat exchanger liquid cooling instead of air alone. |
| 4 | Symbol error vs CRC error on a network port — what's the difference? | Symbol error = corruption detected at the physical encoding/signaling level (link training/transmission). CRC error = frame was received but failed its checksum. Exam tip: both indicate physical-layer issues but at different pipeline stages. |
| 5 | Command to check InfiniBand port error counters (CRC, symbol errors)? | ibqueryerrors (also perfquery for per-port counters). Exam tip: this is the go-to tool for fabric-wide error scanning. |
| 6 | Command/tool to read transceiver DDM diagnostics (Tx/Rx power, temp, voltage)? | mlxlink or ethtool -m <interface>. Exam tip: used to confirm whether a failing link is caused by a bad optic/cable versus a config issue. |
| 7 | What does N+1 PDU redundancy mean? | One additional active unit/feed beyond the minimum (N) required to carry full load, so a single failure causes zero downtime — NOT a cold spare in storage. |
| 8 | What is dual-corded A/B feed redundancy? | Each server has two power supplies, each connected to an independent PDU (A and B). Each PDU alone must be sized to carry the full load, so losing either feed causes no outage. |
| 9 | GPU thermal slowdown threshold vs shutdown threshold — what's the behavioral difference? | At the slowdown threshold, the GPU throttles clocks to reduce heat while staying operational. At the (higher) shutdown threshold, the GPU powers off to prevent hardware damage. Exam tip: throttling ≠ shutdown. |
| 10 | What does a rising CRC error counter with zero symbol errors usually indicate? | Likely a marginal/faulty cable, dirty connector, or degrading transceiver corrupting frames after the signal is otherwise successfully decoded. |
| 11 | Rack power budget planning — what must you account for beyond nameplate/max draw? | PDU derating/circuit capacity, redundancy overhead (N+1 or A/B full-load sizing), and headroom for peak transient draw — not just steady-state average power. |
| 12 | Low Rx optical power reading on a transceiver — what does it suggest? | Rx power approaching or below the receiver sensitivity threshold suggests a degraded link — check for dirty/damaged fiber connectors, excessive cable length/loss, or a failing transceiver. |
