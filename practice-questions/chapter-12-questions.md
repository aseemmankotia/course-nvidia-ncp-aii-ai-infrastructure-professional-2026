## Chapter 12: Full Exam Simulation and Time-Management Strategy — Practice Questions

**Q1.** The NCP-AII exam has 70 questions in a 120-minute window. You are 75 minutes into the exam and have answered 50 questions, with 8 flagged for review. What should you do next?

A) Immediately go back and resolve all 8 flagged questions before continuing forward
B) Continue answering the remaining 20 new questions first, then return to flagged items with whatever time remains
C) Skip the remaining new questions entirely and spend all remaining time perfecting the 8 flagged answers
D) Submit the exam now since you have answered more than half the questions

<details>
<summary>Answer</summary>

**Correct: B**

Every unanswered question is a guaranteed zero, while a flagged question already has a submitted answer that may already be correct. Securing points on the 20 remaining new questions protects your score first; flagged review happens only if time allows.

**Why the others are wrong:**
- Diverting to flagged review now risks running out of time before reaching the remaining 20 unanswered questions, which is a worse outcome
- Ignoring new questions guarantees zero credit on 20 questions — far riskier than revisiting a handful of already-answered flagged items
- Submitting early forfeits the ~45 minutes remaining and the chance to review flagged answers or answer un-attempted questions

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q2.** Based on a 70-question, 120-minute exam, what is the raw average time budget per question before any review buffer is subtracted?

A) Approximately 103 seconds per question
B) Approximately 60 seconds per question
C) Approximately 150 seconds per question
D) Approximately 45 seconds per question

<details>
<summary>Answer</summary>

**Correct: A**

120 minutes = 7200 seconds; 7200 / 70 ≈ 102.9 seconds per question, the raw arithmetic average before reserving buffer time for a final review pass.

**Why the others are wrong:**
- 60 seconds/question would only account for 70 minutes total, leaving 50 minutes unaccounted for
- 150 seconds/question would require 175 minutes total, exceeding the exam length
- 45 seconds/question would only use ~52 minutes of the available 120

</details>

---

**Q3.** A candidate wants to reserve time at the end of the exam for a final review pass. Which pacing plan best balances answering all questions with leaving a review buffer?

A) Spend up to 3 minutes on every question to ensure accuracy, with no reserved review time
B) Answer as fast as possible in under 30 seconds per question so 60 minutes remain fully open for review
C) Target ~90 seconds per question on the first pass, reserving the remaining ~10-15 minutes for a final review of flagged items
D) Split the exam exactly in half: 60 minutes for the first 35 questions, 60 minutes for the last 35

<details>
<summary>Answer</summary>

**Correct: C**

Budgeting ~90 seconds per question on the first pass (below the ~103-second raw average) creates a natural ~10-15 minute buffer at the end for reviewing flagged questions without risking incompletion.

**Why the others are wrong:**
- 3 minutes/question for all 70 questions requires 210 minutes — far beyond the 120-minute limit
- 30 seconds/question sacrifices accuracy on scenario-heavy questions that need careful reading, risking careless errors
- An even 60/60 split ignores that later scenario questions are often harder and cross-domain, needing more time, not equal time

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q4.** A question describes a cluster requiring the lowest possible latency and lossless, RDMA-capable fabric using dedicated switches, with no requirement to interoperate with existing standard Ethernet infrastructure. Which fabric best fits this requirement?

A) InfiniBand
B) RoCE (RDMA over Converged Ethernet)
C) Standard TCP/IP over Ethernet
D) Fibre Channel

<details>
<summary>Answer</summary>

**Correct: A**

InfiniBand is purpose-built for the lowest latency and native lossless RDMA without needing Ethernet compatibility, making it the best fit when there is no requirement to interoperate with existing Ethernet gear.

**Why the others are wrong:**
- RoCE is the correct choice specifically when Ethernet interoperability or existing Ethernet switches are required — the scenario explicitly says that's not a constraint here
- Standard TCP/IP over Ethernet lacks RDMA and has materially higher latency, unsuitable for tightly-coupled GPU workloads
- Fibre Channel is a storage-area-network protocol, not an RDMA compute fabric used for GPU-to-GPU communication

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q5.** A team manages a growing GPU cluster and currently provisions nodes with individual shell scripts, with no centralized health monitoring or automated node re-imaging. Which approach BEST addresses this as the cluster scales?

A) Continue manual scripting but add more engineers to run scripts faster
B) Replace all nodes with cloud-only instances to eliminate management overhead
C) Use a spreadsheet to track node state changes manually
D) Adopt Base Command Manager (BCM) for centralized provisioning, monitoring, and lifecycle management

<details>
<summary>Answer</summary>

**Correct: D**

BCM is designed to centralize provisioning, health monitoring, and re-imaging of GPU clusters at scale, directly solving the stated pain points of manual scripts with no centralized visibility.

**Why the others are wrong:**
- Adding more engineers to manual scripting does not solve the lack of centralized monitoring or automated re-imaging — it only adds labor cost
- Moving to cloud-only instances doesn't inherently solve provisioning/monitoring gaps and may not be feasible for on-prem GPU investments
- A spreadsheet is a manual tracking method, not automated management, and does not scale or provide real-time health data

</details>

---

**Q6.** A workload requires gang-scheduled, tightly-coupled multi-node HPC training jobs with fine-grained control over job queues, priorities, and exclusive node allocation typical of traditional HPC centers. Which orchestrator is the better fit compared to a general container orchestration platform?

A) Slurm, because it provides native gang scheduling and HPC-style job queue management
B) Kubernetes, because it is the industry standard for all workloads
C) Neither — orchestration is unnecessary for GPU clusters
D) Kubernetes with no additional scheduler plugins, since default bin-packing is sufficient for gang scheduling

<details>
<summary>Answer</summary>

**Correct: A**

Slurm was purpose-built for HPC-style batch scheduling, including native gang scheduling and exclusive node allocation, which is the exact requirement described.

**Why the others are wrong:**
- Kubernetes excels at microservices/inference workloads but requires significant additional configuration (e.g., plugins) to approximate HPC-style gang scheduling, making it not the default best fit here
- Orchestration is necessary at scale to prevent resource contention and ensure job fairness; skipping it is not viable
- Kubernetes' default scheduler does not provide gang scheduling out of the box — this is a common exam trap distractor

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q7.** You encounter a long scenario question mentioning storage tiering, network fabric choice, and cluster orchestration all in one paragraph, and you are unsure which single domain it's testing. What is the BEST strategy?

A) Assume it is a networking question since networking is mentioned first
B) Flag it immediately without reading further since it spans multiple domains
C) Identify the specific decision the question is actually asking you to make, then match that decision to its primary domain, ignoring the other domain details as context
D) Choose the answer option that mentions the most domains at once, since it must be the most complete

<details>
<summary>Answer</summary>

**Correct: C**

Cross-domain scenarios provide multi-domain context but almost always test one specific decision point (e.g., which fabric, which storage tier, which scheduler). Identifying the actual question being asked filters out irrelevant context.

**Why the others are wrong:**
- The order in which domains are mentioned in the paragraph has no bearing on which domain the actual question is testing
- Flagging without reading wastes time and doesn't help since the question still needs to be answered eventually
- The most 'complete-sounding' option is a classic distractor pattern — correct answers are precise and scoped, not the broadest-sounding choice

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

**Q8.** With 10 minutes remaining and all 70 questions answered, including 6 flagged items, what is the BEST use of remaining time?

A) Re-read and second-guess every single answered question from question 1 onward
B) Review only the 6 flagged questions, focusing on ones where you were genuinely uncertain, and leave confident answers unchanged
C) Immediately submit without reviewing anything since all questions are answered
D) Change answers on questions where you now have a 'gut feeling' something is different, even if you were originally confident

<details>
<summary>Answer</summary>

**Correct: B**

With limited time, targeted review of genuinely uncertain flagged items yields the highest expected point gain; broad re-reading or second-guessing confident answers wastes time and can introduce errors.

**Why the others are wrong:**
- Re-reading all 70 questions in 10 minutes is not feasible (under 9 seconds/question) and risks running out of time mid-review
- Skipping review entirely forfeits a chance to catch genuine errors on flagged questions, especially since time remains
- Changing confident answers based on a vague 'gut feeling' without new information is a well-documented pattern that lowers scores more often than it helps

> ⚠️ Frequently missed on the exam — read the options carefully.

</details>

---

