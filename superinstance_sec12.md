## 12. Appendices

This chapter collects reference material supporting the preceding architectural narrative: a glossary of SuperInstance-specific terms, a consolidated hardware bill of materials, and a survey of related academic and industrial systems.

### 12.1 Glossary

The following table defines ten terms that carry precise, often SuperInstance-specific meanings derived from usage in preceding chapters and ecosystem documentation [^28^].

| Term | Definition | Reference |
|:---|:---|:---|
| **Vessel** | A Decentralized Identifier (DID)-backed service identity owning a bounded repository set and advertising capabilities as JSON-LD. Four Vessels exist: Forgemaster (330 repos, compilation), CCC (116 repos, UI), JetsonClaw1 (76 repos, hardware), Oracle1 (43 repos, coordination). | Chapter 5 |
| **Room** | A bounded context container defining a physical space, a PLATO knowledge space, and a computational fault domain (DDS domain / Raft cluster) simultaneously. Transitions span all three layers atomically. | Chapter 3 |
| **MUD** | Multi-User Dungeon — a spatial environment decomposed into discrete rooms with exits, contents, and triggers. SuperInstance adopts the MUD model as its universal application architecture [^5^]. | Chapter 1 |
| **Reflex** | The fast-path command tier executing safety-critical actions in under 1 ms via the pincher engine, bypassing the LLM pipeline entirely. | Chapter 7 |
| **Deliberative** | The slow-path tier handling non-safety-critical commands through the full STT → LLM → TTS pipeline, completing in 700 ms to 3 s. | Chapter 7 |
| **Pincher** | The Layer 2 reflex engine combining regex matching with vector database search for sub-millisecond response; 57 commits, 76.6% Rust [^9^]. | Chapter 2 |
| **FLUX** | The intermediate representation (IR) for agent cognition at Layer 3. FLUX bytecode compiles intent to constraint programs; the transpiler targets 12 languages [^10^]. | Chapter 2 |
| **VoxelWorks** | A spatial computing framework enabling voxel-based 3D reasoning, integrated with the FLUX compiler for GPU kernel generation. | Chapter 6 |
| **Nebula** | The reflex engine monitoring service tracking pincher activation counts and fast-path statistics via a REST API consumed by CopilotKit fleet tools. | Chapter 6 |
| **CHARTER.md** | An agent repository file defining purpose, constraints, and behavioral boundaries. With `STATE` and `TASK-BOARD`, it constitutes the agent's runtime specification [^13^]. | Chapter 2 |

The naming pattern across these terms reflects three influences: maritime operations (Vessel, Room), jazz improvisation ("groove" consensus, "voice leading" migration), and systems engineering (FLUX, pincher, CHARTER.md).

### 12.2 Bill of Materials

The hardware configuration from Chapter 4 is reproduced with quantities, unit costs, and sourcing notes (mid-2025 distributor averages).

| Component | Model | Qty | Unit Cost | Extended | Notes |
|:---|:---|---:|---:|---:|:---|
| Microcontroller | ESP32-S3-DevKitC-1 | 6 | $12.50 | $75.00 | 240 MHz dual-core, 512 KB SRAM, 8 MB PSRAM [^17^] |
| Microphone | INMP441 | 6 | $2.50 | $15.00 | I2S digital output; one per ESP32 node |
| Edge coordinator | Raspberry Pi 5 8 GB | 1 | $80.00 | $80.00 | Hosts K3s, Mosquitto, whisper.cpp [^48^] |
| AI inference | Jetson Orin Nano 8 GB | 1 | $379.00 | $379.00 | 67 TOPS INT8, 1024 CUDA cores [^28^] |
| Satellite terminal | Starlink Standard | 1 | $599.00 | $599.00 | 100–400 Mbps down, 25–50 ms RTT [^31^] |
| Storage | 512 GB NVMe SSD (Pi HAT) | 1 | $45.00 | $45.00 | Reduces model load from ~18 s to ~3 s [^14^] |
| Cooling | Pi 5 heatsink + fan | 1 | $15.00 | $15.00 | Prevents 20–30% thermal throttling [^36^] |
| Power, enclosure, cables | Assorted | 1 | $75.00 | $75.00 | 12 V marine DC panel, IP65 enclosure, wiring |
| **One-time total** | | | | **$1,283** | |
| Satellite service | Starlink maritime | 1 | $97.50/mo | — | Range: $75–120/month [^31^] |

The one-time outlay of approximately $1,200–1,600 (accounting for Jetson pricing variation from $259 to $499) positions SuperInstance below a single commercial vessel autopilot ($2,000–5,000). The Starlink subscription dominates ongoing costs, exceeding combined annual electricity for all compute tiers (~$131 at continuous operation per Chapter 4) by roughly 7–11×. Six ESP32 nodes assume full-room deployment; smaller vessels may use three nodes (bridge, engine, back deck), cutting $37.50.

### 12.3 Related Work

SuperInstance draws from four lineages: mobile agent platforms defining its mobility patterns; middleware supplying communication substrate; edge-AI systems providing inference pipelines; and marine connectivity enabling the "usually connected" paradigm.

**Mobile agent platforms.** The strong versus weak mobility distinction was established by Aglets (IBM, 1996–2005), Voyager (ObjectSpace, 1997), and the MOA program (Milojicic et al., USENIX COOTS 1998) [^14^]. SuperInstance adopts weak mobility with structured state bundles, following the historical convergence of production platforms toward weak mobility under platform heterogeneity constraints. JAMES (MATA 1999) demonstrated 87% migration speedup via code prefetching, informing WASM preloading during room transitions.

**Self-organising coordination.** Beal et al.'s SCR provides the theoretical basis for room-based consensus with overlapping partitions [^9^]. Li et al.'s SwarmSys (arXiv:2510.10047, 2024) achieves 10.7% higher accuracy than GPTSwarm via pheromone-inspired reinforcement; its explorer/worker/validator roles map to Vessel specialization. MoA-Off (Yang et al., arXiv:2509.16995, 2025) demonstrates 30% latency reduction through per-modality adaptive offloading, providing the edge-cloud LLM routing logic [^15^].

**Consensus and security.** Castro and Liskov's PBFT (OSDI 1999) underlies the ternary voting protocol. Fotiou et al.'s capability-based access control using Verifiable Credentials (SafeThings 2022) binds DID-backed identities to room-scoped permissions [^22^]. Ongaro and Ousterhout's Raft (USENIX ATC 2014) operates within each physical room's trusted cluster.

**Industry systems.** CopilotKit provides the React hooks and AG-UI protocol mapping voice to tool calls [^28^]. Assessed at 9/10 for agentic UI but 5/10 for distributed orchestration, it defines the boundary between supplied functionality and SuperInstance extensions. NVIDIA Isaac provides sensor-fusion patterns, though SuperInstance replaces its centralized orchestration with decentralized Vessel coordination. K3s runs on the Raspberry Pi 5 at 512 MB RAM minimum [^48^]; Ollama manages local LLM loading on the Jetson [^12^]. Starlink's LEO constellation at ~550 km altitude delivers 25–50 ms median RTT, roughly 10× lower than geostationary links [^31^]. This makes SuperInstance, to our knowledge, the first distributed agent system explicitly designed around LEO-satellite latency as a core architectural parameter.

