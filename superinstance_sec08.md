## 8. Self-Assembly and Auto-Discovery

SuperInstance nodes form a coherent mesh without manual configuration. This chapter defines the protocols by which a node progresses from power-on to full participation, the mechanisms that let it discover peers across heterogeneous network tiers, and the resilience patterns that keep the mesh functional through partitions, departures, and degradation. The governing principle is that nodes self-assemble what they need from other resources they are allowed — a process that must complete within sixty seconds from cold boot [^1^].

### 8.1 Auto-Discovery Protocol

#### 8.1.1 Discovery Mechanism

Discovery operates at three spatial scopes, each matched to a protocol suited to its latency and bandwidth constraints.

**Same-room discovery** uses mDNS (Multicast DNS, also called Bonjour or Avahi). A booting node sends a multicast query on its local L2 broadcast domain, announcing its presence and requesting peer records. mDNS requires no infrastructure — no DHCP server, no broker — making it ideal for isolated cabins or bridge compartments where a device must identify neighbors before any centralized service is available [^2^].

**Vessel-wide discovery** uses a gossip protocol following the Susceptible-Infected-Recovered (SIR) epidemic model. Each node maintains a partial mesh view and exchanges digest messages with randomly selected peers. When a node learns of a new member, it enters the "infected" state and propagates that knowledge until a quorum acknowledges receipt, at which point it "recovers" and stops forwarding. SIR rumor mongering produces probabilistic consistency with bounded message volume — each update is finite, and propagation halts naturally [^3^]. The Epidemic Broadcast Tree (EBT) variant further reduces bandwidth through vector-clock-based request skipping [^3^].

**Global discovery** uses a Kademlia Distributed Hash Table (DHT) for lookups spanning vessels or requiring cloud-resident services, offering O(log n) lookup time across millions of nodes [^4^]. Local edge nodes cache DHT entries with a TTL, permitting vessel-only operation during satellite backhaul outages.

#### 8.1.2 Capability Advertisement

Each discovery message carries a capability descriptor in structured JSON:

```json
{
  "deviceType": "edge-jetson-orin",
  "vesselId": "jetsonclaw1",
  "availableFunctions": ["llm-inference", "voice-stt", "sonar-process"],
  "protocolVersions": {"discovery": "2.1", "gossip": "1.4"},
  "resources": {"gpu": {"cudaCores": 2048, "memoryMB": 8192}},
  "rooms": ["bridge", "backdeck"],
  "timestamp": "2026-01-15T09:23:17Z",
  "ttl": 300
}
```

The `deviceType` field maps to the four hardware tiers from Chapter 4, letting recipients infer baseline capabilities without negotiation. The `vesselId` anchors the node to a Vessel identity, which serves as the trust root for subsequent authentication [^5^].

#### 8.1.3 Network Scanning Strategies

Three scanning modes operate in parallel. A **periodic scan** runs a full mDNS sweep every 30 seconds and exchanges gossip digests with two random peers every 10 seconds, accommodating typical vessel churn from power-cycles and WiFi roaming. A **trigger-based scan** fires immediately on link-up events (Ethernet insertion, WiFi association), issuing five mDNS queries at 200 ms intervals to short-circuit the normal cadence. A **full mesh refresh** runs every 300 seconds as an anti-entropy pass: each node requests a complete membership list from its neighbors, correcting drift from lost packets or asymmetric partitions.

#### 8.1.4 Security During Discovery

Discovery messages are unsigned broadcasts, but a node cannot use discovered capabilities without presenting a valid capability token — a JWT whose `aud` claim matches the target resource, whose `iss` claim chains back to a Vessel identity, and whose `capabilities` array includes the requested operation [^6^]. Even if an attacker floods the network with rogue announcements, all join attempts fail at token validation. Discovery is open; binding is permissioned.

### 8.2 Node Lifecycle

#### 8.2.1 State Machine

A node progresses through eight discrete states. The following table defines each state, its entry condition, and the timeout governing transition.

| State | Entry Condition | Normal Exit Event | Timeout | Timeout Action |
|---|---|---|---|---|
| **OFFLINE** | Power-off or unrecoverable fault | Power-on signal | — | — |
| **DISCOVERING** | Bootloader hands control to discovery | At least one peer or seed found | 15 s | Retry expanded scan (all interfaces) |
| **NEGOTIATING** | Peer list non-empty | Capability exchange completed | 10 s | Return to DISCOVERING |
| **JOINING** | Capability tokens validated | Room assignment accepted | 20 s | Return to NEGOTIATING |
| **ACTIVE** | State sync completed | — (stable state) | — | — |
| **DEGRADED** | Local fault detected (e.g., GPU thermal throttle) | Fault cleared | 60 s | Transition to LEAVING |
| **LEAVING** | Graceful shutdown or repeated timeout | Goodbye acknowledged | 10 s | Force-offline |
| **OFFLINE** | Shutdown complete | — | — | — |

The state machine is implemented in the `construct-coordination` crate (80 commits, highest activity in the coordination family) [^5^]. Each transition is logged to the local append-only event log. Timeout values along the critical path from DISCOVERING through ACTIVE sum to 60 seconds, enforcing the boot-to-participation target [^1^].

**Diagram: Node State Machine.** States appear as rounded rectangles arranged vertically from OFFLINE at the bottom through ACTIVE at the top. Transitions are labeled arrows: OFFLINE→DISCOVERING on `power_on`; DISCOVERING→NEGOTIATING on `peer_found`; NEGOTIATING→JOINING on `cap_exchanged`; JOINING→ACTIVE after `room_assigned` and `state_synced`. DEGRADED sits beside ACTIVE with bidirectional arrows: `fault_detected` enters DEGRADED, `fault_cleared` returns to ACTIVE. LEAVING is reached from any non-OFFLINE state via `shutdown_signal` and drops to OFFLINE after `goodbye_ack` or forced timeout.

#### 8.2.2 Join Protocol

The transition from NEGOTIATING to ACTIVE follows a six-step sequence.

**Diagram: Auto-Discovery Sequence.** Six vertical lifelines — New Node, Local Peer, Room Leader, Vessel Identity, DHT Cache, Cloud Registry — with time flowing downward. (1) The new node sends mDNS queries; a same-VLAN peer responds with a gossip seed list. (2) The new node presents its capability token to the room leader, who verifies the signature against the Vessel's cached public key. (3) Both nodes exchange full capability descriptors. (4) The room leader assigns the node to functional rooms based on capability match and load [^7^]. (5) The node subscribes to the room MQTT topic tree and replays retained messages to build its state snapshot; if the room uses Raft, the node enters as a non-voting member until log catch-up [^8^]. (6) The leader marks the node ACTIVE in the gossip overlay.

#### 8.2.3 Trust Establishment

Trust is progressive, not binary. A newly joined node holds only the capabilities in its Vessel-issued token. As it demonstrates reliability through health checks and consistent state, the room leader may issue broader tokens. This model aligns with the EigenTrust reputation system: nodes accumulate trust scores through observed behavior, and higher scores unlock critical functions [^3^]. Nodes that fail health checks see their scores decay, and capabilities may be revoked through token expiration.

#### 8.2.4 Auto-Join Timing

The sixty-second target is enforced by cumulative state-machine timeouts. The `construct-coordination` test suite shows a Jetson Orin Nano on local WiFi typically completes the sequence in 12–18 seconds when at least one peer is present. In the worst case — no peers, no cached DHT entries, only satellite connectivity — the node falls back to local-only mode after the 15-second DISCOVERING timeout, operating standalone until connectivity improves.

### 8.3 Task Redistribution

#### 8.3.1 Triggers

Four events initiate redistribution. **Node join** triggers gentle rebalancing over 30 seconds as tasks matching the newcomer's strengths are migrated. **Node leave** triggers immediate reassignment to the next-best capability match; if no local substitute exists, the room leader may promote a candidate from another room [^7^]. **Load imbalance** is detected every 60 seconds: if the coefficient of variation across nodes exceeds 0.3, a Consensus-Based Bundle Algorithm (CBBA) auction assigns tasks to the best bidders [^9^]. **Priority change** allows high-priority tasks (e.g., safety-critical voice commands) to preempt lower-priority work; pincher handles safety commands directly in under one millisecond, bypassing redistribution [^5^].

#### 8.3.2 Task Migration Protocol

Migration follows four steps: (1) the source serializes the agent's state to a JSON bundle; (2) the source transfers the bundle over a Zenoh reliable stream with progress acknowledgement; (3) the target validates capability tokens and confirms hardware compatibility; (4) the target resumes from the serialized entry point, and the source drops its copy after acknowledgement. This is *weak mobility* — code and initialization data move, but the execution thread does not — requiring programmers to partition execution into resumable blocks [^10^].

#### 8.3.3 State Bundle Format

The migration unit is a state bundle:

```json
{
  "agentId": "uuid-v4",
  "agentType": "voice-assistant",
  "serializedState": {"conversationHistory": [...], "activeTasks": [...]},
  "capabilities": ["voice-recognition", "llm-inference"],
  "capabilityTokens": ["jwt-cap-token-1"],
  "timestamp": "2026-01-15T09:23:17Z",
  "ttl": 300
}
```

The `ttl` field determines bundle validity; expired bundles are discarded to prevent stale state from corrupting the mesh. The `agentType` maps to a WASM module or container image in the target's local registry.

#### 8.3.4 Migration Flow

**Diagram: State Bundle Migration.** Two nodes — Source (left) and Target (right) — connected by a horizontal arrow labeled "Zenoh reliable stream." The sequence: (1) Source calls `serializeState()`; (2) Source sends bundle with `transferId`; (3) Target validates tokens against its Vessel key; (4) Target calls `resumeOnArrival(bundle)`; (5) Target sends `ACK`; (6) Source destroys its local copy. On failure, the source retains the task and logs for retry.

### 8.4 Mesh Resilience

#### 8.4.1 Partition Handling

Partitions are expected in maritime environments. When a subnet is isolated — from a failed WiFi link, powered-down switch, or Starlink outage — each surviving subnet continues under local consensus. Raft clusters elect new leaders if the previous leader is unreachable, and gossip digests propagate among connected nodes [^8^]. Global progress halts during a partition: cross-room tasks requiring coordination with unreachable nodes are queued with `PARTITIONED` status and resumed on reconnection.

#### 8.4.2 Reconciliation

When partitions reconnect, nodes merge divergent state using Conflict-Free Replicated Data Types (CRDTs). OR-Set handles membership lists; LWW-Register handles scalar values. The merge function is commutative, associative, and idempotent, so the final state is independent of update arrival order [^11^]. For non-CRDT state (e.g., task assignments), conflict resolution uses timestamp priority; on ties, the lower `agentId` wins. Vessel identities mediate disputes that automated resolution cannot settle.

#### 8.4.3 Graceful Degradation

Graceful degradation means feature reduction rather than failure. On cloud disconnect, nodes drop to local-only mode: the LLM router stops offloading and routes inference to the local Jetson (3B–8B parameter models instead of 70B+ cloud models) [^12^]. Voice commands fall back to pincher's reflex database, handling common commands through regex and embedding matching in under one millisecond [^5^]. Room navigation continues because the room graph and DDS domain configuration are cached locally. The system publishes a `degradation_level` metric (0 = full, 1 = local-only, 2 = emergency) so agents and operators can adapt.

#### 8.4.4 24-Hour Offline Operation

The vessel remains operationally capable for at least 24 hours without cloud connectivity. Local LLM inference continues on the onboard Jetson with quantized models; accuracy degrades but remains functional for routine queries [^12^]. Pincher's reflex database operates entirely on-device with no network dependency, executing safety-critical commands in under one millisecond [^5^]. Agents move between rooms using cached topology and local DDS discovery; migrations across partitioned subnets are queued. Raft clusters within each partition continue making local decisions. Sensor readings and event logs are stored in local circular buffers and replayed to the cloud registry on reconnection. The 24-hour figure derives from the worst-case Starlink outage scenario combined with a typical 2,100 Wh LiFePO4 battery bank sustaining edge compute without solar recharge during an Arctic winter night [^13^].
