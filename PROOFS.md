# What We Have Proven — With Receipts

**Document type:** Evidence ledger.
**Purpose:** Distinguish operational proof from theoretical claims.
**Updated:** 2026-05-21.

---

## PROVEN — Operational (receipts exist)

| # | Claim | Evidence | Receipt |
|---|-------|----------|---------|
| 1 | **3-node mesh operates** | swarm1, swarm2, kyle-node-1 all running Transportarr v2.2. 3 ISOs transferred P2P with SHA256 verification. | `SHA256SUMS` on swarm2 match kyle-node-1 build sums. |
| 2 | **Sovereign ledger active** | 169 Notarr entries. 0 malformed. 0 unsafe. 55 scene releases with SHA256SUMS+SFV+NFO. | `GET /health` on Notarr :8310 returns `count: 169, malformedSkippedCount: 0, unsafeSkippedCount: 0`. |
| 3 | **Node death survived** | giant1 (AMD MI300X) deprovisioned May 14, 2026. System reorganized to swarm1+swarm2. 47,792 files classified across 96 parallel lanes. 8 stalled arrs autonomously recovered. | `Standarr/docs/receipts/OVERNIGHT_20260520.md` |
| 4 | **Transfer resume works** | Transportarr v2.2 SQLite-backed state. Partial transfers survive restarts. PreverifyExistingChunks checks SHA256 on resume. | `Transportarr/src/Transportarr.Core/Swarm/PulsearrDistributor.cs` |
| 5 | **Sovereign voice deployed** | Actarr v3 running Piper TTS (120MB ONNX, FOSS, MIT). 11 teaser WAVs generated locally. Zero cloud TTS dependency. | `GET /actarr/health` returns `cloud_free: true, engine: "Piper TTS"`. Audio files at `/opt/dreamarr-actarr/static/teaser_*.wav`. |
| 6 | **Self-hosted search** | SearXNG on :8335. 7 engines. Zero tracking. | `GET /search?q=test` returns HTTP 200. `GET /stats` shows engine stats. |
| 7 | **149K events/sec bus** | Ergo IRCd + mIRCarr processing 149,000 events/sec. 5 channels. 30 blocked actions. Passive-observer confirmed. | `Standarr/docs/milestones/2026-05-20_MIRCARR_BUS_VERIFIED.md` |
| 8 | **24-arr concurrent health** | 24 endpoints probed simultaneously. 319ms total response. 22/24 returned 200 (Actarr uses /actarr/health path). | Today's stress test. |
| 9 | **ISO boots on real hardware** | seed.iso burned to USB. Booted on ASUS tower. Arch Linux UEFI bootloader confirmed. | ISO-001 Loggarr entry. `rootdelay=15` fix applied. |
| 10 | **Structured issue tracking** | Loggarr :8353 with POST /issues. ISO-001, HEALTH-001, ACTARR-001, OPSEC-001, 4x LAUNCH entries. | `GET /issues` returns 8 issues. |
| 11 | **Kokoro research confirmed #1 FOSS TTS 2026** | Searcharr + SearXNG + web sweep. Kokoro identified as best but blocked by Python 3.13. Piper deployed as best available. | `PLANARR_1000_LANE_FULL_STACK_AUDIT_V1.md` |
| 12 | **Canada SCIP $890M identified** | April 2026 launch. Aligned with Dreamarr mission. NOIC contact drafted. | `Standarr/docs/cedc/NOIC_CONTACT_DRAFT_V1.md` |

---

## THEORETICAL — Not yet proven with receipts

| # | Claim | Current State | What It Would Take to Prove |
|---|-------|---------------|---------------------------|
| 1 | **Semantic determinism with replay** | Conceptual. Never tested with identical inputs across 3 nodes. | Run same prompt through 3 identical models on 3 nodes simultaneously. Compare outputs byte-for-byte. |
| 2 | **Sovereign Resonance amplification** | Mathematical metaphor. No empirical measurement. | Measure operational metrics over time on local vs cloud hardware. Prove marginal cost is actually zero at scale. |
| 3 | **120-agent fully autonomous armada** | Cloud models used for agent orchestration. Local agents exist but don't form a self-contained 120-agent swarm without cloud LLM dependency. | Replace cloud LLMs with local models (Ollama, local Claude/OpenAI equivalents). Run 120-agent simulation entirely on local hardware. |
| 4 | **Deterministic provenance prevents all trust failures** | Provenance chain exists but has not been adversarially tested. | Red-team exercise: attempt to inject false provenance, verify detection. |
| 5 | **Full sovereignty (zero cloud dependency ever)** | Not yet. Development still uses cloud models for code generation and architecture. TTS is now sovereign. Search is sovereign. Mesh is sovereign. | Complete the transition. Only local models used for all development workflows. |

---

## HONEST DISCLOSURE

**These papers were compiled using an AMD MI300X (192GB VRAM) running locally in Thunder Bay.** The thesis text itself ran on sovereign hardware.

**DeepSeek v4 Pro via Ollama Cloud was the workhorse.** The vast majority of code generation, architecture design, standards drafting, swarm coordination, and multi-lane execution was driven through it. It orchestrated the 1000-lane audit, the Transportarr v2.2 deployment, the Actarr v3 sovereign voice migration, the Loggarr build, the Simulatarr attestation sweep, and the pre-launch test plan.

Additional cloud models used during development:

- **OpenAI** GPT-4o, GPT-4 Turbo (via ChatGPT Pro OAuth subscription)
- **Anthropic** Claude Sonnet (via OpenCode)
- **Google** Gemini (via web interface)
- **Moonshot AI** Kimi K2.6 (via OpenCode)

These models assisted with code generation, architecture design, standards drafting, and cross-platform verification. The papers describe a sovereign destination; the path there used every tool available, including cloud tools.

**Our goal is full sovereignty.** We are not there yet. The transition is in progress. TTS is now sovereign. Search is sovereign. The mesh is sovereign. Each day removes another cloud dependency.

---

*Built in Thunder Bay, Ontario. Home of the Sleeping Giant.*
*Honest. Receipt-backed. Sovereign as fast as we can build it.*
