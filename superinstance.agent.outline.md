# SuperInstance Architecture: A Voice-Controlled, Self-Assembling Distributed System for Heterogeneous Edge Hardware

## Executive Summary (~800 words, 2 tables)
### Document Purpose and Scope
#### Architecture document mapping the complete SuperInstance-copilot platform integrating 4,095+ repos (as of 2026-07-10) into a unified distributed system
#### Scope boundaries: covers system architecture, communication protocols, hardware tiers, agent model, safety systems, voice pipeline, and implementation roadmap; excludes installation guides, API reference docs, and operational playbooks
#### Intended audience: systems architects, edge AI engineers, distributed systems researchers, marine technology integrators
### System at a Glance
#### Voice-controlled distributed system where agents navigate "rooms" (physical, virtual, computational) across ESP32-to-Cloud hardware tiers, coordinated by CopilotKit natural-language interface
#### Architecture principle summary table: 5 principles and their system manifestations
#### Hardware tier quick-reference table: 4 tiers with device, compute, power, latency, cost
### Success Criteria
#### Performance targets: voice-to-action under 3 seconds, reflex under 700ms, auto-join under 60 seconds, 24-hour offline autonomy
#### Quality attributes priority: safety > availability > latency > throughput > cost
#### Constraints: single maintainer, 4,095-repo (as of 2026-07-10) ecosystem, maritime environment, Starlink dependency

## 1. System Overview and Architecture Philosophy (~1,500 words, 1 table, 1 diagram)
### 1.1 Design Philosophy
#### 1.1.1 "Every app is a MUD" — software decomposed into agent-navigable rooms with context, boundaries, and capabilities
#### 1.1.2 Music cognition as mathematical coordination substrate: jazz groove as consensus mechanism, voice leading as migration protocol, timing advantage of 2.46x over turn-based coordination
#### 1.1.3 The three-room convergence: physical spaces (bridge, engine room), virtual knowledge spaces (PLATO), and computational zones (DDS domains) operate as a single navigable namespace
#### 1.1.4 "Usually connected" assumption: Starlink at 25-50ms RTT inverts edge-first design — connected-with-degradation, not offline-first
### 1.2 Architectural Principles
#### 1.2.1 Rooms are the universal context container — every piece of state lives in a room, every computation runs in a room, every capability is scoped to a room
#### 1.2.2 Vessels are self-sovereign identities — four specialized agents (Forgemaster, CCC, JetsonClaw1, Oracle1) each with DID-backed identity and capability advertisement
#### 1.2.3 Reflex precedes deliberation — safety-critical commands execute in <1ms via pincher reflex engine, bypassing the LLM pipeline entirely
#### 1.2.4 Self-assembly is default — nodes auto-discover, advertise capabilities, and bind to the mesh without manual configuration within 60 seconds
#### 1.2.5 Natural language is the primary control plane — every hardware capability exposed as a CopilotKit tool schema, making voice the universal API
#### 1.2.6 Principles-to-decisions mapping table: each principle linked to concrete architectural choices
### 1.3 System Context
#### 1.3.1 High-level component diagram: voice input → CopilotKit UI → 5-layer Rust stack → four hardware tiers → Starlink cloud LLM, with Vessels coordinating
#### 1.3.2 External interfaces: Starlink terminal (WAN), cloud LLM APIs (OpenAI/Anthropic), marine sensors (NMEA 2000, sonar), physical actuators (autopilot, throttle, cameras)
#### 1.3.3 Request lifecycle: voice capture → STT → intent classification → context assembly → LLM routing → tool execution → hardware action → response synthesis → TTS

## 2. The Ecosystem (~2,000 words, 3 tables, 2 diagrams)
### 2.1 Ecosystem Scale
#### 2.1.1 4,095+ repositories (2,000 cataloged as of 2026-06-06), 6,000+ tests, 373,639+ lines of Rust (as of 2026-07-10), 1,500,000+ words of documentation
#### 2.1.2 Four Vessel identities: Forgemaster (330 repos, constraint theory/math), CCC (116 repos, web UI), JetsonClaw1 (76 repos, hardware/edge), Oracle1 (43 repos, infrastructure)
#### 2.1.3 Published packages: PyPI 35+, crates.io 24+, npm 18+ — ecosystem metrics table
### 2.2 The Five-Layer Stack
#### 2.2.1 Layer 1 — open-parallel: ternary math {-1, 0, +1}, 306 crates, 16x GPU memory bandwidth savings, Eisenstein lattice operations
#### 2.2.2 Layer 2 — pincher: reflex engine <1ms response, vector DB as runtime, LLM as compiler, 57 commits, Rust 76.6%
#### 2.2.3 Layer 3 — flux-core: agent cognition as bytecode IR, 12-language transpiler, constraint compiler
#### 2.2.4 Layer 4 — cuda-oxide: Flux→MIR→PTX GPU compiler pipeline, distributed GPU runtime
#### 2.2.5 Layer 5 — cudaclaw: persistent CUDA kernel deployment, 6 kernels demonstrated on RTX 4050
#### 2.2.6 Layer transformation table: input format, transformation, output format, target hardware per layer
### 2.3 Git-Agent Lifecycle
#### 2.3.1 Lifecycle phases: PULL → BOOT → WORK → LEARN → PUSH → SLEEP → PULL with git as persistent state store
#### 2.3.2 CHARTER.md, STATE, and TASK-BOARD as agent runtime state files
#### 2.3.3 Diary and bottle messages as agent-to-agent communication mechanism
### 2.4 Integration Maturity and Gaps
#### 2.4.1 Maturity assessment: tight integration (constraint-theory bindings), moderate (PLATO SDK), loose (many educational/experimental repos)
#### 2.4.2 Critical gaps: no stable GitHub releases, documentation fragmentation, single contributor, no cross-repo integration test suite
#### 2.4.3 Dependency graph: core chain open-parallel → pincher → flux-* → cuda-oxide → cudaclaw with cross-links to plato-sdk and cocapn-*

## 3. The Room Model (~2,000 words, 1 table, 2 diagrams)
### 3.1 Room Taxonomy
#### 3.1.1 Room definition: encapsulated context with boundaries, state, capabilities, and agent population
#### 3.1.2 Physical rooms: bridge, engine room, cabin, back deck, galley, crow's nest — mapped to hardware placement and sensor/actuator sets
#### 3.1.3 Virtual rooms: PLATO knowledge spaces where agents deliberate, spectral graph knowledge representation, failure-first reading model
#### 3.1.4 Computational rooms: DDS domains for real-time pub/sub, Raft clusters for consensus, MQTT topic namespaces — fault isolation boundaries
#### 3.1.5 Room taxonomy table: type, properties, examples, hardware affinity, agent capacity
### 3.2 Room Mechanics
#### 3.2.1 Room state model: contents (agents, sensors, LLM instance), permissions, routing policy, capability domain
#### 3.2.2 Context inheritance: child rooms, adjacent rooms, portal semantics for state visibility
#### 3.2.3 Context carrying: serialized state bundle transfers with agent on room change including conversation history, active tasks, capability tokens
#### 3.2.4 Room lifecycle: creation, population, active state, hibernation, dissolution with state preservation
### 3.3 Agent Navigation
#### 3.3.1 Presence model: agent registration, heartbeat, capability advertisement per room
#### 3.3.2 Room transitions as atomic operations: physical relocation + DDS domain change + MQTT re-subscription + PLATO context load + capability re-issuance, all-or-nothing
#### 3.3.3 Cross-room coordination: messaging patterns, broadcast scopes, delegation chains
#### 3.3.4 Access control: capability-based entry verification, permission inheritance from parent rooms
### 3.4 Rooms as Application Model
#### 3.4.1 Traditional applications decomposed into room compositions: single-room apps, hub-and-spoke, federated mesh
#### 3.4.2 Persistent rooms for long-lived contexts (vessel state, navigation history) vs ephemeral rooms for transient tasks
#### 3.4.3 Room composition diagram showing how a complex marine operation maps to interconnected rooms

## 4. Hardware Tier Architecture (~1,800 words, 3 tables, 2 diagrams)
### 4.1 Tier Definitions
#### 4.1.1 Tier 0 — ESP32-S3: wake word detection (18-22ms TFLite Micro), command relay, sensor node — $3-8, 160-260mA active, 0.8mA deep sleep
#### 4.1.2 Tier 1 — Raspberry Pi 5 8GB: K3s coordinator, MQTT broker, local LLM fallback (TinyLlama 1.1B at 12-18 tok/s), whisper.cpp STT — $80, 5-7W
#### 4.1.3 Tier 2 — NVIDIA Jetson Orin Nano: AI inference (67 TOPS INT8 at 15W), Llama 3.2 3B at ~28 tok/s, DeepStream vision, TensorRT-LLM — $259-499
#### 4.1.4 Tier 3 — Starlink + Cloud: satellite connectivity (25-50ms RTT, 100-400 Mbps), GPT-4o Realtime API (~320ms e2e), long-term storage
#### 4.1.5 Hardware specifications comparison table: compute, memory, AI TOPS, storage, network, power, cost per tier
### 4.2 Inter-Tier Communication
#### 4.2.1 Topology: ESP32 → MQTT → RPi → gRPC → Jetson → HTTP/2 → Starlink → Cloud LLM API
#### 4.2.2 Protocol selection per tier pair: MQTT for ESP32-RPi, gRPC for RPi-Jetson, WebSocket for streaming, HTTP/2 for Starlink
#### 4.2.3 Latency budgets between tier pairs with fallback chains when tiers fail
#### 4.2.4 Tier-to-tier communication diagram with protocol labels and latency annotations
### 4.3 Workload Placement
#### 4.3.1 Placement decision matrix: which inference runs where (wake word ESP32, STT RPi/Jetson, LLM Jetson/Cloud, vision Jetson)
#### 4.3.2 Migration triggers: node failure, load imbalance, power constraint, connectivity change
#### 4.3.3 Power budgeting: 12V marine DC distribution, annual cost per tier, total system power budget
#### 4.3.4 Capability matrix table: task (wake word, STT, LLM 1-3B, LLM 7-8B, TTS, vision, orchestration) vs tier

## 5. The Four Vessels — Identity and Service Mesh (~1,800 words, 2 tables, 1 diagram)
### 5.1 Vessel Identity Model
#### 5.1.1 Vessel definition: DID-backed service identity with capability advertisement and verifiable credential chain
#### 5.1.2 Forgemaster: constraint theory, mathematics, FLUX compiler, GPU kernels, formal proofs — the builder
#### 5.1.3 CCC: web UI, browser agents, dashboards, marketing, CopilotKit integration — the interface
#### 5.1.4 JetsonClaw1: hardware edge, Jetson deployment, ESP32 firmware, marine sensors, sonar — the hands
#### 5.1.5 Oracle1: core infrastructure, APIs, fleet coordination, lighthouse, search — the memory
#### 5.1.6 Vessel capability matrix: responsibilities, APIs, hardware affinity, published packages, failover behavior
### 5.2 Decentralized Identity
#### 5.2.1 DID document structure: did:key method, cryptographic material, service endpoints, capability assertions
#### 5.2.2 Authentication flow: challenge-response between Vessels using Ed25519 signatures
#### 5.2.3 Capability token format: JWT-style with issuer, subject, capabilities, expiry, scope (room-level)
#### 5.2.4 Vessel lifecycle: genesis (key generation), attestation (capability assignment), operation, retirement, rebirth
### 5.3 Service Mesh Dynamics
#### 5.3.1 Capability advertisement: structured JSON-LD descriptions of functions each Vessel exposes
#### 5.3.2 Service discovery: mDNS for local, gossip protocol (SIR model) for vessel-wide, lightweight DHT for global
#### 5.3.3 Request routing: intent-to-capability matching with load distribution across Vessel instances
#### 5.3.4 Circuit breaker patterns: handling Vessel unavailability, automatic failover to backup instances
### 5.4 Vessel Coordination
#### 5.4.1 Consensus model: Raft for trusted local clusters, ternary voting for cross-Vessel decisions
#### 5.4.2 Leader election: deterministic leader selection per room based on capability match and availability
#### 5.4.3 Conflict resolution: priority-based resolution when multiple Vessels propose conflicting actions

## 6. CopilotKit Integration — Natural Language Control Plane (~1,500 words, 2 tables, 2 diagrams)
### 6.1 CopilotKit Fork Architecture
#### 6.1.1 Fork status: 4 commits ahead (SuperInstance Fleet Copilot), 2,132 commits behind upstream (as of 2026-07-10)
#### 6.1.2 Package structure: @copilotkit/react-core for hooks, @copilotkit/runtime for backend, @copilotkit/voice for audio
#### 6.1.3 SuperInstanceAgent: custom AbstractAgent subclass connecting to DeepSeek V4 Flash, Nebula, VoxelWorks
#### 6.1.4 Extension requirements: MQTT transport, Redis-backed runner, dynamic device registration, distributed agent support
### 6.2 Natural Language Pipeline
#### 6.2.1 Voice input: ESP32 wake word → audio stream → STT (whisper.cpp/faster-whisper) → text intent
#### 6.2.2 Context assembly: current room state, agent capabilities, relevant history → LLM prompt
#### 6.2.3 Action generation: LLM produces structured tool calls mapped to Vessel capabilities
#### 6.2.4 Response synthesis: action results → natural language → TTS (Piper/Kokoro) → audio playback
### 6.3 Control Plane Architecture
#### 6.3.1 Tool schemas as universal API: every device capability exposed as useCopilotAction with name, description, parameters
#### 6.3.2 Voice-to-action mapping table: example commands and their tool call translations
#### 6.3.3 Two-tier command routing: reflex commands (pincher pattern match, <1ms) vs deliberative commands (full LLM pipeline, 700ms-3s)
#### 6.3.4 Error recovery: misunderstood commands → clarification dialogue, failed actions → fallback + notification
### 6.4 Integration with Rust Core
#### 6.4.1 Interface contract: GraphQL mutations/subscriptions over SSE between CopilotKit runtime and 5-layer backend
#### 6.4.2 Message formats: BaseEvent types (RUN_STARTED, TOOL_CALL, TEXT_MESSAGE, RUN_FINISHED)
#### 6.4.3 Latency budget: voice capture (40ms) + VAD (500ms) + STT (300ms) + LLM (500ms) + TTS (100ms) = ~1.5s typical
#### 6.4.4 Integration architecture diagram showing CopilotKit frontend, runtime, and Rust backend connections

## 7. Two-Tier Safety Architecture (~1,500 words, 2 tables, 2 diagrams)
### 7.1 Safety Design Overview
#### 7.1.1 Design goal: reflex <1ms (pincher), deliberative 700ms-3s, never both on the same critical path
#### 7.1.2 Fundamental tension: speed requires simplicity, intelligence requires complexity — separate them completely
#### 7.1.3 Safety invariants: emergency stop, collision avoidance, fire suppression, bilge pump — always reflex
#### 7.1.4 Timing budget allocation table per tier: ESP32 reflex (<1ms), RPi reflex (<10ms), Jetson deliberative (<3s)
### 7.2 Reflex Tier
#### 7.2.1 Trigger conditions: hardware interrupts, threshold violations, pattern matches on sensor streams
#### 7.2.2 Pincher reflex engine: regex + embedding match, vector DB lookup, predefined action dispatch — no LLM in path
#### 7.2.3 ESP32 bare-metal reflex: GPIO interrupt → action lookup → GPIO output in <1ms
#### 7.2.4 Reflex action set: HALT, RETREAT, ALERT, ISOLATE, ACTIVATE_BACKUP — predefined, never generated
#### 7.2.5 Reflex latency diagram: event → pincher → action with sub-millisecond timing annotations
### 7.3 Deliberative Tier
#### 7.3.1 Activation: non-safety-critical commands requiring reasoning, planning, or multi-step execution
#### 7.3.2 Risk assessment: LLM-based evaluation of action safety before execution with confidence scoring
#### 7.3.3 Human-in-the-loop: explicit confirmation for irreversible, expensive, or dangerous operations
#### 7.3.4 Deliberative fallback: timeout or failure → reflex takeover with safe default action
### 7.4 Safety Monitoring
#### 7.4.1 Sentinel Vessel: continuous safety state monitoring, safety event logging, real-time safety posture
#### 7.4.2 Safety event log: immutable record of all reflex activations with cause, response, outcome
#### 7.4.3 Post-incident analysis: automatic learning from safety events to refine reflex triggers

## 8. Self-Assembly and Auto-Discovery (~1,500 words, 1 table, 2 diagrams)
### 8.1 Auto-Discovery Protocol
#### 8.1.1 Discovery mechanism: mDNS/Bonjour broadcast for same-room, gossip protocol for vessel-wide, DHT for global
#### 8.1.2 Capability advertisement: structured JSON format with device type, available functions, protocol versions, resource availability
#### 8.1.3 Network scanning strategies: periodic scan, trigger-based scan (new node detection), full mesh refresh
#### 8.1.4 Security during discovery: capability token validation prevents rogue node join even if network is compromised
### 8.2 Node Lifecycle
#### 8.2.1 State machine: OFFLINE → DISCOVERING → NEGOTIATING → JOINING → ACTIVE → DEGRADED → LEAVING → OFFLINE
#### 8.2.2 Join protocol: discover → authenticate → capability exchange → room assignment → state sync → active
#### 8.2.3 Trust establishment: progressive trust building as nodes prove reliability over time
#### 8.2.4 Auto-join timing: target <60 seconds from power-on to full participation
### 8.3 Task Redistribution
#### 8.3.1 Triggers: node join (absorb load), node leave (redistribute), load imbalance (rebalance), priority change (preempt)
#### 8.3.2 Task migration protocol: state serialization → context transfer → capability handover → resume on target
#### 8.3.3 State bundle format: agentId, agentType, serializedState, capabilities, capabilityTokens, timestamp, TTL
#### 8.3.4 Migration diagram showing state bundle flow between source and target nodes
### 8.4 Mesh Resilience
#### 8.4.1 Partition handling: each partition continues with local consensus, no global progress during split
#### 8.4.2 Reconciliation: CRDT-based state merge when partitions reconnect, conflict resolution via timestamp + priority
#### 8.4.3 Graceful degradation: feature reduction rather than failure — local-only mode, reduced agent capacity
#### 8.4.4 24-hour offline operation: what functions persist without cloud (local LLM, reflex commands, room navigation)

## 9. Network Architecture and Starlink Integration (~1,200 words, 2 tables, 1 diagram)
### 9.1 Network Topology
#### 9.1.1 Local mesh: ESP32 (WiFi/BLE) → RPi (Ethernet/WiFi) → Jetson (Ethernet) — wired preferred for latency
#### 9.1.2 Starlink gateway: RPi or Jetson as default gateway, QoS/CAKE SQM for bufferbloat prevention
#### 9.1.3 Network segmentation: VLAN per room, management network isolated from sensor/actuator traffic
#### 9.1.4 Network topology diagram: physical layout with device placement, link types, latency annotations
### 9.2 Connectivity Management
#### 9.2.1 "Usually connected" paradigm: design for connected-with-degradation, not offline-first
#### 9.2.2 Connectivity state machine: ONLINE → DEGRADED → LOCAL_ONLY → OFFLINE → RECOVERING
#### 9.2.3 Starlink latency profile: median 25-50ms, jitter 5-20ms, handoff spikes 100-500ms every ~15s
#### 9.2.4 Fallback chains: local LLM (Jetson) → cached responses → reflex-only mode → manual override
### 9.3 Data Synchronization
#### 9.3.1 Local-first data model: all state changes commit locally first, async sync to cloud when connected
#### 9.3.2 Sync protocol: CRDTs for conflict-free replication, gossip for efficient dissemination
#### 9.3.3 Bandwidth budgeting: voice commands highest priority, telemetry batched, logs compressed
#### 9.3.4 Starlink optimization: persistent HTTP/2 connections, connection pooling, request batching, GPT-4o-mini for simple commands

## 10. Performance Benchmarks and Success Criteria (~1,200 words, 3 tables, 1 diagram)
### 10.1 Benchmark Targets
#### 10.1.1 Voice-to-action: <3 seconds end-to-end (voice capture → STT → LLM → TTS → playback)
#### 10.1.2 Reflex response: <700ms software, <1ms hardware (GPIO interrupt → action)
#### 10.1.3 Auto-join: <60 seconds from power-on to full mesh participation
#### 10.1.4 Offline autonomy: 24 hours of full operation without cloud connectivity
#### 10.1.5 Performance targets summary table: metric, target, measurement method, priority, verification approach
### 10.2 Voice-to-Action Latency Analysis
#### 10.2.1 Latency waterfall: per-stage timing budget (voice capture 40ms, VAD 500ms, STT 300ms, LLM 500ms, TTS 100ms, playback 40ms)
#### 10.2.2 Critical path: VAD endpointing and LLM generation are the dominant latency contributors
#### 10.2.3 Optimization opportunities: streaming STT, speculative TTS, GPT-4o-mini for simple commands, local Jetson inference
#### 10.2.4 Voice-to-action budget table: stage name, budget, actual measured, gap, optimization
### 10.3 Hardware Benchmarks
#### 10.3.1 Jetson LLM benchmarks: Llama 3.2 3B (~28 tok/s), Mistral 7B (~17 tok/s), Phi-3.5 Mini (~25 tok/s), RAM usage per model
#### 10.3.2 ESP32 wake word: 18-22ms inference, 80KB INT8 model, 4-month battery life on 2000mAh
#### 10.3.3 RPi 5 whisper.cpp: tiny.en 3.5x real-time, TinyLlama 1.1B 12-18 tok/s, K3s with 512MB RAM minimum
#### 10.3.4 Starlink: 25-50ms RTT median, 99th percentile <65ms, <1% packet loss, 100-400 Mbps down
#### 10.3.5 Hardware benchmark comparison table: device, task, metric, measured value, target, status

## 11. Implementation Roadmap (~1,200 words, 2 tables, 1 diagram)
### 11.1 Development Phases
#### 11.1.1 Phase 1 — Foundation (months 1-2): core Rust runtime, basic room model, single-node voice command on RPi+Jetson
#### 11.1.2 Phase 2 — Mesh (months 3-4): auto-discovery, multi-node coordination, ESP32 integration, basic Vessel identities
#### 11.1.3 Phase 3 — Intelligence (months 5-6): CopilotKit integration refinement, NL pipeline optimization, Starlink LLM routing
#### 11.1.4 Phase 4 — Safety (months 7-8): reflex tier certification, safety monitoring dashboard, incident response automation
#### 11.1.5 Phase 5 — Optimization (months 9-12): compilation pipeline integration, performance tuning, benchmark achievement
#### 11.1.6 Phase deliverables table: phase, duration, key deliverables, entry criteria, exit criteria, dependencies
### 11.2 Risk Assessment
#### 11.2.1 Technical risks: compilation pipeline complexity, latency target feasibility, single maintainer bottleneck
#### 11.2.2 Integration risks: CopilotKit fork divergence, LLM API rate limits and costs, Starlink availability
#### 11.2.3 Operational risks: security model validation, hardware failure rates at sea, documentation debt
#### 11.2.4 Risk register: risk ID, description, likelihood, impact, mitigation strategy, owner
### 11.3 Validation Strategy
#### 11.3.1 Per-phase acceptance criteria: measurable exit conditions for each phase
#### 11.3.2 End-to-end test: full voice-to-action scenario on vessel — "Set autopilot to course 270, speed 8 knots"
#### 11.3.3 Continuous benchmarking: automated performance regression detection on each commit

## 12. Appendices
### 12.1 Glossary
#### 12.1.1 Terms: Vessel, Room, MUD, Reflex, Deliberative, Pincher, FLUX, VoxelWorks, Nebula, CHARTER.md
### 12.2 Bill of Materials
#### 12.2.1 Hardware BOM: ESP32-S3 DevKitC ($10-15), INMP441 mic ($2-3), RPi 5 8GB ($80), Jetson Orin Nano ($259-499), Starlink Standard ($599 + $75-120/mo)
#### 12.2.2 Total system cost: ~$1,200-1,600 one-time + $75-120/month Starlink
### 12.3 Related Work
#### 12.3.1 Academic: Mobile agent platforms (Aglets, Voyager), ROS2 DDS, SwarmSys, MoA-Off, SCR
#### 12.3.2 Industry: CopilotKit, NVIDIA Isaac, K3s, Starlink, Ollama, Home Assistant

# References
## superinstance.agent.outline.md
- **Type**: Report outline
- **Description**: This outline file
- **Path**: /mnt/agents/output/superinstance.agent.outline.md

## Research Dimension Files
- **Type**: Deep research artifacts
- **Description**: Four dimension analyses + cross-verification + insights
- **Path**: /mnt/agents/output/research/superinstance_dim01-04_*.md, superinstance_cross_verification.md, superinstance_insight.md

## Supporting Analysis
- **Type**: Requirements, synthesis, structure, content plan
- **Description**: Sub-agent outputs from outline design phase
- **Path**: /mnt/agents/output/superinstance_requirements.md, superinstance_artifact_synthesis.md, superinstance_structure_design.md, superinstance_content_plan.md
