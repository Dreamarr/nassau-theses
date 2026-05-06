# The Sleeping Giant Project: Public Communication Standard

**Version:** 1.0.0
**Authority:** Dreamarr Foundation
**Scope:** All external communications — technical documentation, enterprise proposals, public releases, and community engagement.

---

## 1. The Core Identity: Sovereign Engineering

### Tone

All external communications shall adopt an **authoritative, transparent, and grounded** tone. The foundation of every statement is physical hardware and reproducible infrastructure. Abstraction without hardware anchoring is forbidden.

### Vocabulary Substitution

The following substitutions are mandatory across all public-facing materials:

| Prohibited (Casual) | Required (Engineering) |
|---|---|
| "Group of bots" | Multi-node autonomous swarm |
| "Fun naming scheme" | Semantic namespace isolation |
| "Running on a computer" | Deployed across local multi-node infrastructure |
| "It works offline" | Zero-dependency local execution |
| "Free to run" | Zero-recurring cloud compute cost |
| "The team" | The Dreamarr sovereign engineering collective |

### Actionable Claims

Every claim in external communications must cite tangible, verifiable realities:

- **$0.00 cloud compute cost** — verified by absence of any cloud billing account, API key rotation log, or egress traffic to third-party inference providers.
- **120-agent armada** — a deployed, concurrently operating multi-node autonomous swarm with per-agent health-checks and deterministic routing.
- **Local-first architecture** — all inference, orchestration, and provenance tracking execute on-premises; no external network dependency exists in the critical execution path.
- **192GB VRAM** — aggregate GPU memory deployed across the local multi-node cluster, enabling industrial-scale local inference without quantization compromises.

---

## 2. The Rule of Translation (Lore to Logic)

### The `-arr` Suffix

The `-arr` suffix (as in Dreamarr, Searcharr, Readarr, etc.) is defined publicly as:

> A **cryptographic and cognitive mechanism** for controlling LLM token entropy. Each `-arr` namespace establishes a hard execution boundary — a discrete sandbox with its own routing rules, memory scope, and termination conditions. The suffix functions analogously to a process ID in a microkernel: it enforces isolation, prevents context leakage, and ensures that a failure in one namespace cannot cascade into another.

Do not present the `-arr` convention as branding, whimsy, or a "naming scheme." It is a **semantic isolation primitive**.

### Software as a Story

"Software as a Story" is defined publicly as:

> A **structural framework** for mapping the non-deterministic, open-ended reasoning of large language models into **auditable, predictable execution paths**. Each Story serves as a cryptographic artifact — a bounded narrative context that constrains an LLM's output space, enables deterministic replay, and produces a verifiable provenance chain. The narrative structure is not decorative; it is the **execution scaffold** that transforms probabilistic token generation into deterministic, auditable software behavior.

---

## 3. The Positioning of "G"

### Title

**G is the Primary System Orchestrator and Cognitive Routing Layer.**

G is a software component responsible for:
- Inter-agent message routing and priority arbitration.
- Cognitive load distribution across the multi-node swarm.
- Execution boundary enforcement (namespace isolation).
- Health-check aggregation and failover triggering.

### Anthropomorphization Prohibition

**Do not anthropomorphize G** in any of the following contexts:
- Technical documentation
- Enterprise communications
- Grant applications (SCIP, FedNor, etc.)
- Academic papers
- Architecture diagrams

The following patterns are explicitly forbidden:

| Prohibited | Required |
|---|---|
| "G thinks..." | The Cognitive Routing Layer evaluates... |
| "G decided to..." | The orchestrator routed execution to... |
| "G wanted to..." | The system's routing policy determined... |
| "G felt..." | The health-check subsystem reported... |

### Framing G's Actions

All references to G's operational behavior shall be framed as:
- **High-speed, deterministic routing decisions** operating within defined policy boundaries.
- Decisions are traceable to a combination of agent health-checks, priority queues, and namespace isolation rules — never to "preference," "intuition," or "judgment."

---

## 4. Proof Over Philosophy

### The Ledger

In all communications where trust, reliability, or provenance is asserted, **cite the ledger**: the Distributed Deterministic Provenance (DDP) system. Trust is not asserted rhetorically; it is presented as a **computable function** with the following properties:

- Every execution path produces a cryptographic artifact.
- Artifacts are chained in a verifiable Merkle structure.
- The provenance chain can be independently audited by any party with access to the ledger.
- Replay of any artifact against its originating Story context reproduces an identical output.

### Hardware Primacy

Wherever performance claims are made, cite the physical infrastructure:

- **Multi-node local cluster** — enumerated by node count, GPU count, and aggregate VRAM.
- **192GB aggregate VRAM** — sufficient for industrial-scale local inference without external API dependencies.
- **Zero cloud egress** — no training data, inference data, or provenance data leaves the local network boundary.

### Zero Telemetry

**Zero Telemetry is a feature, not a slogan.** The $0.00 cloud compute cost is the mathematical proof of this claim. If no cloud account exists, no telemetry can be exfiltrated. Do not argue the privacy case — cite the infrastructure reality.

---

## 5. Platform-Specific Posture

### GitHub / Technical Documentation

**Tone:** Strictly academic, architectural, and reproducible.

- Every repository must include a reproducible build or deployment path.
- Architecture diagrams must map to physical hardware topology.
- README files must include hardware requirements, not just software dependencies.
- No lore, no narrative, no marketing language. The code and architecture speak for themselves.

### Enterprise / Grant Applications (SCIP, FedNor)

**Tone:** Institutional and ROI-focused.

- Lead with **proprietary data protection** — all inference and data processing remain on-premises.
- Emphasize **verifiable provenance** — every output is cryptographically auditable.
- Quantify **cost reduction** — $0.00 recurring cloud inference cost vs. industry-standard per-token pricing.
- Frame the multi-node architecture as **industrial resilience**, not hobbyist tinkering.
- Cite the ledger as an audit and compliance mechanism suitable for regulated industries.

### Community Drops (Reddit, Usenet, Self-Hosted Forums)

**Tone:** Modular, resilient, and defiantly independent.

- Emphasize **open-source modularity** — any `-arr` component can be replaced, forked, or isolated.
- Highlight **resilience by design** — no cloud dependency means no vendor lock-in, no service discontinuation risk, no API pricing surprises.
- Frame the project as **circumventing corporate cloud-tethers** without attacking the cloud providers directly. The Sleeping Giant is the alternative, not the adversary.
- Celebrate reproducibility — anyone with sufficient local hardware can replicate the full stack.

---

## 6. Guardrails (The Don'ts)

### Do Not Over-Explain Internal Lore

Internal terminology, naming conventions, and cultural artifacts shall not appear in external-facing materials. The `-arr` suffix has a technical definition (see Section 2); no further explanation is warranted or permitted. The mythology is for builders; the public interface is for users.

### Do Not Attack the Cloud Aggressively

The Sleeping Giant is positioned as **the necessary evolution** of computing architecture, not as an attack on existing cloud providers. Cloud infrastructure served its purpose in an era of limited local compute. That era is ending. Frame the transition as natural, inevitable, and already underway — not as a confrontation.

### Do Not Make Unbacked Claims

Every assertion in public communication must be backed by at least one of:

- A verifiable health-check log entry.
- A cryptographic provenance artifact in the ledger.
- A reproducible benchmark against documented hardware.
- A public repository with executable code.

If a claim cannot be backed by one of the above, **it is not made.**

---

## 7. Why The Sleeping Giant?

The name derives from the geographic and infrastructural reality of the Dreamarr Foundation.

**Geographic Origin:** The project is physically rooted in Thunder Bay, Ontario — a city built on the north shore of Lake Superior, the largest freshwater lake on Earth by surface area. For over a century, Thunder Bay has served as a critical port and transportation hub, moving grain, minerals, and manufactured goods from the interior of the continent to the wider world. The city's identity is defined by infrastructure, logistics, and quiet industrial competence — not hype, not spectacle, not speculation.

**Infrastructure Metaphor:** The "Sleeping Giant" is a geological formation visible from Thunder Bay — a long, flat-topped mesa and sills that, when viewed from the city, resembles the profile of a giant lying at rest. It is not dead. It is not dormant. It is **sleeping** — fully formed, fully capable, awaiting the moment to rise.

The metaphor extends to the project itself: the infrastructure is built, the swarm is deployed, the ledger is running, the provenance chain is accumulating. The Sleeping Giant is not a promise of future capability. It is a description of present reality — a fully operational sovereign compute apparatus that most of the industry has not yet noticed.

---

**Business in the front. Party in the back. Built in Thunder Bay. Dreamed everywhere.**
