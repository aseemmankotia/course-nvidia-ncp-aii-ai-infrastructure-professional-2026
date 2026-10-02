## Chapter 5 Cheat Sheet — Cabling, Transceivers, Fabric Topology

### Cable/Transceiver Type by Distance (NDR 400Gb/s reference)
| Type | Typical Max Reach | Cost | Power | Notes |
|---|---|---|---|---|
| Passive DAC | ~2-3 m | Lowest | Lowest | No active components; in-rack use only |
| Active Copper Cable (ACC) | ~3.5-5 m | Low-Med | Med | Extends copper reach with signal amplification |
| AOC (Active Optical Cable) | ~50 m (up to ~100 m at lower speeds) | Med | Med | Fixed optics both ends, no separate patch fiber |
| Multimode transceiver (SR) + OM3/4/5 fiber | ~30-100 m | Med | Med | Row-to-row DC links |
| Single-mode transceiver (DR) + SMF | ~500 m | High | Med-High | Adjacent building/long row |
| Single-mode transceiver (FR) + SMF | ~2 km | High | Med-High | Campus-scale |
| Single-mode transceiver (LR) + SMF | ~10 km | Highest | High | Metro/long campus |

### Selection Rule of Thumb
Pick the **shortest-reach, lowest-cost** option that still covers the required distance:
DAC → ACC → AOC → Multimode transceiver → Single-mode (DR/FR/LR)

### Connector Form Factors
| Generation | Speed | Common Connector |
|---|---|---|
| HDR | 200 Gb/s | QSFP56 |
| NDR | 400 Gb/s | OSFP / QSFP-DD |
| XDR | 800 Gb/s | OSFP |

### Topology Comparison
| Topology | Purpose | Key Trait |
|---|---|---|
| Fat-tree (Clos) | General scalable any-to-any fabric | Non-blocking hierarchy via spine layer |
| Rail-optimized | GPU backend east-west traffic | Same-rank GPU NICs share a leaf/rail switch, minimizing hops for collectives |

### Oversubscription Ratio
**Formula:** Downlink BW : Uplink BW
Example: 32 downlinks : 8 uplinks (equal speed) = **4:1**

| Fabric Type | Typical Ratio |
|---|---|
| GPU compute/backend fabric | 1:1 (non-blocking) |
| Storage / frontend network | 2:1 to 4:1 (cost-optimized) |

### Exam Traps
- Confusing AOC (fixed optics + cable) with a discrete pluggable transceiver + separate patch fiber (these are different SKUs/architectures).
- Assuming passive DAC can be used for any inter-row/building link — it cannot, distance is too short.
- Forgetting that GPU compute fabrics should stay near 1:1 oversubscription even when cost pressure suggests otherwise.
- Mixing up rail-optimized (traffic-pattern/hop optimization) with oversubscription ratio (bandwidth-ratio concept) — they are independent design levers.
