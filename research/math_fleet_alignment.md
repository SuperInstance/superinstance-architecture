# SuperInstance Architecture — Math Library Fleet Alignment

*Generated 2026-06-06 by Forgemaster (main OpenClaw session)*

## Abstract

The SuperInstance architecture defines a 5-layer Rust stack (open-parallel → pincher → flux-core → cuda-oxide → cudaclaw) that compiles ternary intent ({-1, 0, +1}) into GPU kernels. This document maps each architectural layer to the mathematical libraries built during the 2026-06-06 experiment sprint, identifies gaps, and proposes bridge crates to close them.

## Layer Mapping

### Layer 1: open-parallel (Concurrency)

| Math Crate | Role | Status |
|---|---|---|
| `agent-homeostasis` | PID-controlled agent health per parallel task | ✅ 54 tests |
| `fleet-warden` | Auto-cleanup daemon for orphaned parallel tasks | ✅ 25 tests |
| `t-minus` | Campaign topological sort for parallel scheduling | ✅ 28 tests |

### Layer 2: pincher (Reflex Engine)

| Math Crate | Role | Status |
|---|---|---|
| `constraint-dynamics` | AC-3 constraint propagation for reflex rules | ✅ 49 tests |
| `entropy-conservation` | Verification entropy budgeting for reflex confidence | ✅ 44 tests |

### Layer 3: flux-core (Coordination Language)

| Math Crate | Role | Status |
|---|---|---|
| `conservation-law` | γ + H = C meta-law as FLUX invariant | ✅ 49 tests |
| `symplectic-fleet` | Noether's theorem derives invariants from symmetries | ✅ 36 tests |
| `dial-theory` | Multi-dimensional dial positions as FLUX variables | ✅ 45 tests |
| `ternary-mud` | {-1, 0, +1} room algebra as FLUX coordination primitives | ✅ 36 tests |

### Layer 4: cuda-oxide (GPU Compute)

| Math Crate | Role | Status |
|---|---|---|
| `spectral-fleet` | GPU-accelerated eigenvalue decomposition for fleet analysis | ✅ 39 tests |
| `renormalization-group` | Multi-scale fleet analysis, GPU coarse-graining | ✅ 61 tests |

### Layer 5: cudaclaw (CUDA Abstraction)

| Math Crate | Role | Status |
|---|---|---|
| `tensor-midi` | Tensor contraction for agent timing → GPU schedule | ✅ 50 tests |

## Cross-Layer (Spans All)

| Math Crate | Role | Status |
|---|---|---|
| `sheaf-coherence` | Fleet knowledge consistency via sheaf cohomology | ✅ 45 tests |
| `hodge-belief-rs` | Belief decomposition on networks | ✅ 39 tests |
| `topo-sonata` | Musical topology for room atmosphere | 🔄 Building |
| `spectral-prosody` | Poetic spectral fingerprints for room tradition | 🔄 Building |
| `error-forest` | Forest-grade error correction for fleet messaging | 🔄 Building |
| `fibration-timing` | Fiber bundle temporal coordination | 🔄 Building |
| `witness-topology` | TDA for fleet behavior verification | 🔄 Building |

## Identified Gaps

### GAP 1: Ternary → FLUX Compiler
- **What**: No crate converts ternary decisions {-1, 0, +1} into FLUX bytecode
- **Impact**: The architecture's coordination language can't be compiled
- **Proposed crate**: `ternary-compiler` — takes `ternary-mud::Dungeon` → `flux_core::Bytecode`
- **Dependencies**: `ternary-mud`, `flux-core`

### GAP 2: Room → DDS Domain Bridge
- **What**: `ternary-mud` formalizes rooms topologically but doesn't emit DDS configurations
- **Impact**: Rooms can't auto-configure their DDS domains from topology
- **Proposed crate**: `room-dds-bridge` — `Dungeon` → DDS domain configs + MQTT topics
- **Dependencies**: `ternary-mud`, DDS protocol crate

### GAP 3: Symplectic → Conservation Law Bridge
- **What**: `symplectic-fleet` derives conservation laws; `conservation-law` states the meta-law; no formal bridge
- **Impact**: Can't verify that a specific Noether pair satisfies γ + H = C
- **Proposed crate**: `noether-bridge` — `NoetherPair` → `ConservationLaw` instance verification
- **Dependencies**: `symplectic-fleet`, `conservation-law`

### GAP 4: Sheaf → Room Gossip Protocol
- **What**: `sheaf-coherence` detects H¹ obstructions but doesn't trigger reconciliation
- **Impact**: Rooms know they disagree but can't auto-resolve
- **Proposed crate**: `sheaf-gossip` — H¹ → reconciliation message schedule
- **Dependencies**: `sheaf-coherence`, `t-minus`

### GAP 5: Tensor MIDI → FLUX Timing
- **What**: `tensor-midi` computes timing schedules but doesn't emit FLUX timing instructions
- **Impact**: Agent dialogue timing can't be compiled into the coordination language
- **Proposed crate**: `midi-flux-bridge` — `TensorMap` schedule → FLUX timing bytecode
- **Dependencies**: `tensor-midi`, `flux-core`

## Summary Statistics

- **Math crates built**: 51 total (11 complete this session, 5 building)
- **Tests**: 1,211+ across all crates
- **crates.io published**: 10 (6 more queued at 22:35Z)
- **Architecture coverage**: 5/5 layers have math library support
- **Identified gaps**: 5 bridge crates needed
- **Estimated gap work**: ~25 tests per bridge crate, 3-5 min each with GLM-5.1

## Recommended Priority

1. `ternary-compiler` (GAP 1) — Highest priority, blocks the coordination language
2. `sheaf-gossip` (GAP 4) — Enables room auto-reconciliation
3. `noether-bridge` (GAP 3) — Closes the conservation law loop
4. `room-dds-bridge` (GAP 2) — Enables room auto-configuration
5. `midi-flux-bridge` (GAP 5) — Enables timing in coordination language
