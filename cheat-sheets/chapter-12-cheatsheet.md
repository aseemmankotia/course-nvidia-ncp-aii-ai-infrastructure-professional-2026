# Chapter 12 Cheat Sheet: Full Exam Simulation & Time Management

## Exam Format Numbers
| Item | Value |
|---|---|
| Total questions | 70 |
| Total time | 120 minutes (7200 sec) |
| Raw average/question | ~103 sec |
| Recommended first-pass budget | ~90 sec/question |
| Reserved review buffer | ~10-15 minutes |

## Pacing Checkpoints
| Time Elapsed | Questions You Should Have Answered |
|---|---|
| 30 min | ~20 |
| 60 min | ~40 |
| 90 min | ~60 |
| 105 min | 70 (all answered, entering review) |

If you're behind a checkpoint by more than 5 questions, stop reading extra context sentences and move to the core ask of each question faster.

## Flag-and-Skip Rules
1. Never leave a question blank — always submit a best guess first.
2. Flag only when you've eliminated ≥1 option but are torn between the rest.
3. Move on immediately after flagging — don't linger.
4. Review flagged items only after ALL 70 are answered.

## Terminology Traps (Cross-Domain)
| Trap Pair | Correct Distinction |
|---|---|
| InfiniBand vs RoCE | InfiniBand = lowest latency, dedicated fabric, no Ethernet interop needed. RoCE = RDMA over Ethernet, chosen for Ethernet interoperability/reuse of existing switches |
| BCM vs manual scripts | BCM = centralized provisioning/monitoring/re-imaging at scale. Manual scripts = no drift detection, doesn't scale |
| Kubernetes vs Slurm | Slurm = gang scheduling, tightly-coupled HPC batch jobs, exclusive allocation. Kubernetes = elastic microservices/inference, NOT native gang scheduling by default |

## Elimination Strategy
1. Cut options that violate a hard constraint stated in the scenario (cost, latency, deprecated service).
2. Cut options that solve only part of the requirement.
3. Compare remaining 2 options against the SINGLE decision being tested.
4. Beware: the option mentioning the most services/domains is usually a distractor, not the 'most complete' answer.

## Cross-Domain Scenario Recognition
- Multi-domain context paragraph → find the ONE specific decision asked (fabric? scheduler? tier?).
- Ignore domain details not tied to that decision.
- Don't assume the first-mentioned domain is the tested one.

## Final Review Pass (Last 10-15 Minutes)
1. Confirm all 70 questions are answered — zero blanks.
2. Revisit only flagged questions, prioritizing genuine uncertainty.
3. Do NOT re-read confidently answered questions.
4. Do NOT change answers based on a vague feeling — only change if you spot a factual reading error.
5. Submit once flagged review is exhausted or time expires.
