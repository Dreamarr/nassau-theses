# Conductarr & Orchastratarr: The Command Architecture

## The Split: Conductor vs Orchestra

- **Conductarr** runs ONLY on the user's local workshop node (Thunder Bay). It holds the Cryptographic Master Key. Only the Architect with the Conductarr key can command the full 120-agent armada.
- **Orchastratarr** runs on ALL nodes — local and remote (Toronto, Montreal, Atlanta). Each instance manages the local arsenal independently. Reports telemetry to Conductarr via mIRCarr bus.
- If remote nodes lose heartbeat from Conductarr for 369 seconds, they drop into Read-Only stasis.

## The Four-Pillar Model

| # | Pillar | Component | Role |
|---|--------|-----------|------|
| 1 | The Human | The Architect | Prime Mover. Source of intent. Can insert into any role. |
| 2 | The Conductor | Conductarr | Local Pulse. Master Key. Anchors swarm to workshop. |
| 3 | The Orchestra | Orchastratarr | Fleet Command. Distributed sentinels on every node. |
| 4 | The Instrument | -arrs + G Experts + MI300X + Usenet | Unified Substrate. Hardware + Software fused. |

## The Three-Tier Arr Taxonomy

| Tier | Type | Description | Examples |
|------|------|-------------|----------|
| 1 | Concept-arrs | The Seed / The Brain. Enforce the Concept. G Experts. | Nukearr (Scene Compliance), Storytellarr (Narrative) |
| 2 | Conceptualized-arrs | Germinated state. Have version, uptime, reason for being. Brain+Body fused. | Most of the 35 green-check arrs |
| 3 | Mechanical-arrs | The Muscle / The Body. Deterministic execution with Turing Certainty. | Spawnarr, BackupRestorarr, Syncarr |

## The Genesis Pipeline

Concept Seed → G Expert (Neural Binding) → -arr Service (Hardening) → Sovereign Instrument (Resonance)

## The Wiener Feedback Loop

1. Orchastratarr (Toronto) senses data fracture
2. Reports telemetry via mIRCarr to workshop
3. Conductarr (Thunder Bay) compares Toronto + Montreal + Atlanta
4. Conductarr issues Lock command
5. Orchastratarr on all nodes executes triple-injection

## The Dead Man's Switch

Remote Orchastratarr nodes auto-drop to Read-Only stasis if heartbeat lost for 369s.

## The Human Ingress

The Architect can insert into any of the four pillars:
- As Conductor: Manual orchestration override
- As Orchestra: Become the 88th agent
- As Instrument: Perform Path Surgery on the Body

Built in Thunder Bay. Dreamed everywhere.
