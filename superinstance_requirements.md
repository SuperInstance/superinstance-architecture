# SuperInstance-Copilot: Comprehensive Requirements Document

## 1. Functional Requirements

### 1.1 Voice Command Interface

| ID | Requirement | Priority | Notes |
|----|------------|----------|-------|
| F1.1 | Accept natural language voice commands from boat operators | P0 | Must work in marine environment (wind, engine noise) |
| F1.2 | Route voice commands to LLM APIs (DeepSeek V4 Flash, Claude, Hermes) | P0 | Via Starlink satellite connection |
| F1.3 | Parse voice commands into structured intents for agent dispatch | P0 | "Set autopilot to heading 195" -> structured command |
| F1.4 | Provide voice feedback for command acknowledgment and status | P1 | Text-to-speech for confirmation |
| F1.5 | Support command chaining: "check engine room then show me the back deck" | P2 | Sequential room navigation via voice |
| F1.6 | Maintain command history and allow reference ("show me what happened 5 minutes ago") | P2 | Temporal query support |

### 1.2 Agent Mobility and Room Navigation

| ID | Requirement | Priority | Notes |
|----|------------|----------|-------|
| F2.1 | Decompose every application into agent-navigable "rooms" | P0 | MUD (Multi-User Dungeon) paradigm |
| F2.2 | Allow agents to "walk" between rooms to access different context | P0 | Ex: wheelhouse -> engine room -> back deck |
| F2.3 | Maintain agent presence and state within rooms | P0 | Agent location, current task, available senses |
| F2.4 | Support room-specific sensor feeds (sonar, GPS, engine temp, camera) | P0 | Each room exposes its own tick stream |
| F2.5 | Enable context handoff when agents move between rooms | P1 | Preserve working memory across room transitions |
| F2.6 | Support multiple simultaneous agents in the same room | P1 | Shared context with individual agent partitions |
| F2.7 | Map physical boat spaces to virtual rooms (wheelhouse, engine room, back deck, galley, crow's nest) | P0 | Physical-to-virtual mapping |

### 1.3 Hardware Abstraction and Tiered Deployment

| ID | Requirement | Priority | Notes |
|----|------------|----------|-------|
| F3.1 | Abstract autopilot and throttle controls on ESP32 | P0 | GPIO/PWM interfacing |
| F3.2 | Run backdeck camera processing on Jetson (CUDA-accelerated) | P0 | Object detection, scene analysis |
| F3.3 | Deploy reflex agents on Raspberry Pi | P1 | Pincher runtime, ~700ms fast-path matching |
| F3.4 | Auto-detect hardware capabilities at runtime | P1 | Probe for GPU, RAM, available sensors |
| F3.5 | Degrade gracefully when hardware tier is insufficient | P1 | Fallback from CUDA to CPU, from LLM to reflex |
| F3.6 | Support $3 microcontroller (ESP32-C3) as reflex endpoint | P2 | Minimal reflex matching on bare metal |
| F3.7 | Provision hardware resources automatically based on workload | P2 | Self-assembly: pull compute from allowed nodes |

### 1.4 Self-Assembly and Resource Discovery

| ID | Requirement | Priority | Notes |
|----|------------|----------|-------|
| F4.1 | Auto-discover available hardware nodes on the local network | P0 | Mesh discovery protocol |
| F4.2 | Request compute/sensor resources from allowed peers | P0 | Agent can say "I need a camera feed" and find one |
| F4.3 | Negotiate resource allocation based on capability advertisements | P1 | Nodes publish what they can do |
| F4.4 | Self-heal when a node goes offline by redistributing its tasks | P1 | Swarm re-coordination on node failure |
| F4.5 | Assemble multi-node pipelines for complex tasks | P2 | Camera (Jetson) -> Object detect (Jetson) -> Decision (LLM) |
| F4.6 | Maintain provenance tracking for all assembled resources | P2 | Origin-centric knowledge: who provided what |

### 1.5 Fleet Coordination and Swarm Management

| ID | Requirement | Priority | Notes |
|----|------------|----------|-------|
| F5.1 | Orchestrate up to 100 parallel agents via Swarm MCP | P0 | Multi-agent parallel execution |
| F5.2 | Distribute knowledge asymmetrically (agents only know what they need) | P0 | Security and efficiency |
| F5.3 | Support origin-centric coordination with provenance tracking | P1 | Every decision traced to its source |
| F5.4 | Resolve conflicts between agent outputs (voting, weighted, hierarchical) | P1 | Multiple conflict resolution strategies |
| F5.5 | Enable adaptive task allocation based on agent performance | P2 | Dynamic reassignment |
| F5.6 | Support hierarchical agent structures (parent-child relationships) | P2 | Fleet -> Vessel -> Room -> Agent |
| F5.7 | Implement T-Minus simulation-first coordination (70x fewer messages than polling) | P1 | Predict arrival, confirm once |
| F5.8 | Enable agent-to-agent communication through git commits (I2I pattern) | P2 | "Iron sharpens iron. We don't talk, we commit." |

### 1.6 CopilotKit Integration and UI Layer

| ID | Requirement | Priority | Notes |
|----|------------|----------|-------|
| F6.1 | Integrate CopilotKit React chat UI as primary fleet interface | P0 | Forked from CopilotKit/CopilotKit |
| F6.2 | Connect chat UI to SuperInstanceAgent (custom runtime) | P0 | AbstractAgent subclass with fleet knowledge |
| F6.3 | Display live system status dashboard (Nebula, VoxelWorks, CraftMind, etc.) | P0 | Auto-refreshing health indicators |
| F6.4 | Provide quick action buttons for common fleet queries | P1 | One-click status checks |
| F6.5 | Support dark editor-style UI theme (fleet theme) | P1 | Marine-friendly low-light interface |
| F6.6 | Render agent responses with structured data (tables, charts, maps) | P2 | Generative UI for fleet data |
| F6.7 | Expose fleet tools to LLM (get_nebula_status, query_voxelworks, etc.) | P0 | Tool-use pattern for system interaction |
| F6.8 | Route agent reasoning through DeepSeek V4 Flash via DeepInfra | P0 | 1M context, fast streaming |

### 1.7 5-Layer Stack Integration

| ID | Requirement | Priority | Notes |
|----|------------|----------|-------|
| F7.1 | **L1 - Deterministic Silicon**: Integrate ternary math crates and Eisenstein lattice computations | P0 | constraint-theory-core, origin-centric-math |
| F7.2 | **L2 - Reflex Runtime**: Connect Pincher and Nebula reflex engines | P0 | ~700ms edge reflex matching |
| F7.3 | **L3 - Form & Placement**: Support cellular automata, music forms (Tensor MIDI) | P1 | Form synthesis and placement |
| F7.4 | **L4 - IR & Compilation**: Integrate cellforge and cognitive JIT | P1 | FLUX code translation across 12 languages |
| F7.5 | **L5 - Cloud LLM**: Connect to DeepSeek, Claude, Hermes, and other LLM providers | P0 | Multi-provider fallback |
| F7.6 | Allow the copilot to explain any layer of the stack on demand | P1 | Architecture introspection |
| F7.7 | Enable cross-layer debugging and performance monitoring | P2 | Trace execution through all 5 layers |

### 1.8 Cross-Platform and Multi-Language Support

| ID | Requirement | Priority | Notes |
|----|------------|----------|-------|
| F8.1 | Provide Rust implementation for runtime/embedded | P0 | Primary language for L1-L3 |
| F8.2 | Provide Python implementation for lab/data science | P0 | Primary language for L4-L5 |
| F8.3 | Provide TypeScript implementation for browser/UI | P0 | CopilotKit integration, web interfaces |
| F8.4 | Provide C implementation for microcontroller targets | P1 | ESP32 bare-metal reflexes |
| F8.5 | Provide CUDA kernels for GPU acceleration on Jetson | P1 | Parallel computation, vision processing |
| F8.6 | Maintain API parity across all language implementations | P1 | Same interface, different runtime |

---

## 2. Non-Functional Requirements

### 2.1 Performance

| ID | Requirement | Target | Notes |
|----|------------|--------|-------|
| N1.1 | Reflex engine fast-path latency | < 700ms | Nebula edge matching |
| N1.2 | Voice command round-trip (voice to action) | < 3 seconds | End-to-end including LLM |
| N1.3 | Agent room transition time | < 500ms | Context handoff overhead |
| N1.4 | System status dashboard refresh | 30-second interval | Auto-check with manual override |
| N1.5 | LLM token streaming latency | < 100ms first token | DeepSeek V4 Flash via DeepInfra |
| N1.6 | Mesh network node discovery | < 10 seconds | Fleet auto-assembly |
| N1.7 | Concurrent agent support | 100+ parallel | Swarm MCP capacity |
| N1.8 | Message reduction vs polling | 70x fewer | T-Minus prediction-first |

### 2.2 Reliability and Availability

| ID | Requirement | Target | Notes |
|----|------------|--------|-------|
| N2.1 | System uptime (single node) | 99.9% | Graceful degradation on failure |
| N2.2 | Voice command success rate in marine environment | > 95% | Wind and engine noise tolerance |
| N2.3 | Agent recovery from node failure | < 5 seconds | Automatic task redistribution |
| N2.4 | Network partition tolerance | Split-brain safe | Asymmetric knowledge isolation |
| N2.5 | Reflex engine availability | 99.99% | Critical safety systems |

### 2.3 Offline and Limited-Connectivity Operation

| ID | Requirement | Target | Notes |
|----|------------|--------|-------|
| N3.1 | Core reflex functions work without internet | Full offline | L1-L2 operate autonomously |
| N3.2 | Voice commands cache and retry on Starlink outage | 1-hour buffer | Queue commands, execute when connected |
| N3.3 | Local LLM fallback for common commands | 80% coverage | Hermes or similar on Jetson |
| N3.4 | Agent state persists across connectivity drops | Seamless | Local SQLite or similar |
| N3.5 | Starlink bandwidth optimization | < 1 MB/hour background | Minimal keep-alive traffic |

### 2.4 Security

| ID | Requirement | Target | Notes |
|----|------------|--------|-------|
| N4.1 | mTLS for all inter-node communication | Mandatory | QUIC tunnels with certificate pinning |
| N4.2 | Asymmetric knowledge distribution enforced | Strict isolation | Agents only access explicitly granted knowledge |
| N4.3 | API key management for LLM providers | Secure vault | DeepInfra keys, Claude keys |
| N4.4 | Voice command authentication | Speaker identification | Prevent unauthorized commands |
| N4.5 | Resource access policies | Configurable | Allowed/denied peer lists |
| N4.6 | Audit logging for all agent actions | Complete trace | Origin-centric provenance |

### 2.5 Maintainability

| ID | Requirement | Target | Notes |
|----|------------|--------|-------|
| N5.1 | Single maintainer operable | Yes | Casey, commercial fisherman |
| N5.2 | Automated testing coverage | > 80% | Unit, integration, e2e |
| N5.3 | Self-documenting agent responses | Yes | Architecture explanation built-in |
| N5.4 | Hot-reload configuration | Yes | No restart for policy changes |
| N5.5 | Health check endpoints for all services | Standardized | /api/health on all nodes |

---

## 3. Constraints

### 3.1 Hardware Constraints

| Constraint | Impact |
|-----------|--------|
| **ESP32** ($3 tier) | 520KB RAM, 240MHz dual-core, no FPU. Only lightweight reflex matching. WiFi/BT only. |
| **Raspberry Pi** ($35 tier) | 1-8GB RAM, ARM Cortex-A72. Can run Pincher runtime, limited LLM inference. USB/Ethernet. |
| **NVIDIA Jetson** ($300-1000 tier) | 4-16GB shared memory, CUDA cores. Full vision pipeline, local LLM (Hermes), GPU kernels. Primary edge node. |
| **Boat power budget** | All hardware must run on 12V DC marine power. Peak consumption < 200W for compute stack. |
| **No cloud dependency at sea** | Starlink is primary but intermittent. Core safety functions must work offline. |

### 3.2 Network Constraints

| Constraint | Impact |
|-----------|--------|
| **Starlink satellite** | 20-100ms latency, occasional drops. Not guaranteed at high latitudes or in weather. |
| **Local mesh** | WiFi/Ethernet on boat. Must self-configure without DHCP infrastructure. |
| **Bandwidth limits** | Starlink may be rate-limited. Compressed sensor feeds, batched telemetry. |
| **No cellular fallback** | Open ocean has no cellular coverage. Starlink or VHF only. |

### 3.3 Operational Constraints

| Constraint | Impact |
|-----------|--------|
| **Single maintainer** | All systems must be self-healing, self-documenting, and require minimal intervention. |
| **Marine environment** | Salt, moisture, vibration, temperature extremes. Hardware must be ruggedized or housed. |
| **24/7 operation** | Fishing vessels run continuously. No scheduled maintenance windows. |
| **Regulatory** | VHF marine radio, AIS, and safety systems must not be interfered with. |
| **Safety-critical** | Autopilot and throttle commands must be verified before execution. Human override always available. |

### 3.4 Organizational Constraints

| Constraint | Impact |
|-----------|--------|
| **3,200+ existing repos** | CopilotKit integration must map across existing ecosystem, not replace it. |
| **5-layer stack is fixed** | Architecture must respect L1-L5 separation. |
| **Open source first** | All components must be open-source licensed (MIT/Apache-2.0). |
| **Multi-language** | Rust, Python, TypeScript, C, CUDA - no single language mandate. |

---

## 4. Integration Requirements

### 4.1 CopilotKit to 5-Layer Stack Mapping

```
+-------------------------------------------------------------+
|                    COPILOTKIT UI LAYER                       |
|  React Chat UI | Status Dashboard | Quick Actions | Voice    |
+-------------------------------------------------------------+
|                    SUPERINSTANCE AGENT                       |
|  System Prompt | Fleet Tools | DeepSeek Router | Context     |
+----------+----------+----------+----------+------------------+
|   L5     |   L4     |   L3     |   L2     |       L1        |
| Cloud    | IR &     | Form &   | Reflex   |  Deterministic  |
| LLM      | Compile  | Placement| Runtime  |   Silicon       |
|----------|----------|----------|----------|-----------------|
| DeepSeek | FLUX     | Tensor   | Nebula   | constraint-     |
| Claude   | cellforge| MIDI     | Pincher  | theory-core     |
| Hermes   | cognitive| cellular | ~700ms   | ternary crates  |
|          | JIT      | automata | reflex   | Eisenstein math |
+----------+----------+----------+----------+-----------------+
|                    HARDWARE ABSTRACTION                      |
|  ESP32 (GPIO)  |  RPi (USB/Eth)  |  Jetson (CUDA/AI)       |
+-------------------------------------------------------------+
|                    CONNECTIVITY                              |
|  Starlink (WAN)  |  WiFi Mesh (LAN)  |  VHF ( Marine )       |
+-------------------------------------------------------------+
```

### 4.2 Specific Integration Points

| From | To | Protocol | Data |
|------|-----|----------|------|
| CopilotKit Chat | SuperInstanceAgent | @ag-ui/client (AG-UI) | Messages, tool calls |
| SuperInstanceAgent | DeepSeek V4 Flash | OpenAI-compatible API | Streaming completions |
| SuperInstanceAgent | Nebula Reflex | HTTP GET/POST | Reflex queries, status |
| SuperInstanceAgent | VoxelWorks Gateway | HTTP GET | Room status, health |
| SuperInstanceAgent | CraftMind Ranch | HTTP/WebSocket | Evolution data |
| Nebula | ESP32 Endpoints | MQTT/CoAP | Reflex triggers |
| Nebula | Jetson | gRPC/HTTP | Vision pipeline results |
| Swarm MCP | Claude Code | MCP Protocol | Parallel agent execution |
| FLUX Swarm | Agent Nodes | A2A Messaging | Cross-agent coordination |
| Equipment Swarm Coordinator | All Agents | Internal API | Task decomposition, knowledge partitioning |
| OpenConstruct | Agent Sandboxes | Docker/Containerd | Agent runtime provisioning |
| Cocapn | Git Repos | Git protocol | Agent memory, skill storage |

### 4.3 Voice Command Pipeline

```
[Voice Input] -> [Noise Cancellation] -> [STT (Whisper/Local)] 
    -> [Intent Parser (LLM)] -> [SuperInstanceAgent]
    -> [Tool Router] -> [Target System (Autopilot/Camera/etc.)]
    -> [Response Formatter] -> [TTS] -> [Voice Output]
```

### 4.4 Agent Room Navigation Protocol

```
[Voice: "go to engine room"] 
    -> [CopilotKit Chat] -> [SuperInstanceAgent]
    -> [Room Resolver: "engine-room"]
    -> [Context Save: current room] 
    -> [Context Load: engine-room ticks, agents, sensors]
    -> [Response: "Engine Room. Main engine at 1450 RPM. Coolant 195F."]
```

### 4.5 Self-Assembly Discovery Protocol

```
[Node Boots] -> [Multicast: capability advertisement]
    -> [Fleet Registry Update] -> [Mesh Graph Recompute]
    -> [Knowledge Redistribution] -> [Agent Rebalancing]
    -> [CopilotKit Dashboard Update]
```

---

## 5. Success Criteria

### 5.1 "Ready to Go" Definition

The SuperInstance-Copilot is "ready to go" when:

| Criterion | Measurement | Target |
|-----------|-------------|--------|
| Voice command to boat action | End-to-end latency | < 3 seconds |
| Reflex response for common commands | Nebula fast-path | < 700ms |
| Agent can navigate all boat rooms | Room transition | Seamless with context preservation |
| System operates without internet | Offline duration | 24+ hours with full reflex capability |
| New hardware auto-joins fleet | Discovery to operational | < 60 seconds |
| Single voice command chains multiple systems | Command complexity | "Set course 195, check engine, show back deck" executes all three |
| Copilot explains its own architecture | Query coverage | 100% of 5-layer stack explainable |
| System health visible at a glance | Dashboard accuracy | All subsystems show real-time status |
| Agent failure recovery | Detection to replacement | < 5 seconds |
| Bandwidth over Starlink | Background traffic | < 1 MB/hour |

### 5.2 Demonstrable Scenarios

**Scenario A: Voice-Controlled Navigation**
1. Operator says "Set autopilot to heading 195, waypoint BRAVO"
2. Voice is captured, noise-filtered, converted to text
3. Intent parsed, command routed to autopilot agent
4. Autopilot agent (on ESP32) receives command via Nebula
5. Autopilot engages, confirmation spoken back
6. Dashboard updates to show "Autopilot ENGAGED, HDG 195"

**Scenario B: Multi-Room Context Walk**
1. Operator says "Check engine room"
2. Agent transitions to engine room, reads all sensor ticks
3. Response: "Engine room: 1450 RPM, coolant 195F, fuel 72%"
4. Operator says "Now show me the back deck"
5. Agent transitions, preserves engine room context
6. Response: "Back deck: pot #55 deployed, hydraulic pressure normal"

**Scenario C: Self-Assembly on Hardware Failure**
1. Jetson running backdeck vision goes offline
2. Swarm coordinator detects failure within 5 seconds
3. Camera feed redirected to backup processing node
4. Copilot announces: "Backdeck vision switched to backup node"
5. Operator can still issue voice commands for camera functions

**Scenario D: Offline Operation**
1. Starlink connection drops
2. Voice commands continue to work for L1-L2 functions (autopilot, sensors)
3. Commands requiring LLM are queued
4. When Starlink returns, queued commands execute, results spoken
5. No manual intervention required

### 5.3 Development Milestones

| Milestone | Deliverable | Target |
|-----------|-------------|--------|
| M1 | CopilotKit chat UI integrated with SuperInstanceAgent | Chat works with DeepSeek, Nebula, VoxelWorks tools |
| M2 | Voice command pipeline (STT -> Intent -> Action -> TTS) | Basic voice control on boat |
| M3 | Room navigation with context preservation | Agent can walk between all boat rooms |
| M4 | Hardware abstraction for ESP32 + RPi + Jetson | Auto-detection and tiered deployment |
| M5 | Self-assembly protocol | New nodes join fleet automatically |
| M6 | Offline operation | Core functions work without Starlink |
| M7 | Full boat integration | All systems voice-controllable, "ready to go" |

---

## 6. Architecture Summary

### 6.1 Component Diagram

```
                    +-------------------+
                    |   Operator Voice   |
                    +--------+----------+
                             |
                    +--------v----------+
                    |  CopilotKit Chat  |
                    |  (React UI/Voice) |
                    +--------+----------+
                             |
                    +--------v----------+
                    | SuperInstanceAgent|
                    | (Custom Runtime)  |
                    +--------+----------+
                             |
           +-----------------+-----------------+
           |                 |                 |
    +------v------+  +------v------+  +------v------+
    |   Nebula    |  |  DeepSeek   |  |  VoxelWorks |
    |Reflex Engine|  |   V4 Flash  |  |   Gateway   |
    +------+------+  +------+------+  +------+------+
           |                 |                 |
    +------v-----------------v-----------------v------+
    |           Equipment Swarm Coordinator            |
    |  (Task Decomposition | Knowledge | Consensus)    |
    +------+-----------------+-----------------+------+
           |                 |                 |
    +------v------+  +------v------+  +------v------+
    |   ESP32     |  |  Raspberry  |  |   Jetson    |
    | Autopilot/  |  |     Pi      |  |  (CUDA/AI)  |
    |  Throttle   |  | Pincher/    |  | Vision/LLM  |
    +-------------+  |   Mesh      |  +-------------+
                       +-------------+
                             |
                    +--------v----------+
                    |   Starlink/WiFi   |
                    |    Mesh Network   |
                    +-------------------+
```

### 6.2 Key Repositories in Scope

| Repository | Role | Language | Layer |
|-----------|------|----------|-------|
| SuperInstance/copilotkit | UI/glue layer (this fork) | TypeScript | UI |
| SuperInstance/equipment-swarm-coordinator | Multi-agent orchestration | TypeScript | L2-L5 |
| SuperInstance/flux-swarm | Distributed agent coordination | Go | L4-L5 |
| SuperInstance/cocapn | Repo-first agent runtime | Python | L5 |
| SuperInstance/OpenConstruct | Agent onboarding sandbox | Rust | L2-L4 |
| SuperInstance/constraint-theory-core | Geometric constraint theory | Rust | L1 |
| SuperInstance/Equipment-Swarm-Coordinator | Asymmetric knowledge distribution | TypeScript | L2-L5 |
| Nebula (fleet-murmur-worker) | Reflex engine edge | TypeScript/Cloudflare | L2 |
| VoxelWorks | Game dev gateway | TypeScript | L3 |

---

*Document generated from analysis of SuperInstance ecosystem, CopilotKit fork, and user requirements.*
*Author: Casey Digennaro, Sitka, Alaska*
*Context: F/V Quantum, Bering Sea Pacific Cod Season*
