## Chapter 5: Cabling Standards, Transceivers, and Fabric Topology — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Max supported length for passive copper DAC at InfiniBand NDR (400Gb/s)? | ~2 meters. Beyond this, Active Copper Cable (ACC) or AOC/optics are required. Exam tip: passive DAC = cheapest, lowest power, shortest reach. |
| 2 | Typical max reach of an AOC (Active Optical Cable) at NDR speeds? | ~50 meters (some lower-speed AOC variants reach up to 100m). Exam tip: AOC has fixed optics on both ends — no separate transceiver or patch cable needed. |
| 3 | Reach of single-mode DR vs FR optical transceivers? | DR ~500m, FR ~2km. Exam tip: choose single-mode for building-to-building or long inter-row runs. |
| 4 | Reach of a single-mode LR transceiver? | ~10 km. Exam tip: LR = longest reach single-mode option in this family, highest cost, used for campus/metro-scale links. |
| 5 | Typical reach of multimode SR optical transceivers? | ~30-100m depending on fiber grade (OM3/OM4/OM5) and link speed. Exam tip: cheaper than single-mode optics but shorter reach; common for row-to-row DC links. |
| 6 | What is an Active Copper Cable (ACC) used for? | Extends copper reach beyond passive DAC limits (roughly 3.5-5m) via built-in signal amplification. Exam tip: pick ACC when passive DAC can't reach but optics aren't yet justified. |
| 7 | What defines a rail-optimized topology? | Connects same-rank GPU NICs across nodes to a common leaf/rail switch, minimizing hops for GPU collective traffic (all-reduce/all-to-all). Exam tip: used in NVIDIA SuperPOD-style backend fabrics. |
| 8 | What defines a fat-tree (Clos-based) topology? | Non-blocking hierarchical design providing uniform any-to-any bandwidth between all leaf switches via a spine layer. Exam tip: default scalable choice for HPC/AI backend fabrics. |
| 9 | How is oversubscription ratio calculated on a leaf switch? | Downlink bandwidth : Uplink bandwidth. Example: 32 downlinks : 8 uplinks (equal speed) = 4:1. Exam tip: 1:1 means non-blocking. |
| 10 | Recommended oversubscription ratio for a GPU training backend fabric? | 1:1 (non-blocking) — required to avoid degrading NCCL collective communication performance. Exam tip: storage/frontend networks can tolerate 3:1 or 4:1 to save cost. |
| 11 | DAC vs AOC vs discrete transceiver+fiber — how to choose by distance? | DAC: up to ~2-3m, cheapest/lowest power. AOC: ~3-100m, integrated fixed optics. Discrete transceiver+fiber: >100m or when independent reuse of fiber/optics is needed. Exam tip: match shortest viable option to the given distance/cost constraint. |
| 12 | Common connector form factors for NDR (400Gb/s) and XDR (800Gb/s) InfiniBand ports? | OSFP or QSFP-DD. Exam tip: these differ from the QSFP28 form factor common at older/lower InfiniBand and Ethernet speeds. |
