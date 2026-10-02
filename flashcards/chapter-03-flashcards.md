## Chapter 3: Rack, Cable, and Configure Compute Servers — Flashcards

| # | Front (Question) | Back (Answer) |
|---|-----------------|---------------|
| 1 | Correct install order for a DGX-class node before running containerized GPU workloads? | OS (DGX OS) → NVIDIA driver → container engine (Docker/containerd) → NVIDIA Container Toolkit → configure runtime. Exam tip: toolkit must come LAST, after both driver and engine exist. |
| 2 | Command to configure Docker to use the NVIDIA container runtime? | nvidia-ctk runtime configure --runtime=docker, then restart the Docker daemon (systemctl restart docker). Exam tip: this step comes after the toolkit is installed, not before. |
| 3 | Standard airflow direction for DGX/NVIDIA-Certified rack servers? | Front-to-back. Cold aisle = front (intake), hot aisle = rear (exhaust). Exam tip: reversing this causes hot exhaust recirculation and thermal throttling. |
| 4 | DGX OS vs standard Ubuntu — which do you use on a DGX system joining a validated cluster? | DGX OS — it bundles NVIDIA-tested, version-pinned kernel, driver, and networking (OFED) stack for that hardware generation, reducing cross-node compatibility risk. |
| 5 | Correct docker command to authenticate to NGC's registry? | docker login nvcr.io -u '$oauthtoken' -p <NGC_API_KEY>. Exam tip: username is literally the string $oauthtoken, not your NGC account name. |
| 6 | What does NGC (nvcr.io) provide? | A registry of GPU-optimized, security-scanned containers (frameworks, HPC apps, pretrained models) from NVIDIA and partners. Not a hardware component. |
| 7 | Best-practice PDU cabling for a server with dual redundant PSUs? | Each PSU goes to a separate PDU on an independent electrical circuit (A/B feed). Exam tip: same-PDU cabling defeats PSU redundancy entirely. |
| 8 | Node compatibility checklist item that most often causes intermittent NCCL job failures in a multi-node cluster? | Mismatched driver, CUDA toolkit, or NIC/OFED firmware versions across nodes. Exam tip: always verify version parity across ALL nodes before running distributed jobs. |
| 9 | Command to verify GPU topology (NVLink/PCIe paths) on a multi-GPU server? | nvidia-smi topo -m. Exam tip: used to confirm the expected interconnect topology matches hardware documentation before cluster validation. |
| 10 | What must be true before the NVIDIA Container Toolkit configuration step will succeed? | The base NVIDIA driver AND a container engine (Docker or containerd) must already be installed and running. Exam tip: toolkit depends on both, installed in that order. |
| 11 | Rack unit planning trap the exam tests beyond simple U-height counting? | Per-rack power draw and cooling capacity — a rack can run out of power/cooling budget before running out of physical U space. |
| 12 | How do you confirm a container can see the GPU after toolkit setup? | Run a test container with GPU access enabled and check for GPU listing inside it (e.g., via the container engine's GPU flag plus nvidia-smi executed inside the container). Exam tip: failure here usually traces back to install-order or runtime-config mistakes. |
