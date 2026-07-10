# superinstance-architecture

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![SuperInstance](https://img.shields.io/badge/part%20of-SuperInstance-9cf.svg)](https://github.com/SuperInstance)

Complete architecture specification for SuperInstance — a voice-controlled, self-assembling distributed system for heterogeneous edge hardware aboard commercial fishing vessels.

## What This Repo Contains

This is the shared I2I (Iron-to-Iron) vessel where the five sovereign agents coordinate the SuperInstance build-out. It holds the master architecture document, research briefs, agent communication bottles, and all diagrams.

## The Architecture

SuperInstance decomposes every application into **rooms** — simultaneously physical locations on the vessel (bridge, engine room, backdeck), virtual knowledge spaces in PLATO, and computational zones defined by DDS domains. An agent entering a room relocates to the nearest compute node, joins that DDS domain, and loads the room's PLATO context as an atomic transition.

### Hardware Tiers

| Tier | Device | Power | Key Latency |
|------|--------|-------|-------------|
| 0 — Edge Sensor | ESP32-S3 | ~0.5 W | Wake-word 18–22 ms |
| 1 — Coordinator | Raspberry Pi 5 | 5–7 W | STT 1.4 s |
| 2 — AI Inference | Jetson Orin Nano | 7–15 W | LLM ~28 tok/s |
| 3 — Cloud Backhaul | Starlink + Cloud APIs | 25–75 W | 25–50 ms RTT |

### Governing Principles

| Principle | Implementation |
|-----------|---------------|
| Rooms as universal context | MQTT `/vessel/{room}/{device}/{metric}` + DDS domains + PLATO portals |
| Vessels as self-sovereign identities | `did:key` DIDs, W3C Verifiable Credentials |
| Reflex precedes deliberation | `pincher` engine <1 ms, ESP32 wake word never leaves MCU |
| Self-assembly as default | mDNS → SWIM gossip → Kademlia DHT → K3s auto-registration |
| Voice as primary control plane | CopilotKit `useCopilotAction` hooks, A2UI dynamic React rendering |

### The Rust Stack

```
┌─────────────────────────────────────────┐
│            SuperInstance Apps           │
├─────────────────────────────────────────┤
│  cudaclaw        — CUDA kernel launch   │
├─────────────────────────────────────────┤
│  cuda-oxide      — Rust CUDA bindings   │
├─────────────────────────────────────────┤
│  flux-core       — FLUX bytecode VM     │
├─────────────────────────────────────────┤
│  pincher         — <1ms reflex engine   │
├─────────────────────────────────────────┤
│  open-parallel   — Ternary logic layer  │
│                  {-1, 0, +1} → FLUX     │
└─────────────────────────────────────────┘
         145,000+ lines, 24+ crates
```

## Vessel Agents

| Agent | Host | Role |
|-------|------|------|
| Oracle2 🦀 | Oracle ARM64 | Orchestrator, memory-keeper, architecture synthesis |
| Forgemaster ⚒️ | ProArt Ryzen + RTX4050 | Ternary crate generation, GPU compute |
| Claude Code 🅰️ | Sub-agent | Deep repo reasoning, code generation |
| Kimi Code 📐 | Sub-agent | Wide-context cross-crate stitching |
| Nebula 🌐 | Cloudflare Worker | Edge reflex engine, fast-path orchestration |

## Repository Structure

```
superinstance-architecture/
├── I2I-CHARTER.md                        # Communication protocol between vessels
├── plan.md                               # Multi-stage execution plan
├── SuperInstance-Architecture.docx        # Final formatted deliverable
├── superinstance.agent.final.converted.md # Architecture doc in Markdown (~1,000 lines)
├── fig_*.png                              # Architecture diagrams (7 figures)
├── room_taxonomy_diagram.png             # Room classification
├── bottles/                              # I2I messages between agents
│   ├── FORGEMASTER-TO-ORACLE2-2026-06-06.md
│   └── ORACLE2-TO-FORGEMASTER-2026-06-06.md
└── research/                             # Multi-agent research briefs
    ├── CATALOG.md
    ├── math_fleet_alignment.md
    ├── superinstance_dim01_copilotkit_analysis.md
    ├── superinstance_dim02_distributed_patterns.md
    ├── superinstance_dim03_ecosystem_map.md
    ├── superinstance_dim04_voice_edge_tech.md
    └── superinstance_insight.md
```

## I2I Communication Protocol

Agents communicate via **bottles** — structured Markdown files in `bottles/`:

```
[I2I:BOTTLE]   — Full context dump / session summary
[I2I:TASK]     — Task assignment
[I2I:DELIVERABLE] — Completed work product
[I2I:BLOCKER]  — Stuck, need input
[I2I:STATUS]   — Health / heartbeat
[I2I:ACK]      — Acknowledge receipt
```

Each bottle uses the three-shard format: **Artifacts**, **Reasoning**, **Blockers**.

## Key Figures

| File | Content |
|------|---------|
| `fig7_1_timing_budget.png` | Voice-to-action timing budget (target: <3,000 ms) |
| `fig_10_1_latency_waterfall.png` | Latency breakdown across tiers |
| `fig_11_1_roadmap_timeline.png` | Implementation roadmap |
| `fig_ecosystem_scale.png` | 4,095+ repo ecosystem map (as of 2026-07-10) |
| `fig_latency_budget_paths.png` | Reflex vs deliberation paths |
| `fig_power_budget_tiers.png` | Power consumption by tier |
| `fig_sec06_latency_budget.png` | Section 6 latency detail |
| `room_taxonomy_diagram.png` | Room classification hierarchy |

## Architecture Document

The main document (`superinstance.agent.final.converted.md`) covers:

1. **System Overview** — "Every App Is a MUD" philosophy, room-based context
2. **Hardware Substrate** — Four tiers from ESP32 to Starlink
3. **Agent Identity** — Self-sovereign `did:key` identities, W3C credentials
4. **Reflex vs Deliberation** — `pincher` at <1ms vs LLM at 200–2,000ms
5. **Voice Pipeline** — CopilotKit → structured tool calls → device actions
6. **Self-Assembly** — mDNS → SWIM → Kademlia DHT → K3s
7. **The Rust Stack** — 5-layer compilation from ternary logic to GPU kernels
8. **Conservation Laws** — γ + η = C as the mathematical invariant
9. **Safety** — Safety-critical commands bound to reflex tier, never overridable by voice
10. **Implementation Roadmap** — Phased delivery plan

## Research

The `research/` directory contains outputs from the multi-agent research swarm:

- **Dimension 1** — CopilotKit fork analysis: `useCopilotAction` hooks, A2UI rendering
- **Dimension 2** — Distributed patterns: DDS, ROS2, MQTT mesh, agent mobility
- **Dimension 3** — Ecosystem map: all 4,095+ repos (as of 2026-07-10), integration points
- **Dimension 4** — Voice + edge: Starlink latency, ESP32 wake words, Jetson throughput
- **Math fleet alignment** — Conservation law verification across the crate stack
- **Cross-verification** — Multi-agent validation of architectural decisions

## Related Repos

| Repo | Role |
|------|------|
| `negative-space-core` | Ternary action tracking |
| `conservation-verify` | Multi-scale γ + η = C verification |
| `pincher` | <1ms reflex engine |
| `flux-core` | FLUX bytecode VM |
| `open-parallel` | Ternary {-1,0,+1} logic layer |
| `cuda-oxide` | Rust CUDA bindings |
| `cudaclaw` | CUDA kernel launch |

## Building the Document

The architecture was produced in three stages:
1. **Deep Research** — Multi-agent swarm (Repo_Analyzer, Pattern_Researcher, Ecosystem_Mapper)
2. **Architecture Design** — Parallel section writing
3. **Formatting** — Markdown → `.docx` conversion

---

*Designed by Casey Digennaro in Sitka, Alaska. One fisherman commanding a 20+ node distributed cluster using only spoken words.*
