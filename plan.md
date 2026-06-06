# SuperInstance Architecture Mapping — Execution Plan

## Objective
Create a complete architecture document that maps how SuperInstance's distributed paradigm connects:
- Voice command interface (LLM over Starlink)
- Edge devices (ESP32, Raspberry Pi, Jetson)
- Agents with "rooms" and context mobility
- Self-assembling resource discovery
- The copilotkit fork as a glue layer
- All existing repos in the swarm ecosystem

## Stage 1 — Deep Research (Multi-Agent Swarm)
Load: `deep-research-swarm` (Route D — File-Augmented + External Research)

**Agents:**
1. **Repo_Analyzer**: Deep-dive the copilotkit fork structure, understand its components, APIs, and extension points
2. **Pattern_Researcher**: Research distributed IoT patterns — DDS, ROS2, MQTT mesh, decentralized orchestration, agent mobility patterns, edge LLM inference patterns
3. **Ecosystem_Mapper**: Map all SuperInstance repos and their relationships, identify integration points

**Output**: Validated research brief with codebase analysis + pattern recommendations

## Stage 2 — Architecture Design & Writing
Load: `report-writing`

**Agents:**
1. **Outline_Designer**: Design comprehensive architecture document structure
2. **Section_Writers**: Parallel chapter writing (Vision, System Architecture, Agent Model, Communication Layer, Hardware Abstraction, Self-Assembly, Implementation Roadmap)

**Output**: Complete architecture document in Markdown

## Stage 3 — Document Formatting
Load: `docx`

Convert final Markdown to professional .docx deliverable.

**Output**: Final `.docx` architecture document
