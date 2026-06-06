## 11. Implementation Roadmap

The preceding chapters defined a voice-controlled, self-assembling distributed system for maritime operation. This chapter converts that architecture into a twelve-month implementation plan, structured around five phases with defined deliverables, measurable exit criteria, and identified risk mitigations. The plan acknowledges four hard constraints from earlier analysis: a single contributor maintains 3,200+ repositories [^1^]; no cross-repo integration test suite exists [^15^]; the CopilotKit fork trails upstream by seven commits [^48^]; and the hardware bill of materials totals approximately $1,200–1,600 one-time plus $75–120 per month for Starlink [^28^].

### 11.1 Development Phases

#### 11.1.1 Phase 1 — Foundation (Months 1–2)

Phase 1 establishes the minimal deployable unit: a single-node voice command pipeline on the Raspberry Pi 5 and Jetson Orin Nano pair, with one ESP32-S3 node in a single room. Engineering focus is on hardening the `construct-coordination` Rust runtime (80 commits [^5^]), implementing the basic room model via PLATO SDK, closing the CopilotKit upstream gap, and creating a CI skeleton for the five load-bearing repositories. The exit criterion is a spoken command captured on the ESP32, processed through whisper.cpp STT at 3.5x real-time [^40^], routed to Llama 3.2 3B on the Jetson at ~28 tok/s [^12^], and answered via Piper TTS within the 3,000 ms P0 ceiling from Chapter 10.

#### 11.1.2 Phase 2 — Mesh (Months 3–4)

Phase 2 expands to multi-node coordination. The three-tier discovery protocol — mDNS for same-room, gossip (SIR epidemic model) for vessel-wide, Kademlia DHT for global — is implemented and stress-tested. ESP32 nodes deploy across all rooms (bridge, engine room, backdeck, hold), each running the 80 KB INT8 wake-word model at 18–22 ms inference [^12^]. The four Vessels (Forgemaster, CCC, JetsonClaw1, Oracle1) register as DID-backed service identities with room-scoped capability tokens [^28^]. Auto-join is verified empirically: 50 cold-boot cycles per tier must show a 90th-percentile join time below 60 seconds. This phase introduces the first cross-repo integration tests — the most significant technical blocker identified in Chapter 2 [^15^].

#### 11.1.3 Phase 3 — Intelligence (Months 5–6)

With the mesh stable, Phase 3 optimizes the natural-language pipeline and implements Starlink-aware LLM routing. The CopilotKit fork is reconciled with upstream, and `SuperInstanceAgent` is hardened for concurrent multi-crew operation. Optimization targets the two dominant latency stages from Chapter 10: VAD endpointing (650 ms to 300 ms via streaming Silero VAD) and LLM generation (speculative TTS, streaming STT overlap). The connectivity state machine (ONLINE → DEGRADED → LOCAL_ONLY → OFFLINE → RECOVERING) is deployed with validated Jetson fallback. Exit criterion: end-to-end voice-to-action below 2,000 ms for the hybrid local-plus-cloud path, measured over 100 sequential command trials.

#### 11.1.4 Phase 4 — Safety (Months 7–8)

Phase 4 certifies the safety-critical reflex tier. The pincher engine (57 commits, <1 ms demonstrated reflex matching [^9^]) undergoes formal validation: 1,000 trigger events measured by logic analyzer at 1 MHz, with zero misses on safety patterns ("stop engine," "emergency alert," "acknowledge alarm"). An A2UI safety dashboard provides real-time visibility into reflex hit rates and fallback-chain activations. Incident response automation triggers reflex-only mode after three consecutive health-check failures. The 24-hour offline autonomy target is validated via an isolated test with simulated command load (one command every five minutes), requiring 100% local service availability.

#### 11.1.5 Phase 5 — Optimization (Months 9–12)

The final phase integrates the full five-layer compilation pipeline — natural language intent through pincher, flux-core bytecode, cuda-oxide PTX, to cudaclaw persistent kernel execution — and tunes for the Chapter 10 benchmarks. TensorRT-LLM targets a 15–25% throughput improvement over llama.cpp baseline [^50^]. The four-month duration reflects the experimental maturity of cudaclaw (six kernels on RTX 4050, no prior Jetson deployment evidence [^12^]) and the need for hardware-in-the-loop testing absent from earlier development.

#### 11.1.6 Phase Deliverables Table

**Table 11.1 — Phase Deliverables, Entry/Exit Criteria, and Dependencies**

| Phase | Duration | Key Deliverables | Entry Criteria | Exit Criteria | Dependencies |
|---|---|---|---|---|---|
| 1 — Foundation | M1–M2 | Rust runtime; room model; single-node voice; CI skeleton | Hardware procured ($1,200–1,600 BOM) [^28^] | Voice-to-action <3,000 ms (100 trials) | None |
| 2 — Mesh | M3–M4 | 3-tier discovery; multi-node coordination; ESP32 per room; Vessel identities | Phase 1 exit criteria met; 4x ESP32 nodes flashed | Auto-join <60 s at 90th percentile (50 cold boots/tier) | Phase 1 |
| 3 — Intelligence | M5–M6 | NL pipeline optimization; Starlink LLM routing; connectivity state machine | Phase 2 exit; Starlink active ($75–120/mo) [^31^] | Voice-to-action <2,000 ms hybrid path | Phase 2 |
| 4 — Safety | M7–M8 | Reflex certification; safety dashboard; incident automation; 24 h offline test | Phase 3 exit; safety patterns in pincher | Reflex <1 ms (1,000 events); 24 h autonomy pass | Phase 3 |
| 5 — Optimization | M9–M12 | Full 5-layer pipeline; TensorRT-LLM tuning; benchmark achievement | Phase 4 exit; JetPack 6.1 on Orin Nano [^52^] | All P0/P1 benchmarks from Chapter 10 passed | Phase 4 |

The phase ordering follows a dependency chain: each phase validates a layer before the next builds upon it. Foundation proves voice through one node; Mesh proves multi-node self-assembly; Intelligence proves latency targets under realistic connectivity; Safety proves the reflex tier and offline modes; Optimization closes the performance gap. Safety certification is placed after Intelligence because the reflex bypass is only meaningful when the deliberative pipeline it shortcuts functions reliably.

![Implementation Roadmap Timeline](fig_11_1_roadmap_timeline.png)

*Figure 11.1 — 12-month implementation roadmap with five phases, quarterly groupings, and milestone markers (M1–M5). Phase durations reflect integration complexity from Chapter 2: Foundation and Mesh are two-month phases; Optimization extends to four months because the full 5-layer compilation pipeline has no prior integration test coverage [^15^].*

### 11.2 Risk Assessment

#### 11.2.1 Technical Risks

The five-layer compilation pipeline is the highest technical risk. The stack (`open-parallel` → `pincher` → `flux-core` → `cuda-oxide` → `cudaclaw`) has no validated end-to-end path; Chapter 2 found no automated test that natural language intent traverses all five layers and executes on GPU hardware [^15^]. Latency feasibility is a second risk: the 500 ms gap identified in Chapter 10 concentrates in VAD endpointing (+150 ms) and LLM generation (+350 ms), requiring streaming implementations not yet in the codebase. The single-maintainer bottleneck is structural — one contributor across 3,200+ repositories yields a bus factor of one [^1^].

#### 11.2.2 Integration Risks

The CopilotKit fork (seven commits behind upstream [^48^]) risks unmergeability if upstream introduces breaking AG-UI protocol changes. LLM API costs scale with fleet size: at one command per five minutes, a single vessel generates 288 API calls daily; multi-vessel deployments could exceed the $75–120 monthly Starlink subscription in API spend alone. Starlink availability depends on environmental conditions — heavy rain and high-latitude gaps can degrade the 25–50 ms median RTT for hours [^31^].

#### 11.2.3 Operational Risks

The capability-based security model with DID-backed Vessel identities [^28^] has been specified but not penetration-tested; a compromised token could expose repository-scope capabilities. Consumer hardware (RPi 5, Jetson Orin Nano) is not marine-rated, and salt air, vibration, and thermal cycling present failure modes untested in the existing codebase. Documentation fragmentation — 1,500,000+ words across 2,000 repositories without unified indexing [^3^] — makes incident response dependent on the single maintainer's institutional knowledge.

#### 11.2.4 Risk Register

**Table 11.2 — Risk Register**

| Risk ID | Description | Likelihood | Impact | Mitigation Strategy | Owner |
|---|---|---|---|---|---|
| R-TECH-01 | 5-layer pipeline fails integration; no e2e intent-to-kernel path | Medium | Critical | Phase 1 CI validates layer pairs; Phase 5 extends to full pipeline; fallback to deliberative-only mode | Architecture lead |
| R-TECH-02 | Voice-to-action cannot close 500 ms gap in VAD + LLM | Medium | High | Streaming VAD (−150 ms); local Jetson routing (−350 ms); P0 ceiling of 3,000 ms allows 52% headroom | Pipeline engineer |
| R-TECH-03 | Single maintainer unavailable >2 weeks; bus factor of one | Medium | Critical | Document decisions in-repo via git-agent lifecycle [^13^]; establish second contributor in Phases 1–2 | Project coordinator |
| R-INT-01 | CopilotKit upstream breaking change strands fleet copilot | Medium | High | Sync fork within 2 weeks of each upstream release; pin runtime deps; maintain hook regression tests | Frontend engineer |
| R-INT-02 | LLM API costs exceed budget at multi-vessel scale | Medium | Medium | Route simple commands to local Jetson (~28 tok/s [^12^]); cache 50 common responses; per-vessel rate limiting | Operations |
| R-INT-03 | Starlink outage exceeds 24-hour offline autonomy window | Low | High | 2,100 Wh LiFePO4 battery sustains 24+ h compute [^13^]; reflex-only mode; command queue for replay | Marine engineer |
| R-OPS-01 | Capability token compromise exposes Vessel permissions | Low | Critical | 1-hour token expiry; room-scoped capabilities limit blast radius [^28^]; gossip revocation; offline `did:key` validation | Security engineer |
| R-OPS-02 | Consumer hardware fails in marine environment | Medium | High | Conformal coating; active cooling (mandatory, prevents 20–30% thermal throttle [^36^]); sealed enclosures; spares aboard | Hardware engineer |
| R-OPS-03 | Documentation fragmentation blocks contributor onboarding | High | Medium | Unified index from CATALOG.md [^2^]; in-repo decision records required; automated doc-link CI check | Documentation lead |

The three Critical-impact risks (R-TECH-01, R-TECH-03, R-OPS-01) are front-loaded into Phases 1–2 because early detection prevents cascading failure. If the compilation pipeline cannot integrate, the architecture pivots to deliberative-only mode before Phase 5 investments are made. If the maintainer becomes unavailable, the Phase 1–2 pair-programming window is the only opportunity to transfer institutional knowledge.

### 11.3 Validation Strategy

#### 11.3.1 Per-Phase Acceptance Criteria

Each phase in Table 11.1 defines exit criteria that are measurable and binary. "Voice feels fast enough" is not an exit criterion; "median voice-to-action below 2,000 ms over 100 sequential trials with 95th percentile below 3,000 ms" is. This quantitative rigor is necessary because the single-contributor constraint removes peer-review safety nets — automated gates are the only reliable quality mechanism [^1^]. The criteria also serve as regression detectors: if a Phase 3 optimization causes Phase 2 auto-join timing to degrade, the CI skeleton catches it before deployment.

#### 11.3.2 End-to-End Test Scenario

The definitive validation is a full voice-to-action scenario on the vessel: "Set autopilot to course 270, speed 8 knots." This exercises the complete pipeline — ESP32 wake-word detection (18–22 ms [^12^]), audio streaming, STT via whisper.cpp (300 ms [^40^]), context assembly with room state and Vessel capabilities, LLM tool-call generation (cloud via Starlink at 25–50 ms RTT [^31^] or local Jetson), action execution against the NMEA 0183 autopilot interface [^28^], and TTS confirmation. The test runs under three connectivity profiles (ONLINE, DEGRADED, LOCAL_ONLY); all profiles must produce a correct command within the 3,000 ms P0 ceiling, with the reflex tier intercepting safety overrides ("cancel autopilot") in under one millisecond.

#### 11.3.3 Continuous Benchmarking

Performance regression detection runs automatically on every commit to the five load-bearing repositories. The CI workflow executes a standardized matrix: voice-to-action latency (100 trials), reflex response (1,000 trigger events), auto-join duration (50 cold boots), and Jetson inference throughput. Results are compared against Phase baselines — any commit increasing median latency by more than 10% or decreasing throughput by more than 5% fails the build. This addresses the maturity gap from Chapter 2 by making performance a first-class citizen of development [^15^], with benchmark artifacts serving as living documentation more precise than any prose description.
