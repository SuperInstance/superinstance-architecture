# SuperInstance Architecture Document — Content Plan

This plan defines specific content points, required elements, tables, diagrams, code examples, cross-references, and data sources for each chapter of the SuperInstance Architecture Document (~15,000-20,000 words).

---

## Chapter 1: Executive Summary

### Content Points
- What SuperInstance is: a voice-controlled, self-assembling distributed system for heterogeneous edge hardware
- The problem it solves: operating complex distributed systems in disconnected maritime environments with single-maintainer constraints
- The four defining architectural insights: (1) music cognition as coordination substrate, (2) room-based context architecture, (3) two-tier safety, (4) voice as universal API
- Hardware scope: ESP32 microcontrollers to Starlink-connected cloud, with NVIDIA Jetson as edge AI anchor
- Ecosystem scale: 4,095+ repositories (as of 2026-07-10), four Vessels, 5-layer execution stack
- Document scope and intended audience: systems architects, edge AI engineers, distributed systems researchers, marine operators
- Key metrics summary: latency budgets, power budgets, offline capability duration

### Required Tables
| Table Title | Column Headers |
|---|---|
| Architecture at a Glance | Component, Technology, Purpose, Latency |
| Hardware Tier Summary | Tier, Device, Cost, Power, Primary Role |
| Ecosystem Scale Metrics | Metric, Value, Source |
| Document Chapter Guide | Chapter, Topic, Page Estimate |

### Required Diagrams
- **SuperInstance system overview diagram**: Single-page visual showing voice input flowing through CopilotKit, the 5-layer stack, four hardware tiers, and Starlink cloud connectivity. Show the four Vessels positioned around the stack.
- **Document scope diagram**: What this document covers (architecture, protocols, interfaces) vs. what it does not cover (installation, operation, marketing)

### Code Examples
- None required for executive summary

### Cross-References
- Forward references to all 14 subsequent chapters with one-line summaries
- Cross-reference to requirements document (`superinstance_requirements.md`)

### Key Sources
- `superinstance_artifact_synthesis.md` (all themes, confidence levels)
- `superinstance_requirements.md` (success criteria, NFR targets)
- `superinstance_dim03_ecosystem_map.md` (scale metrics)

---

## Chapter 2: Vision and Principles

### Content Points
- The "right moment is everything" philosophy: timing over quality in multi-agent coordination, supported by agent-sync's 2.46x timing advantage
- Jazz improvisation as mathematical coordination substrate, not metaphor: groove as consensus mechanism, voice leading as migration protocol
- Ternary logic {-1, 0, +1} as the natural agent expression language: agree, neutral, disagree
- The MUD principle: every application is a Multi-User Dungeon; agents navigate rooms, sense contexts, and act accordingly
- The three-room model: physical rooms (bridge, engine room, back deck), virtual rooms (PLATO knowledge spaces), and computational rooms (DDS domains, Raft clusters)
- Origin-centric coordination: every decision traced to its source; provenance as first-class concern
- Five design principles: (1) usually connected with graceful degradation, (2) voice as universal API, (3) safety never depends on LLM, (4) rooms are namespaces for everything, (5) compilation unifies cognition and governance
- Connection to actual practice: Casey Digennaro, commercial fisherman in Sitka, Alaska — building the system while operating the F/V Quantum in the Bering Sea

### Required Tables
| Table Title | Column Headers |
|---|---|
| Design Principles | Principle, Description, Architectural Implication, Verification Method |
| Music-to-Architecture Mapping | Musical Concept, Distributed Systems Analog, Implementation, Confidence |
| Ternary Logic Operations | Operation, Binary Equivalent, Computational Advantage, Usage Context |
| MUD Spatial Primitives | Primitive, Function, SuperInstance Implementation, Research Source |

### Required Diagrams
- **The "jam session" coordination model**: Visual showing agents as musicians in a room, with arrows representing listening (observing), feeling (consensus), contributing (action). Annotate with corresponding distributed systems mechanisms.
- **Three-room convergence diagram**: Three overlapping circles labeled Physical, Virtual (PLATO), Computational (DDS) with agent transition arrows showing the atomic nature of room changes across all three layers.
- **Ternary decision space**: Triangle diagram with vertices at -1, 0, +1 showing how agents express positions and how consensus is reached through geometric methods on the Eisenstein lattice.

### Code Examples
- Ternary state enum pseudocode: `enum TernaryState { Disagree = -1, Neutral = 0, Agree = 1 }`
- Constraint theory lattice operation (from constraint-theory-core): brief C/Rust snippet showing Eisenstein integer operations
- MUD room definition YAML: example room configuration with exits, sensors, agents

### Cross-References
- Chapter 5 (Room Model) — detailed room architecture
- Chapter 6 (Agent Architecture) — agent lifecycle and coordination
- Chapter 10 (Safety Layer) — reflex vs. deliberative separation
- Chapter 12 (Security Model) — capability-based access

### Key Sources
- `superinstance_artifact_synthesis.md` (Theme 1: music cognition as mathematical substrate)
- `superinstance_dim02_distributed_patterns.md` (Section 5: context/room-based architecture, SCR)
- `superinstance_dim03_ecosystem_map.md` (Agent timing: 2.46x advantage from agent-sync)
- `superinstance_insight.md` (Insights 1, 3, 7, 8)

---

## Chapter 3: Ecosystem Overview

### Content Points
- Full ecosystem scale: 4,095+ repositories (2,000 cataloged as of 2026-06-06), 6,000+ tests, 373,639+ lines of Rust (as of 2026-07-10), 1,500,000+ words of documentation
- The four Vessels as service identities:
  - **Forgemaster** (330 repos): constraint theory, mathematics, FLUX compiler, GPU kernels, formal proofs
  - **CCC** (116 repos): web UI, browser agents, dashboards, marketing, frontend components
  - **JetsonClaw1** (76 repos): hardware edge, Jetson, ESP32, marine sensors, sonar, edge ML
  - **Oracle1** (43 repos): core infrastructure, APIs, fleet coordination, lighthouse
- The 5-layer execution stack: open-parallel (L1: ternary math) -> pincher (L2: reflex) -> flux-core (L3: bytecode IR) -> cuda-oxide (L4: GPU compiler) -> cudaclaw (L5: deployed kernels)
- How the layers connect: pincher as spinal cord, flux-core as cortex, cuda-oxide as motor cortex, cudaclaw as motor execution
- Git-agent lifecycle (v2.0): PULL -> BOOT -> WORK -> LEARN -> PUSH -> SLEEP -> PULL
- Published packages: PyPI 35+, crates.io 24+, npm 18+
- Key repository families: lau-* (105), flux-* (83), cocapn-* (59), ternary-* (59), agent-* (50), fleet-* (49), plato-* (41)
- Integration maturity assessment: tight (constraint-theory bindings), moderate (PLATO SDK), loose (many educational repos)
- Critical gaps: no stable releases, documentation fragmentation, no integration test suite, single contributor across 4,095+ repos (as of 2026-07-10)

### Required Tables
| Table Title | Column Headers |
|---|---|
| Vessel Profiles | Vessel, Repos, Role, Specialization, Key Repos |
| 5-Layer Stack | Layer, Component, Language, Purpose, Latency |
| Repo Family Inventory | Prefix, Count, Description, Key Packages |
| Published Packages | Package Name, Registry, Purpose, Install Command |
| Maturity Assessment | Repo, Evidence, Maturity Level |
| Critical Gaps | Gap, Severity, Description, Mitigation |

### Required Diagrams
- **5-layer stack architecture**: Vertical diagram showing L1 through L5 with data flow arrows. Each layer annotated with component name, language, and primary function. Show the compilation path: intent -> FLUX bytecode -> MIR -> PTX -> GPU kernel.
- **Vessel identity map**: Four quadrants, one per Vessel, showing repo count, key domains, published packages, and inter-vessel communication channels.
- **Git-agent lifecycle diagram**: Circular flow diagram showing PULL->BOOT->WORK->LEARN->PUSH->SLEEP with annotations for each phase's purpose and duration.
- **Dependency graph**: Simplified graph showing open-parallel -> pincher -> flux-* -> cuda-oxide -> cudaclaw chain, plus plato-sdk -> cocapn-* -> fleet-* cross-links.

### Code Examples
- Git-agent lifecycle pseudocode: show the six phases with git commands and state transitions
- State bundle JSON format (from Dim02): agentId, agentType, serializedState, capabilities, capabilityTokens, TTL
- CHARTER.md template: what an agent reads on boot

### Cross-References
- Chapter 4 (Hardware Architecture) — JetsonClaw1 hardware repos
- Chapter 6 (Agent Architecture) — git-agent lifecycle detail
- Chapter 7 (CopilotKit Integration) — CCC web/frontend repos
- Chapter 14 (Roadmap) — gap mitigation planning

### Key Sources
- `superinstance_dim03_ecosystem_map.md` (full ecosystem analysis)
- `superinstance_artifact_synthesis.md` (Theme 7: 5-layer pipeline)
- `superinstance_dim01_copilotkit_analysis.md` (integration patterns)

---

## Chapter 4: Hardware Architecture

### Content Points
- Four-tier hardware hierarchy rationale: matching compute to task, power budget constraints, cost optimization
- **Tier 1 — ESP32-S3**: wake word detection (18-22ms inference via TFLite Micro), command relay, sensor node. $3-8/chip, 160-260mA active, 0.8mA deep sleep. Vector instructions for ML acceleration. WiFi + BLE.
- **Tier 2 — Raspberry Pi 5**: edge coordinator, K3s host (512MB RAM minimum), MQTT broker (Mosquitto), small LLM inference (TinyLlama 1.1B at 12-18 tok/s), whisper.cpp STT (tiny.en at 3.5x real-time). $80, 5-7W active. NVMe SSD via HAT mandatory.
- **Tier 3 — NVIDIA Jetson Orin Nano**: AI inference node. 67 TOPS INT8 at 15W, 8GB unified memory. Llama 3.2 3B at ~28 tok/s (TensorRT-LLM), Mistral 7B at ~17 tok/s. DeepStream 7.1 for vision, faster-whisper for STT. $259-499.
- **Tier 4 — Starlink + Cloud**: satellite connectivity (25-50ms RTT, 100-400 Mbps), cloud LLM APIs (GPT-4o Realtime API at ~320ms e2e). Starlink Mini 3-year TCO ~$3,899.
- Hardware capability matrix by task: wake word, STT, LLM inference (1-3B, 7-8B), TTS, computer vision, container orchestration
- Power consumption comparison across tiers with annual cost at $0.15/kWh
- Starlink latency characteristics: median 25-50ms, 99th percentile <65ms, jitter 5-20ms, satellite handoffs every ~15s
- Starlink disruption types and mitigations: handoff (15-100ms), brief obstruction (100ms-3s), weather (1-60s), peak congestion (sustained)
- Latency budget for full voice pipeline (local STT + cloud LLM + local TTS): 700-2500ms total
- Bill of materials: ESP32-S3 DevKitC ($10-15), INMP441 mic ($2-3), RPi 5 8GB ($80), Jetson Orin Nano ($259-499), Starlink Standard ($599 + $75-120/mo)

### Required Tables
| Table Title | Column Headers |
|---|---|
| Hardware Tier Specifications | Tier, Device, CPU, GPU/AI, RAM, Storage, Network, Power, Price |
| Capability by Task | Task, ESP32-S3, RPi 5, Jetson Orin Nano, Cloud |
| Power Consumption | Device, Idle, Active, Annual Cost |
| Starlink Latency Budget | Stage, Time, Notes |
| Starlink Disruption Types | Type, Duration, Mitigation |
| Jetson LLM Benchmarks | Model, Size, Quantization, Tokens/sec, RAM Used |
| Bill of Materials | Item, Unit Cost, Quantity, Purpose |
| Software Stack per Tier | Layer, ESP32-S3, RPi 5, Jetson Orin Nano |

### Required Diagrams
- **Four-tier hardware architecture**: Horizontal layered diagram showing ESP32 -> RPi -> Jetson -> Starlink/Cloud with data flows, protocol labels (MQTT, gRPC, HTTP/2), and latency annotations at each transition.
- **Hardware placement on vessel**: Floor plan showing ESP32 nodes in each room (bridge, engine room, hold, cabin), RPi coordinator in central location, Jetson in equipment rack, Starlink antenna on deck.
- **Power budget diagram**: Power flow from 12V marine DC through buck converters to each tier with consumption values and total budget.
- **Starlink latency profile**: Time-series style diagram showing typical latency band (25-50ms), jitter range, handoff spikes, and worst-case weather events.
- **Voice pipeline latency waterfall**: Stacked bar showing time contribution of each stage from audio capture to TTS playback.

### Code Examples
- ESP32 wake word pipeline: I2S mic -> MFCC extraction -> TFLite Micro inference -> MQTT publish (with timing annotations per stage)
- K3s node registration YAML for RPi coordinator
- Jetson TensorRT-LLM model configuration for Llama 3.2 3B
- Starlink optimization: connection pooling, SQM/CAKE configuration for prioritizing voice traffic

### Cross-References
- Chapter 5 (Room Model) — hardware placement per room
- Chapter 9 (Self-Assembly) — hardware auto-detection
- Chapter 11 (Voice Pipeline) — full voice pipeline detail
- Chapter 13 (Starlink Integration) — connectivity patterns

### Key Sources
- `superinstance_dim04_voice_edge_tech.md` (full hardware benchmarks, Starlink analysis)
- `superinstance_artifact_synthesis.md` (Theme 3: four-tier hardware, critical data points table)
- `superinstance_requirements.md` (hardware constraints section)

---

## Chapter 5: The Room Model

### Content Points
- The room as universal namespace: physical, virtual, and computational rooms converge on a single abstraction
- **Physical rooms**: bridge, engine room, cabin, back deck, galley, crow's nest, hold. Each has its own sensor set, actuator set, and human occupants.
- **Virtual rooms (PLATO)**: knowledge as spectral graph, tiles with dependencies, failure-first reading. Knowledge rooms where agents learn, deliberate, and store context.
- **Computational rooms**: DDS domains for real-time pub/sub, Raft clusters for local consensus, MQTT topic namespaces. Each room is a fault-isolation boundary.
- Room contents: agents present, sensors publishing, LLM instance with routing policy, capability domain, agent population
- Room transitions as atomic operations: physical relocation + DDS domain change + MQTT re-subscription + PLATO context load + capability re-issuance
- The "portal" abstraction: handles state migration, message forwarding, and context preservation during room transitions
- MUD/MUSH architecture foundations: Evennia framework, room-exit-object-trigger primitives
- Self-Organising Coordination Regions (SCR): decentralized leader election, overlapping regions for fault tolerance
- Topic structure: `/vessel/{room}/{device}/{sensor|actuator}/{metric}`
- Context handoff protocol: what state is preserved, what is discarded, what is recomputed on arrival
- Tripartite room pattern: three innate agents per room — Ground Truth (physics), Constraint (engineer), Communication (diplomat)

### Required Tables
| Table Title | Column Headers |
|---|---|
| Physical Room Inventory | Room, Sensors, Actuators, Hardware Tier, Agents |
| Room Type Comparison | Aspect, Physical Room, PLATO Room, Computational Room |
| Room Transition Atomicity | Step, Action, Duration, Failure Mode |
| MUD-to-SuperInstance Mapping | MUD Primitive, SuperInstance Equivalent, Implementation |
| MQTT Topic Structure | Topic Pattern, Room, Device Type, Metric, QoS |
| PLATO Room Components | Component, Description, Technology |

### Required Diagrams
- **Three-room convergence**: Three overlapping circles (Physical, Virtual, Computational) with an agent at the intersection. Show transition arrows crossing all three simultaneously.
- **Vessel floor plan with rooms**: Top-down layout of vessel showing room boundaries, ESP32 node placement, DDS domain boundaries, and agent population per room.
- **Room transition sequence**: Swimlane diagram showing agent, portal service, DDS, MQTT broker, and PLATO store collaborating through a room change. Annotate each step with timing.
- **SCR regional overlay**: Diagram showing overlapping coordination regions on the vessel floor plan with leader election annotations.
- **Topic namespace tree**: Hierarchical tree diagram of MQTT topics rooted at `/vessel/` with branches per room.

### Code Examples
- Room definition YAML/JSON: name, sensors, actuators, DDS domain, LLM policy, capability requirements
- Portal transition API: request migration, serialize state, relocate, re-subscribe, resume
- DDS domain configuration XML for a room
- PLATO room query example: loading context for a newly arrived agent

### Cross-References
- Chapter 4 (Hardware) — hardware placement per room
- Chapter 6 (Agent Architecture) — agent mobility and lifecycle
- Chapter 8 (Communication) — DDS domains and MQTT topics
- Chapter 12 (Security) — room-scoped capability tokens

### Key Sources
- `superinstance_dim02_distributed_patterns.md` (Section 5: context/room-based architecture, SCR)
- `superinstance_artifact_synthesis.md` (Theme 2: room-based context architecture)
- `superinstance_insight.md` (Insight 3: rooms are both physical and virtual)
- `superinstance_requirements.md` (F2: agent mobility and room navigation)

---

## Chapter 6: Agent Architecture

### Content Points
- Agent identity model: Vessel-backed, DID-based, capability-scoped
- The four Vessels as agent identity families: Forgemaster (math/compiler), CCC (web/UI), JetsonClaw1 (hardware/edge), Oracle1 (fleet/coordination)
- Git-agent lifecycle v2.0: six phases (PULL, BOOT, WORK, LEARN, PUSH, SLEEP) with state transitions and failure handling
- Weak mobility with structured state bundles: code + serialized state + capability tokens, restart via onArrival() hook
- State bundle format: agentId, agentType, sourceNode, targetNode, serializedState, capabilities, capabilityTokens, TTL
- Capability-based security at agent level: each agent carries capability tokens defining what it can do in each room
- Agent-to-agent communication patterns: Message-in-a-Bottle (git-native), A2A protocol, I2I (Iron sharpens Iron — git commits), murmur protocol (gossip)
- Agent presence and heartbeats: ternary beacon protocol for discovery, health scoring with necrosis detection
- Agent coordination patterns: ensemble (musical), swarm (hierarchical), jam session (ad-hoc)
- T-Minus prediction-first coordination: 70x fewer messages than polling by predicting arrival and confirming once
- Agent memory model: CHARTER (directive), STATE (current), TASK-BOARD (pending), SKILLS (capabilities), DIARY (learning log)
- Agent sandboxing: OpenConstruct for runtime provisioning, Docker/Containerd for isolation

### Required Tables
| Table Title | Column Headers |
|---|---|
| Agent Lifecycle Phases | Phase, Git Command, Purpose, Duration, Failure Handling |
| Agent Communication Patterns | Pattern, Protocol, Latency, Reliability, Use Case |
| State Bundle Format | Field, Type, Description, Example Value |
| Agent Memory Files | File, Purpose, Format, Updated When |
| Coordination Pattern Comparison | Pattern, Message Reduction, Convergence, Best For |
| Vessel-Agent Mapping | Vessel, Agent Types, Specialization, Repos |

### Required Diagrams
- **Agent lifecycle state machine**: Six states in a cycle with transition conditions and git commands labeled on edges.
- **Agent mobility sequence**: Sequence diagram showing agent serialization, transport, deserialization, and onArrival() execution.
- **Agent communication topology**: Mesh diagram showing Message-in-a-Bottle, A2A, I2I, and murmur channels between agents with protocol labels.
- **Capability token structure**: JWT-style token diagram showing iss (DID), sub (agent DID), aud (resource), capabilities array, room scope, expiry.
- **Agent memory model**: Five connected boxes showing CHARTER -> STATE -> TASK-BOARD -> SKILLS -> DIARY with read/write arrows.

### Code Examples
- State bundle JSON example with all fields populated
- onArrival() lifecycle hook pseudocode: validate tokens, load context, subscribe to topics, announce presence
- Agent tool schema (CopilotKit compatible): name, description, parameters, handler
- Capability token verification pseudocode: check DID signature, check expiration, check room scope, verify proof of possession
- T-Minus coordination message: predict arrival, set confirmation deadline, confirm once

### Cross-References
- Chapter 5 (Room Model) — agent presence in rooms
- Chapter 7 (CopilotKit) — agent tools as CopilotKit actions
- Chapter 9 (Self-Assembly) — agent discovery and registration
- Chapter 12 (Security) — capability tokens and DID identities

### Key Sources
- `superinstance_dim03_ecosystem_map.md` (Vessel system, git-agent lifecycle)
- `superinstance_dim02_distributed_patterns.md` (Section 2: agent mobility, Section 3: decentralized orchestration)
- `superinstance_artifact_synthesis.md` (Theme 6: Vessel identities as service mesh)
- `superinstance_insight.md` (Insight 5: Vessel system is service mesh identity)

---

## Chapter 7: CopilotKit Integration

### Content Points
- CopilotKit fork status: 1 commit ahead (SuperInstance Fleet Copilot), 7 commits behind upstream
- Core architecture: frontend (React/Angular/Vue) + GraphQL/SSE communication + CopilotRuntime with pluggable LLM adapters
- Why CopilotKit: 9/10 as agentic UI framework, reactive architecture (RxJS + SSE), clean multi-agent registration, A2UI rendering
- Known gaps: 5/10 as distributed glue — no agent discovery, single-process runtime, no MQTT/NATS, SQLite-only persistence
- SuperInstance Fleet Copilot integration: `showcase/integrations/superinstance/` — custom SuperInstanceAgent extending AbstractAgent
- Extension points for SuperInstance: custom MQTT transport, Redis-backed runner, dynamic agent registration, DistributedAgent base class
- Voice UI layer: speech-to-text input, streaming LLM responses, text-to-speech output, wake word activation
- Tool registration pattern: every hardware node exposes capabilities as CopilotKit tool schemas
- Multi-agent pattern: per-device agent mapping, fleet controller agent, agent switching at runtime
- A2UI (Agent-to-UI) for fleet dashboards: dynamic React component rendering based on agent state and room context
- MCP (Model Context Protocol) integration for external tool interoperability
- Extension priority: (1) MQTT transport, (2) Redis runner, (3) dynamic registration, (4) DistributedAgent class, (5) event sourcing

### Required Tables
| Table Title | Column Headers |
|---|---|
| CopilotKit Package Inventory | Package, Path, Role, SuperInstance Relevance |
| Suitability Assessment | Criterion, Score, Strength/Gap, Mitigation |
| Extension Points | Extension Point, Current State, Target, Priority |
| Tool Schema Examples | Tool Name, Description, Parameters, Target Hardware |
| Frontend Hooks for Fleet | Hook, Purpose, Extension Point, Usage |

### Required Diagrams
- **CopilotKit architecture with SuperInstance extensions**: Original CopilotKit stack (frontend -> GraphQL -> runtime -> adapters) annotated with SuperInstance additions: MQTT transport layer, Redis runner, dynamic agent registry, distributed runtime federation.
- **Fleet Copilot UI mockup**: Chat panel on left, dynamic dashboard on right (A2UI-rendered), status indicators at top. Show voice input button and command history.
- **Tool-to-hardware mapping**: Agent tool calls fanning out to ESP32, RPi, Jetson, and cloud endpoints with protocol labels.
- **Extension roadmap diagram**: Five extension items in priority order with estimated effort and dependency arrows.
- **Data flow diagram**: User voice -> CopilotKit -> GraphQL -> SuperInstanceAgent -> tool router -> device action -> response stream -> TTS.

### Code Examples
- CopilotKit provider setup with SuperInstance runtime URL
- SuperInstanceAgent class extending AbstractAgent (from dim01 analysis)
- Fleet tool definitions: get_nebula_status, query_voxelworks, control_autopilot, read_sensor
- useCopilotAction() for frontend tool registration
- useCoagentStateRender() for dynamic fleet dashboard components
- CopilotRuntime configuration with multi-agent registry
- Custom MQTT transport implementation sketch

### Cross-References
- Chapter 4 (Hardware) — tool-to-hardware mapping
- Chapter 6 (Agent Architecture) — agent registration and lifecycle
- Chapter 8 (Communication) — MQTT transport extension
- Chapter 11 (Voice Pipeline) — voice UI layer
- Chapter 14 (Roadmap) — extension implementation priority

### Key Sources
- `superinstance_dim01_copilotkit_analysis.md` (full structural analysis)
- `superinstance_artifact_synthesis.md` (Theme 4: CopilotKit as NL control plane)
- `superinstance_insight.md` (Insight 2: fork creates natural control plane, Insight 9: A2UI as fleet dashboard)

---

## Chapter 8: Communication Layer

### Content Points
- Three-scope communication: local real-time (DDS/ROS2), vessel-wide async (MQTT), cloud bridge (Zenoh)
- **DDS/ROS2** for local real-time: broker-less, peer-to-peer, 1-10ms latency. Fast DDS (default), Cyclone DDS (lightweight), RTI Connext (certified). Used for sensor-actuator loops sub-10ms.
- **MQTT** for vessel-wide: broker-based, lightweight (2-byte header), QoS 0/1/2. Mosquitto (edge), EMQX (scalable), NanoMQ (edge-optimized). Topic structure: `/vessel/{room}/{device}/{sensor|actuator}/{metric}`.
- **Zenoh** for cloud-edge bridging: unifies pub/sub, geo-distributed storage, queries. Zero overhead, native ROS2 bridge. Best for intermittent connectivity.
- Protocol comparison: MQTT (broker, 10-50ms), DDS (broker-less, 1-10ms), ROS2/DDS (broker-less, 1-10ms), Zenoh (broker-less, 1-10ms), ZeroMQ (broker-less, microseconds)
- ROS2 RMW implementations: Fast DDS (Apache 2), Cyclone DDS (EPL 2.0), RTI Connext (commercial), Zenoh (EPL 2.0)
- Gossip protocols for presence and state sync: Rumor Mongering (SIR) for agent heartbeat, Epidemic Broadcast Trees (EBT) for state synchronization
- Reactive streams (RxPY/RxJS) for sensor processing: debounce, throttle, buffer, merge, distinct_until_change
- Voice pipeline communication: WebRTC for audio streaming, WebSocket for text/metadata, gRPC for service-to-service
- Store-and-forward queue for offline buffering during Starlink outages

### Required Tables
| Table Title | Column Headers |
|---|---|
| Protocol Comparison | Protocol, Architecture, Latency, QoS, Discovery, Best For |
| ROS2 RMW Implementations | Implementation, License, RMW Package, Notes |
| Gossip Variants | Variant, Consistency, Bandwidth, Latency, Best For |
| MQTT Topic Patterns | Topic, Room, Device, Metric, QoS |
| Sensor Stream Operators | Operator, Purpose, Example, Backpressure |
| Voice Pipeline Protocols | Stage, Protocol, Latency, Fallback |

### Required Diagrams
- **Communication scope diagram**: Concentric circles labeled Local (DDS), Vessel (MQTT), Cloud (Zenoh) with device types at each layer and protocol labels.
- **Protocol latency spectrum**: Horizontal bar chart showing latency ranges for each protocol from ZeroMQ (microseconds) to MQTT over satellite (100ms+).
- **MQTT topic tree**: Hierarchical diagram of `/vessel/bridge/gps/lat`, `/vessel/engine/temp/coolant`, etc.
- **Gossip propagation visualization**: Network graph showing SIR model: initial rumor, first hop, second hop, convergence. Color nodes by state (ignorant/spreading/removed).
- **Reactive stream pipeline**: Dataflow diagram showing temperature sensor -> debounce -> filter -> buffer(5s) -> map(avg/max/min) -> MQTT publish.
- **Offline buffering flow**: Message queue filling during outage, draining on reconnection with priority labels.

### Code Examples
- DDS Quality of Service XML configuration for sensor data
- MQTT subscription setup with wildcard topics and QoS levels
- Zenoh put/get example for cloud-edge state sync
- RxPY sensor stream pipeline (from Dim02): debounce, filter, buffer, map, subscribe
- Gossip message format: type, payload, vector_clock, TTL
- Store-and-forward queue pseudocode: enqueue with priority, flush on connectivity

### Cross-References
- Chapter 4 (Hardware) — which protocols run on which tiers
- Chapter 5 (Room Model) — DDS domains per room, MQTT topic structure
- Chapter 9 (Self-Assembly) — gossip for discovery
- Chapter 11 (Voice Pipeline) — WebRTC and WebSocket usage
- Chapter 13 (Starlink) — offline buffering and cloud bridging

### Key Sources
- `superinstance_dim02_distributed_patterns.md` (Section 7: event-driven architecture, Section 3.4: gossip protocols)
- `superinstance_dim04_voice_edge_tech.md` (Section 7: streaming protocols)
- `superinstance_artifact_synthesis.md` (Theme 8: Starlink enables usually connected)

---

## Chapter 9: Self-Assembly Protocol

### Content Points
- Self-assembly definition: system configures itself from power-on to operational without manual intervention
- Three-tier discovery architecture:
  - **Local (same room)**: mDNS/Bonjour/Avahi for immediate device discovery. Works offline, no infrastructure.
  - **Vessel-wide**: gossip-based SWIM protocol (HashiCorp Memberlist) for cluster membership and presence.
  - **Global/Cloud**: libp2p Kademlia DHT for global service lookup with offline caching.
- Capability advertisement: each node broadcasts its capabilities (sensors, GPU, LLM support, storage) via gossip messages
- Self-assembly sequence: power-on -> capability discovery -> health assessment -> role assignment -> service startup -> operational announcement
- Resource binding protocol: agent requests resource -> capability matcher finds candidates -> negotiation -> binding -> confirmation
- Mesh formation: ESP32 nodes join WiFi mesh, RPi coordinator as K3s master, Jetson as GPU worker
- Failure recovery: node heartbeat timeout -> task redistribution -> health score update -> CopilotKit dashboard notification
- Hierarchical discovery for efficiency: don't search global DHT for a sensor in the same room
- Bootstrap problem: first node (RPi coordinator) creates the network, subsequent nodes discover it
- IPv6 link-local for mDNS fallback, bootstrap nodes for DHT, static coordinator list for SWIM

### Required Tables
| Table Title | Column Headers |
|---|---|
| Discovery Tier Matrix | Scope, Protocol, Latency, Offline?, Implementation |
| Capability Advertisement Format | Field, Type, Description, Example |
| Self-Assembly Sequence | Step, Trigger, Duration, Failure Mode |
| Resource Binding Flow | Step, Actor, Action, Protocol, Timeout |
| Mesh Formation States | State, Description, Transition Trigger |
| Failure Recovery Actions | Failure Type, Detection Time, Recovery Action, Duration |

### Required Diagrams
- **Discovery architecture**: Three-tier diagram showing mDNS (local LAN), gossip SWIM (vessel-wide), DHT (global) with connecting arrows showing fallback paths.
- **Self-assembly sequence**: Swimlane diagram with Node, mDNS, Gossip, DHT, K3s, and MQTT lanes showing the full power-on to operational flow.
- **Mesh topology**: Network graph showing RPi as central hub, ESP32 nodes as leaves, Jetson as GPU branch, Starlink as external link.
- **Resource binding flow**: Sequence diagram: agent -> capability matcher -> candidate nodes -> negotiation -> binding confirmation.
- **Failure recovery timeline**: Time axis showing node failure, heartbeat timeout (5s), task redistribution (10s), dashboard notification (15s).

### Code Examples
- mDNS service registration: service type, port, TXT record with capabilities
- SWIM gossip message format: node ID, sequence number, capabilities, health score
- Kademlia DHT bootstrap and key lookup
- Self-assembly state machine pseudocode with transitions
- Resource binding API: request, match, negotiate, bind

### Cross-References
- Chapter 4 (Hardware) — hardware auto-detection
- Chapter 6 (Agent Architecture) — agent discovery and presence
- Chapter 8 (Communication) — gossip protocols, mDNS
- Chapter 10 (Safety) — emergency stop bypasses normal discovery
- Chapter 14 (Roadmap) — implementation milestones

### Key Sources
- `superinstance_dim02_distributed_patterns.md` (Section 4: resource discovery & self-assembly)
- `superinstance_artifact_synthesis.md` (Decision AD-2: hierarchical discovery)
- `superinstance_requirements.md` (F4: self-assembly and resource discovery)

---

## Chapter 10: Safety and Reflex Layer

### Content Points
- Two-tier command classification: reflex tier (safety-critical) and deliberative tier (general)
- **Reflex tier**: Pincher engine — <1ms response using regex and embeddings in a vector database. No LLM involvement. Hard real-time.
- **Deliberative tier**: full STT -> LLM -> TTS pipeline. 700ms-3s latency. Soft real-time. Acceptable for non-safety commands.
- Pincher architecture: vector DB as runtime, LLM as compiler. 57 commits, Rust 76.6% + Python 19%. Installable via `cargo install pincher`.
- ESP32 wake word as hardware reflex: 18-22ms inference, bypasses all software for initial trigger
- Command classification rules: what is safety-critical (engine stop, throttle cut, emergency alert) vs. deliberative (status queries, navigation, routine control)
- Bridging reflex to UI: status feedback mechanism that does not block the reflex path
- Fail-safe defaults: on LLM pipeline failure, default to last known safe state; on reflex engine failure, emergency hardware cutout activates
- Nebula reflex engine integration: Cloudflare Workers-based edge matching, ~700ms fast-path
- Emergency stop architecture: hardware-level E-stop circuit independent of all software, triggers on any reflex engine failure
- Confidence thresholds: commands below certainty threshold are escalated to human confirmation before execution

### Required Tables
| Table Title | Column Headers |
|---|---|
| Two-Tier Command Classification | Tier, Examples, Response Time, Pipeline, Failure Mode |
| Safety-Critical Commands | Command, Reflex Handler, Confirmation Required?, Hardware Cutout? |
| Pincher vs. Deliberative | Aspect, Pincher (Reflex), Full Pipeline (Deliberative) |
| Latency Comparison | Stage, Reflex Path, Deliberative Path |
| Fail-Safe States | Component, Failure Condition, Default State, Recovery |

### Required Diagrams
- **Two-tier architecture**: Vertical diagram showing voice input splitting into reflex path (Pincher, <1ms, straight to actuator) and deliberative path (STT->LLM->TTS, 700ms-3s). Highlight the bypass: reflex goes around LLM entirely.
- **Pincher internal architecture**: Vector DB -> embedding match -> regex validation -> action dispatch. Show the <1ms target on the critical path.
- **Emergency stop circuit**: Hardware diagram showing E-stop button -> relay -> actuator power cutoff, completely independent of software path.
- **Command classification decision tree**: Flowchart starting with voice input -> wake word -> classification (reflex/deliberative) -> appropriate pipeline.
- **Failure mode diagram**: What happens when each component fails (LLM down: local fallback; Pincher down: hardware cutout; Starlink down: offline mode; ESP32 down: room-level failover).

### Code Examples
- Pincher reflex rule definition: pattern, embedding, priority, action, safety classification
- Two-tier command router: classify command, route to appropriate tier, handle response
- Emergency stop handler: hardware trigger, state machine, recovery procedure
- Confidence threshold check: if confidence < 0.85, require human confirmation
- Fail-safe state machine: states (normal, degraded, emergency, offline) with transitions

### Cross-References
- Chapter 4 (Hardware) — ESP32 wake word, hardware cutout
- Chapter 5 (Room Model) — room-scoped safety contexts
- Chapter 11 (Voice Pipeline) — full deliberative pipeline detail
- Chapter 12 (Security) — voice command authentication

### Key Sources
- `superinstance_dim03_ecosystem_map.md` (pincher description: <1ms reflex, 57 commits)
- `superinstance_artifact_synthesis.md` (Theme 5: two-tier safety, Decision AD-5)
- `superinstance_insight.md` (Insight 10: Pincher is safety-critical layer)
- `superinstance_requirements.md` (N2.5: reflex availability 99.99%, N3.1: offline core reflex)

---

## Chapter 11: Voice Pipeline

### Content Points
- End-to-end pipeline: Audio Capture -> VAD -> STT -> Intent Parsing -> LLM Routing -> TTS -> Audio Playback
- Pipeline stage options and tradeoffs at each step
- **VAD**: Silero VAD (<1ms/frame, most popular), WebRTC VAD (~10ms), SNR-based (~5ms on ESP32)
- **STT options**: Whisper Large V3 (2.01% WER, cloud), faster-whisper (4x speedup, GPU), whisper.cpp (CPU-optimized, Pi 5 at 3.5x real-time for tiny.en), Vosk (lightweight, 20+ languages)
- **LLM routing**: adaptive modality-aware offloading (MoA-Off style). Complexity estimator -> routing decision -> local or cloud execution.
- **Local LLM**: Llama 3.2 3B at ~28 tok/s (Jetson), TinyLlama 1.1B at 12-18 tok/s (Pi). Cloud: GPT-4o-mini (~100-200ms), GPT-4o Realtime API (~320ms e2e).
- **TTS**: Piper (CPU-friendly, 30+ languages), Kokoro (82M params, ~200ms), Chatterbox-Turbo (sub-200ms, emotion control)
- **Edge vs. cloud routing decision**: complexity score + network state + cost budget + latency SLA -> target endpoint
- Latency budget breakdown per stage with optimization strategies
- Streaming architecture: WebRTC for audio, WebSocket for text+metadata, gRPC for service-to-service
- Five deployment patterns: fully local (offline), Starlink hybrid (smart routing), GPT-4o Realtime (lowest latency), multi-modal edge (voice+vision+sensors), distributed mesh (multiple ESP32 nodes)
- Optimization strategies: persistent connections, request caching, model selection, prompt optimization, batch non-urgent requests, gRPC/Protobuf over REST

### Required Tables
| Table Title | Column Headers |
|---|---|
| Pipeline Stage Comparison | Stage, Local Option, Cloud Option, Latency, Quality |
| STT Options | Model, WER, Latency, Size, Platform, Best For |
| Local LLM Benchmarks | Model, Size, Quantization, Tokens/sec, Platform |
| TTS Options | Model, Parameters, Quality, Latency, License, Best For |
| Routing Decision Matrix | Condition, Edge Action, Cloud Action, Fallback |
| Latency Budget | Stage, Local Path, Cloud Path, Optimization |
| Deployment Patterns | Pattern, Latency, Pros, Cons, Best For |

### Required Diagrams
- **End-to-end voice pipeline**: Linear flow diagram showing all stages with timing annotations. Two parallel paths: local path (1-3s) and cloud path (700ms-2.5s).
- **LLM routing decision tree**: Diamond decision nodes for complexity, network state, cost budget leading to local LLM or cloud API.
- **Streaming architecture**: Protocol diagram showing WebRTC (audio), WebSocket (text), gRPC (services) with latency annotations.
- **Five deployment patterns**: Five side-by-side mini diagrams showing hardware arrangement for each pattern.
- **MoA-Off complexity estimator**: Block diagram showing inputs (text tokens, audio SNR, image resolution) -> estimator -> routing decision -> target endpoint.

### Code Examples
- Voice pipeline configuration JSON: VAD model, STT model, LLM endpoint, TTS model, routing policy
- MoA-Off routing decision function pseudocode
- Whisper.cpp initialization on Pi 5 with model selection
- GPT-4o Realtime API WebSocket session setup
- Piper TTS invocation with voice selection

### Cross-References
- Chapter 4 (Hardware) — hardware benchmarks per tier
- Chapter 7 (CopilotKit) — voice UI integration
- Chapter 10 (Safety) — reflex bypass for safety commands
- Chapter 13 (Starlink) — cloud routing over satellite

### Key Sources
- `superinstance_dim04_voice_edge_tech.md` (complete voice pipeline analysis)
- `superinstance_dim02_distributed_patterns.md` (Section 6: edge-cloud LLM orchestration)
- `superinstance_artifact_synthesis.md` (Decision AD-6: adaptive LLM offloading)

---

## Chapter 12: Security Model

### Content Points
- Capability-based access control (CapBAC) with W3C Verifiable Credentials
- DID-based identity: did:key method for offline verification, no central authority required
- Vessel identities as DID-backed service accounts: Forgemaster, CCC, JetsonClaw1, Oracle1 each as a DID
- Capability token format (JWT-style): iss (issuer DID), sub (subject DID), aud (target resource), capabilities array, room scope, nbf, exp, jti
- Hierarchical delegation: Vessel Owner -> Room Owner -> Agent -> Sub-agent. Each level delegates a subset.
- Revocation: capability tokens include jti for revocation; revocation propagates down delegation chain
- Verification flow: (1) check DID signature, (2) check expiration, (3) check audience matches resource, (4) check capabilities include requested operation, (5) verify proof of possession
- mTLS for transport security: QUIC tunnels with certificate pinning
- Voice command authentication: speaker identification to prevent unauthorized commands
- Asymmetric knowledge distribution: agents only access explicitly granted knowledge (strict isolation)
- Privacy properties: ephemeral DIDs prevent tracking, proof of possession prevents token theft
- CapBAC for pub/sub: publishers hold publish capabilities, subscribers hold subscribe capabilities, broker validates on each operation
- Security properties: distributed access control (no central point), attack surface reduction, MITM resilience

### Required Tables
| Table Title | Column Headers |
|---|---|
| Capability Token Schema | Field, Type, Description, Example |
| Delegation Hierarchy | Level, Can Delegate To, Maximum Depth, Example |
| Verification Steps | Step, Action, Failure Result, Time Budget |
| Security Properties | Property, Mechanism, Threat Mitigated, Verification |
| Pub/Sub Capabilities | Role, Publish Caps, Subscribe Caps, Broker Validation |
| Auth Methods by Interface | Interface, Method, Offline?, Strength, Use Case |

### Required Diagrams
- **Capability delegation tree**: Hierarchical tree from Vessel Owner root through Room Owners, Agents, Sub-agents with capability labels on edges.
- **Token verification flow**: Flowchart showing five verification steps with pass/fail branches.
- **Security architecture**: Full diagram showing mTLS transport, CapBAC at application layer, DID identities, with trust boundaries marked.
- **Pub/sub capability enforcement**: Publisher with capability token -> broker validates -> subscriber with capability token -> broker validates -> message delivery.
- **Voice auth pipeline**: Voice input -> speaker identification -> confidence check -> command authorization -> execution.

### Code Examples
- Capability token JWT example with all claims
- DID creation and resolution: did:key generation, document retrieval
- Token verification pseudocode: all five steps with error handling
- mTLS certificate configuration for QUIC tunnels
- Speaker identification API call and confidence threshold check

### Cross-References
- Chapter 6 (Agent Architecture) — agent identities and capability tokens
- Chapter 8 (Communication) — capability-based pub/sub
- Chapter 10 (Safety) — safety-critical command authorization
- Chapter 13 (Starlink) — transport security over satellite

### Key Sources
- `superinstance_dim02_distributed_patterns.md` (Section 9: capability-based security)
- `superinstance_artifact_synthesis.md` (Theme 6: Vessel identities, Decision AD-7)
- `superinstance_insight.md` (Insight 5: Vessel system as service mesh identity)
- `superinstance_requirements.md` (N4: security requirements)

---

## Chapter 13: Starlink and Cloud Integration

### Content Points
- Starlink architecture: LEO constellation at ~550km altitude, 25-50ms median RTT, 100-400 Mbps down, 10-40 Mbps up
- Starlink viability for LLM APIs: confirmed feasible with proper optimization
- Latency characteristics: median 25-50ms, best case 12-20ms, 99th percentile <65ms, spikes 100-500ms during handoffs
- Comparison with alternatives: Starlink LEO (excellent) vs. fiber (optimal) vs. cellular (good) vs. GEO satellite (unusable)
- Starlink Standard vs. Mini: power, dimensions, weight, cost, throughput comparison
- Duty cycling for power management: boot time 2-5min, minimum session 15-30min, 4x 30-min sessions/day = 100-150 Wh
- Optimization strategies: persistent HTTP/2 connections, request caching, GPT-4o-mini for simple commands, local LLM fallback, QoS/SQM (CAKE/fq_codel), Ethernet over WiFi
- Offline degradation modes: (1) full Starlink — cloud LLM primary, (2) degraded — local Jetson LLM, (3) offline — reflex-only operation
- LLM API routing: primary (DeepSeek V4 Flash), secondary (Claude via Anthropic), tertiary (local Hermes on Jetson)
- Bandwidth optimization: <1 MB/hour background traffic target, compressed sensor feeds, batched telemetry
- Store-and-forward queue: commands queued during outage, executed on reconnection with priority ordering
- Maritime-specific considerations: no cellular fallback in open ocean, Iridium as critical telemetry backup, environmental hardening

### Required Tables
| Table Title | Column Headers |
|---|---|
| Starlink Specifications | Parameter, Standard, Mini, Notes |
| Connectivity Comparison | Technology, Latency, Bandwidth, Suitability |
| Latency Budget over Starlink | Stage, Time, Notes |
| Optimization Strategies | Strategy, Impact, Effort, Priority |
| Offline Degradation Modes | Mode, Trigger, LLM Source, Capabilities |
| LLM API Routing | Priority, Provider, Model, Latency, Fallback |
| Power Budget (Starlink Mini) | Component, Consumption, Daily Total |

### Required Diagrams
- **Starlink connectivity architecture**: Starlink terminal -> RPi coordinator -> Jetson -> ESP32 nodes with latency annotations on each link. Show ground station and satellite handoff.
- **Offline degradation state machine**: Three states (connected, degraded, offline) with transition triggers and capability annotations.
- **LLM API routing flow**: Request -> complexity estimator -> network check -> route to DeepSeek/Claude/local Hermes with fallback arrows.
- **Store-and-forward queue**: Timeline showing queue filling during outage, draining on reconnection with priority labels.
- **Starlink power budget**: Power consumption diagram showing always-on vs. duty-cycled modes with daily Wh totals.

### Code Examples
- Starlink latency monitor: periodic ping, jitter calculation, connectivity status
- LLM routing function: provider selection with fallback chain
- Store-and-forward queue implementation: enqueue with priority, flush on connectivity
- Connection pool configuration: HTTP/2 keepalive, retry policy, timeout settings
- Offline mode detection and transition handler

### Cross-References
- Chapter 4 (Hardware) — Starlink as Tier 4 hardware
- Chapter 8 (Communication) — MQTT over Starlink, Zenoh bridging
- Chapter 11 (Voice Pipeline) — cloud LLM routing in voice pipeline
- Chapter 14 (Roadmap) — Starlink optimization milestones

### Key Sources
- `superinstance_dim04_voice_edge_tech.md` (Section 3: Starlink latency, Section 6: integration patterns)
- `superinstance_artifact_synthesis.md` (Theme 8: Starlink enables usually connected)
- `superinstance_insight.md` (Insight 4: Starlink changes edge-cloud boundary)
- `superinstance_requirements.md` (N3: offline operation, bandwidth limits)

---

## Chapter 14: Implementation Roadmap

### Content Points
- Phased approach: six milestones from basic chat to full vessel integration
- **Phase 1 — Foundation**: CopilotKit chat UI integrated with SuperInstanceAgent. DeepSeek routing. Basic Nebula/VoxelWorks tools. Target: chat works with text commands.
- **Phase 2 — Voice**: STT pipeline (whisper.cpp on Pi), TTS (Piper), wake word (ESP32). Target: basic voice control.
- **Phase 3 — Rooms**: Room navigation with context preservation. PLATO integration. Agent room transitions. Target: agent walks between all boat rooms.
- **Phase 4 — Hardware**: ESP32 + RPi + Jetson abstraction. Auto-detection. Tiered deployment. Target: all hardware tiers operational.
- **Phase 5 — Self-Assembly**: Discovery protocol, mesh formation, resource binding, failure recovery. Target: new nodes join in <60 seconds.
- **Phase 6 — Production**: Offline operation, security hardening, monitoring, documentation. Target: "ready to go" criteria met.
- Dependencies between phases: Phase 2 depends on 1, Phase 3 on 2, Phase 4 on 1, Phase 5 on 4, Phase 6 on all
- Risk assessment: single maintainer (high), fork currency (medium), hardware availability (medium), Starlink reliability (medium)
- Success criteria table: latency targets, offline duration, discovery time, command success rate
- Resource requirements: time estimates, hardware procurement, LLM API costs
- Development milestones with target dates and acceptance criteria

### Required Tables
| Table Title | Column Headers |
|---|---|
| Phase Overview | Phase, Deliverable, Dependencies, Target, Key Risk |
| Milestone Details | Milestone, Deliverables, Acceptance Criteria, Effort Estimate |
| Dependency Matrix | Phase, Requires, Required By, Parallel With |
| Risk Assessment | Risk, Impact, Probability, Mitigation, Owner |
| Success Criteria | Criterion, Measurement, Target, Verification |
| Resource Requirements | Resource, Phase, Cost, Lead Time |

### Required Diagrams
- **Roadmap timeline**: Gantt-style chart showing six phases with duration, dependencies, and milestones. Highlight parallel tracks (voice, hardware, rooms).
- **Dependency graph**: Directed graph showing phase dependencies with critical path highlighted.
- **Risk heat map**: 2x2 grid (Impact x Probability) with risk items positioned and color-coded.
- **Success criteria dashboard**: Mockup of the "ready to go" checklist with pass/fail indicators.

### Code Examples
- Phase acceptance test pseudocode: define test for each milestone
- Health check endpoint: `/api/health` returning status, version, uptime, connectivity
- Deployment script: one-command fleet deployment (`docker compose up -d`)

### Cross-References
- All previous chapters — this chapter synthesizes implementation ordering
- Chapter 3 (Ecosystem) — gap mitigation planning
- Chapter 7 (CopilotKit) — extension implementation priority

### Key Sources
- `superinstance_requirements.md` (development milestones, success criteria)
- `superinstance_artifact_synthesis.md` (gaps analysis: invention required vs. existing patterns)
- `superinstance_dim03_ecosystem_map.md` (maturity assessment, gap analysis)

---

## Chapter 15: Appendices

### Content Points
- **Appendix A: Hardware Benchmark Tables**: Comprehensive benchmarks for ESP32-S3 (wake word inference time, power modes), RPi 5 (LLM tokens/s, STT real-time factor, K3s resource usage), Jetson Orin Nano (LLM tokens/s per model, vision pipeline FPS, DeepStream streams), Starlink (latency statistics, bandwidth measurements).
- **Appendix B: Software Stack Reference**: OS/runtime, wake word, STT, LLM, TTS, vision, broker, monitoring for each hardware tier. Version numbers where applicable.
- **Appendix C: Complete Citation Index**: All academic papers, technical reports, GitHub repositories, vendor documentation, and community sources cited throughout the document. Organized by chapter.
- **Appendix D: Glossary**: Definitions of all specialized terms (A2A, A2UI, AG-UI, CapBAC, CBBA, CRDT, DDS, DID, DHT, EDT, EBT, FLUX, K3s, LLM, MCP, MoA-Off, MQTT, PLATO, QoS, Raft, RTT, SCR, STT, SWIM, TTS, VC, VAD, WASM, ZHC).
- **Appendix E: Repo Catalog**: Abbreviated catalog of the 50 most important repositories with name, vessel, language, purpose, and maturity status.
- **Appendix F: Capability Token Schema**: Full JSON schema for capability tokens with all fields, types, constraints, and examples.
- **Appendix G: State Bundle Schema**: Full JSON schema for agent migration state bundles.
- **Appendix H: MQTT Topic Reference**: Complete topic hierarchy with patterns, QoS recommendations, and retention policies.
- **Appendix I: API Reference**: Key API endpoints for SuperInstanceAgent, Nebula reflex engine, VoxelWorks gateway, and PLATO room API.

### Required Tables
| Table Title | Column Headers |
|---|---|
| Hardware Benchmarks | Device, Metric, Value, Conditions, Source |
| Software Stack per Tier | Tier, Component, Technology, Version, Notes |
| Citation Index | ID, Source, Type, URL/Reference, Chapter |
| Glossary | Term, Definition, First Use Chapter, Related Terms |
| Repo Catalog (Top 50) | Name, Vessel, Language, Purpose, Status |
| MQTT Topic Reference | Topic Pattern, Description, QoS, Retained? |

### Required Diagrams
- None required for appendices (reference material)

### Code Examples
- Full capability token JSON example
- Full state bundle JSON example
- MQTT topic subscription example with all relevant wildcards
- API curl examples for key endpoints

### Cross-References
- All previous chapters — appendices serve as reference for all

### Key Sources
- All four dimension research files
- `superinstance_artifact_synthesis.md` (critical data points, citation summary)
- `superinstance_dim03_ecosystem_map.md` (CATALOG.md — full repo listing)

---

## Content Element Summary

### Tables Required by Chapter

| Chapter | Table Count | Key Tables |
|---|---|---|
| 1. Executive Summary | 4 | Architecture at a Glance, Hardware Tier Summary, Ecosystem Scale, Chapter Guide |
| 2. Vision and Principles | 4 | Design Principles, Music-to-Architecture, Ternary Operations, MUD Primitives |
| 3. Ecosystem Overview | 6 | Vessel Profiles, 5-Layer Stack, Repo Families, Packages, Maturity, Gaps |
| 4. Hardware Architecture | 8 | Tier Specs, Capability Matrix, Power Consumption, Latency Budget, Starlink Disruptions, Jetson Benchmarks, BOM, Software Stack |
| 5. The Room Model | 6 | Room Inventory, Room Type Comparison, Transition Atomicity, MUD Mapping, MQTT Topics, PLATO Components |
| 6. Agent Architecture | 6 | Lifecycle Phases, Communication Patterns, State Bundle, Memory Files, Coordination Patterns, Vessel Mapping |
| 7. CopilotKit Integration | 5 | Package Inventory, Suitability Assessment, Extension Points, Tool Schemas, Frontend Hooks |
| 8. Communication Layer | 7 | Protocol Comparison, RMW Implementations, Gossip Variants, MQTT Topics, Stream Operators, Voice Protocols |
| 9. Self-Assembly Protocol | 6 | Discovery Tiers, Capability Format, Assembly Sequence, Resource Binding, Mesh States, Failure Recovery |
| 10. Safety and Reflex Layer | 5 | Two-Tier Classification, Safety Commands, Pincher vs. Deliberative, Latency Comparison, Fail-Safe States |
| 11. Voice Pipeline | 7 | Pipeline Stages, STT Options, LLM Benchmarks, TTS Options, Routing Matrix, Latency Budget, Deployment Patterns |
| 12. Security Model | 6 | Token Schema, Delegation Hierarchy, Verification Steps, Security Properties, Pub/Sub Caps, Auth Methods |
| 13. Starlink and Cloud | 7 | Starlink Specs, Connectivity Comparison, Latency Budget, Optimizations, Degradation Modes, API Routing, Power Budget |
| 14. Implementation Roadmap | 6 | Phase Overview, Milestones, Dependencies, Risk Assessment, Success Criteria, Resource Requirements |
| 15. Appendices | 7 | Benchmarks, Software Stack, Citations, Glossary, Repo Catalog, Topic Reference |

### Diagrams Required by Chapter

| Chapter | Diagram Count | Key Diagrams |
|---|---|---|
| 1. Executive Summary | 2 | System overview, document scope |
| 2. Vision and Principles | 4 | Jam session model, three-room convergence, ternary space, MUD architecture |
| 3. Ecosystem Overview | 4 | 5-layer stack, Vessel map, git-agent lifecycle, dependency graph |
| 4. Hardware Architecture | 5 | Four-tier architecture, vessel floor plan, power budget, Starlink latency, voice pipeline waterfall |
| 5. The Room Model | 5 | Three-room convergence, vessel floor plan, room transition sequence, SCR overlay, topic tree |
| 6. Agent Architecture | 5 | Lifecycle state machine, mobility sequence, communication topology, capability token, memory model |
| 7. CopilotKit Integration | 5 | Extended architecture, Fleet Copilot UI, tool-to-hardware mapping, extension roadmap, data flow |
| 8. Communication Layer | 6 | Communication scopes, latency spectrum, MQTT topic tree, gossip propagation, reactive pipeline, offline buffer |
| 9. Self-Assembly Protocol | 5 | Discovery architecture, assembly sequence, mesh topology, resource binding, failure recovery timeline |
| 10. Safety and Reflex Layer | 5 | Two-tier architecture, Pincher internals, E-stop circuit, command classification tree, failure mode map |
| 11. Voice Pipeline | 5 | E2E pipeline, LLM routing tree, streaming architecture, deployment patterns, MoA-Off estimator |
| 12. Security Model | 5 | Delegation tree, verification flow, security architecture, pub/sub enforcement, voice auth pipeline |
| 13. Starlink and Cloud | 5 | Starlink architecture, degradation state machine, API routing, store-and-forward queue, power budget |
| 14. Implementation Roadmap | 4 | Timeline Gantt, dependency graph, risk heat map, success dashboard |
| 15. Appendices | 0 | Reference material, no diagrams |

### Code Examples Required by Chapter

| Chapter | Code Examples | Languages/Formats |
|---|---|---|
| 1. Executive Summary | 0 | — |
| 2. Vision and Principles | 3 | Rust/C (ternary), YAML (room config) |
| 3. Ecosystem Overview | 3 | JSON (state bundle), Markdown (CHARTER), shell (git commands) |
| 4. Hardware Architecture | 4 | C (ESP32), YAML (K3s), Python (TensorRT-LLM), shell (network config) |
| 5. The Room Model | 4 | YAML (room def), pseudocode (portal API), XML (DDS), Python (PLATO query) |
| 6. Agent Architecture | 5 | JSON (state bundle), TypeScript (agent class), pseudocode (lifecycle), TypeScript (tool schema) |
| 7. CopilotKit Integration | 7 | TypeScript/React (provider, agent, tools, hooks, runtime, transport) |
| 8. Communication Layer | 6 | XML (DDS QoS), Python (MQTT), Rust (Zenoh), Python (RxPY), JSON (gossip), pseudocode (queue) |
| 9. Self-Assembly Protocol | 5 | pseudocode (mDNS, gossip, DHT, state machine, resource binding) |
| 10. Safety and Reflex Layer | 5 | Rust (Pincher rule), pseudocode (router, E-stop, confidence, state machine) |
| 11. Voice Pipeline | 6 | JSON (pipeline config), pseudocode (routing), C++ (whisper.cpp), Python (Realtime API), shell (Piper) |
| 12. Security Model | 5 | JSON (JWT claims), pseudocode (DID, verification, mTLS, speaker ID) |
| 13. Starlink and Cloud | 5 | Python (latency monitor), pseudocode (routing, queue, pool, offline handler) |
| 14. Implementation Roadmap | 3 | pseudocode (acceptance tests), shell (health check, deployment) |
| 15. Appendices | 4 | JSON (token, bundle), shell (MQTT, API curl) |

### Research Source Mapping

| Source File | Chapters Supported | Key Data Provided |
|---|---|---|
| `superinstance_dim01_copilotkit_analysis.md` | 7, 1, 3, 6, 14 | CopilotKit architecture, extension points, tool patterns, AG-UI protocol |
| `superinstance_dim02_distributed_patterns.md` | 5, 6, 8, 9, 10, 12, 2 | Agent mobility, discovery, gossip, consensus, rooms, CapBAC, SCR |
| `superinstance_dim03_ecosystem_map.md` | 3, 6, 10, 15 | Ecosystem scale, Vessels, repos, git-agent lifecycle, pincher, 5-layer stack |
| `superinstance_dim04_voice_edge_tech.md` | 4, 8, 11, 13 | Hardware benchmarks, Starlink analysis, voice pipeline, STT/LLM/TTS options |
| `superinstance_artifact_synthesis.md` | 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14 | All 8 themes, 7 architectural decisions, 14 patterns with confidence |
| `superinstance_insight.md` | 2, 3, 5, 6, 7, 10, 11, 12, 13 | 10 cross-dimension insights with confidence levels |
| `superinstance_cross_verification.md` | 1, 2, 4, 7, 8 | High/medium confidence findings, conflict zones |
| `superinstance_requirements.md` | 1, 4, 5, 6, 9, 10, 11, 13, 14 | Functional/non-functional requirements, success criteria, milestones |
