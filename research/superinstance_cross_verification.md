# Cross-Verification: SuperInstance Architecture Research

## High Confidence Findings (Confirmed by >=2 agents)

1. **CopilotKit is excellent as UI layer but insufficient as distributed glue** — Confirmed by Repo_Analyzer (needs MQTT/NATS, distributed state, service discovery) and Pattern_Researcher (needs agent mobility, gossip protocols)

2. **Weak mobility with state serialization is the right agent mobility pattern** — Confirmed by Pattern_Researcher (historical platforms all use weak mobility) and Voice_Edge_Researcher (WASM module migration on Jetson feasible)

3. **Three-tier discovery: mDNS local + gossip vessel-wide + DHT global** — Confirmed by Pattern_Researcher and Ecosystem_Mapper (PLATO rooms concept aligns)

4. **ESP32 -> RPi -> Jetson -> Starlink -> Cloud is correct hardware tiering** — Confirmed by Voice_Edge_Researcher (benchmarks) and Pattern_Researcher (capability-based security matches)

5. **Starlink is viable for LLM APIs (25-50ms RTT)** — Confirmed by Voice_Edge_Researcher with multiple sources

6. **The SuperInstance ecosystem has 4,095+ repos (as of 2026-07-10) with 5-layer Rust core** — Confirmed by Ecosystem_Mapper with CATALOG.md evidence

7. **Music cognition as mathematical architecture** — Confirmed by Ecosystem_Mapper (not metaphor but actual math)

## Medium Confidence Findings

1. **Gossip protocols (SIR model) ideal for maritime** — Single authoritative source but well-supported theoretically

2. **MoA-Off achieves 30% latency reduction** — Single academic paper (2025)

3. **K3s on RPi for edge orchestration** — Multiple community sources but no production maritime deployment found

## Conflict Zones

1. **Ternary math adoption**: Ecosystem heavily invested in {-1,0,+1} ternary system; external research primarily uses binary/floating-point. This is a deliberate architectural choice by SuperInstance, not a conflict to resolve.

2. **CopilotKit fork currency**: Fork is 7 commits behind upstream. Need to maintain sync.

3. **Single maintainer vs scale**: 4,095 repos (as of 2026-07-10) with 1 contributor is a structural risk.
