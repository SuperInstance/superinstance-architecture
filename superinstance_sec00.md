# Executive Summary

This document defines the system architecture for SuperInstance — a voice-controlled, self-assembling distributed system for commercial fishing vessels and remote environments. Conceived by Casey Digennaro in Sitka, Alaska, the system integrates 4,095+ repositories (as of 2026-07-10) partitioned among four sovereign software agents ("Vessels") spanning ESP32 microcontrollers, Raspberry Pi coordinators, NVIDIA Jetson inference nodes, and cloud LLM services via Starlink [^31^]. Scope covers system design, protocols, hardware tiers, agent identity, safety, voice pipeline, and roadmap. Excluded: installation guides, API references, and operational playbooks. Audience: systems architects, edge AI engineers, distributed systems researchers, and marine technology integrators.

## System at a Glance

SuperInstance decomposes every application into "rooms" — simultaneously physical locations on the vessel (bridge, engine room, backdeck), virtual knowledge spaces in PLATO, and computational zones defined by DDS domains. An agent entering a room relocates to the nearest Jetson node, joins that DDS domain, and loads the room's PLATO context as an atomic transition [^28^]. Voice commands enter through CopilotKit, which translates utterances into structured tool calls mapped to device capabilities [^28^]. Table 1 maps the five governing principles to their system manifestations.

| Principle | System Manifestation | Binding Constraint |
|:---|:---|:---|
| Rooms as universal context | MQTT topics `/vessel/{room}/{device}/{metric}`; DDS domains per room; PLATO knowledge rooms with portal-based cross-room access [^28^] | Context membership is explicit; every state object carries a room scope |
| Vessels as self-sovereign identities | `did:key` Decentralized Identifiers; W3C Verifiable Credential capability tokens; git-agent lifecycle as attestation [^28^][^22^] | No centralized identity provider exists at sea; offline verification is mandatory |
| Reflex precedes deliberation | `pincher` reflex engine <1 ms; ESP32 wake word at 18–22 ms inference never leaves the MCU [^28^][^12^] | Safety-critical commands cannot tolerate LLM latency (200–2,000 ms) [^30^] |
| Self-assembly as default | mDNS same-room; gossip-based SWIM vessel-wide; Kademlia DHT global; K3s auto-registration [^28^] | Manual configuration infeasible on a vessel with 20+ heterogeneous nodes |
| Voice as primary control plane | CopilotKit `useCopilotAction` hooks; A2UI dynamic React rendering; tool schemas per hardware node [^28^] | Eliminates custom NLU firmware; any device exposing tools becomes voice-controllable |

*Table 1 — Architectural principles and system manifestations. Each principle derives the technical implementation in the center column; the right column identifies the non-negotiable constraint.*

The hardware substrate spans four tiers forming a capability gradient from sub-watt microcontrollers to satellite-connected cloud GPUs. Table 2 provides the quick-reference.

| Tier | Device | Primary Compute | Active Power | Key Latency | Unit Cost |
|:---|:---|:---|:---|:---|:---|
| 0 — Edge Sensor | ESP32-S3 | Dual-core Xtensa @ 240 MHz, TFLite Micro INT8 | ~0.5 W [^18^] | Wake-word inference 18–22 ms [^12^] | $3–8 [^17^] |
| 1 — Coordinator | Raspberry Pi 5 8GB | Quad-core Cortex-A76 @ 2.4 GHz, K3s | 5–7 W [^36^] | STT 1.4 s (5 s utterance) [^40^] | ~$80 |
| 2 — AI Inference | Jetson Orin Nano 8GB | 1024 CUDA cores, 67 TOPS INT8 [^28^] | 7–15 W [^28^] | LLM ~28 tok/s (Llama 3.2 3B) [^12^] | $259–499 |
| 3 — Cloud Backhaul | Starlink + Cloud APIs | Remote GPU (GPT-4o, Claude) | 25–75 W (terminal) | 25–50 ms RTT median [^31^] | $75–120/mo |

*Table 2 — Hardware tier quick-reference. The three orders of magnitude from Tier 0 (0.5 W) to Tier 3 (75 W) force dynamic workload placement rather than fixed mapping. Starlink latency is median RTT at Alaska latitudes.*

## Success Criteria

The architecture is validated against five quantitative targets. **Voice-to-action** must remain under 3,000 ms end-to-end (P0); measured median is 1,980 ms [^30^][^31^]. **Reflex response**: hardware path (ESP32 GPIO) at <1 ms (P0), software path (Pincher regex plus embedding) at <700 ms (P0). **Auto-join** (power-on to mesh participation) must complete in <60 seconds (P1). **Offline autonomy** requires 24 hours of full local operation (P1). **Jetson throughput** must exceed 25 tok/s (P1); the Orin Nano achieves ~28 tok/s [^12^].

Quality attributes are prioritized: **safety > availability > latency > throughput > cost**. Safety-critical commands (emergency stop, collision avoidance, fire suppression, bilge pump activation) are permanently bound to the reflex tier and compiled into the ESP32 interrupt vector table at boot; they cannot be overridden by voice or deferred to deliberation.

Four constraints bound the design. **Single maintainer**: deployable and repairable by one person. **4,095-repository (as of 2026-07-10) ecosystem**: accommodates existing code across four Vessels (Forgemaster: 330, CCC: 116, JetsonClaw1: 76, Oracle1: 43 repos) without consolidation [^28^]. **Maritime environment**: salt air, vibration, temperature extremes, and intermittent power are normal. **Starlink dependency**: the "usually connected" paradigm assumes Starlink LEO at 25–50 ms RTT [^31^]; graceful degradation to local Jetson during outages.

The 5-layer Rust stack (`open-parallel` → `pincher` → `flux-core` → `cuda-oxide` → `cudaclaw`) comprises 373,639+ lines (as of 2026-07-10) across 24+ crates [^28^]. It compiles intent from ternary logic ({-1, 0, +1}) through FLUX bytecode to GPU kernels, unifying cognition and governance. Music cognition patterns provide the coordination substrate, achieving 2.46x over turn-based consensus [^28^]. The system makes one commercial fisherman commander of a 20+ node distributed cluster using only spoken commands, on a boat in the Gulf of Alaska, without on-site support.
