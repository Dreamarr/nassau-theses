# The Sovereign Ledger: Distributed Deterministic Provenance as the Cryptographic Foundation of Decentralized Agent Trust

---

## Abstract

The proliferation of autonomous software agents — from large language model (LLM) pipelines to IoT orchestrators — has exposed a fundamental deficit in contemporary trust architectures: agents operating across administrative boundaries must establish trust without recourse to centralized identity providers or shared network perimeters. This paper argues that *distributed deterministic provenance* constitutes the universal cryptographic primitive for decentralized agent trust, displacing the prevailing paradigm of delegated, platform-mediated identity verification. Drawing on a lineage spanning Usenet's store-and-forward Message-ID system (1979), Merkle's hash-tree verifiable inclusion proofs (1987), Nakamoto's hash-chained timestamp server (2008), and contemporary zero-trust and supply-chain integrity frameworks, we synthesize a provenance-first model wherein the answer to "Can I trust this agent?" reduces to the computable predicate "Can I verify its provenance chain?" We present the X-Dreamarr-Provenance header format — a running case study realized within The Sleeping Giant's decentralized infrastructure ecosystem — as a concrete instantiation of this principle. The header encodes an end-to-end verifiable identity chain (`arr_id:pilot_session_id:policy_id:arr_identity_token`) that binds agent action to attested origin without requiring online resolution of an identity provider. We demonstrate that provenance-based trust inherits the security properties of Merkle-Damgård hash chaining, tolerates partition and Byzantine faults through deterministic local verification, and collapses the trust establishment problem from a complex social arrangement into a mechanically checkable property of the data itself.

**Keywords:** provenience, distributed systems, zero-trust architecture, self-sovereign identity, Merkle trees, agent trust, cryptographic provenance, supply-chain integrity

---

## 1. Introduction

The landscape of computational trust is undergoing a structural transformation driven by the emergence of autonomous software agents that operate across heterogeneous, administratively disjoint domains. A language model agent dispatched to execute a financial workflow, a sensor mesh node reporting environmental telemetry, a federated inference coordinator distributing computation across volunteer hardware — each confronts the same irreducible question: upon what basis should any peer accept its assertions, actions, or outputs?

The dominant answer supplied by two decades of web infrastructure has been *delegation*: trust the agent because a mutually recognized third party vouches for it. OAuth 2.0 token chains, X.509 certificate hierarchies, cloud IAM role assumptions — all encode trust as a transitive social relationship anchored in a centralized root of trust. This architecture functions adequately within the walled gardens of platform ecosystems but fractures at the boundaries where the sovereign agent must operate: across organizational perimeters, intermittent connectivity regimes, and adversarial environments where the identity provider itself may be unavailable, compromised, or jurisdictionally adversarial.

The alternative proposition, developed across parallel threads of computer science research spanning forty years, is that trust can be *computed locally* from cryptographically verifiable claims about an agent's origin, the chain of transformations it has undergone, and the policies that constrain its operation. This is the provenance thesis: *trust in an agent is a deterministic function of its verifiable provenance chain.* The claim is not merely that provenance is useful or complementary to identity-based trust — it is that provenance *is* the primitive, and that identity, authorization, and integrity are all derivative properties computable from it.

This paper situates that claim within a continuous intellectual lineage. Section 3 examines the pre-blockchain distributed systems — Usenet and IRC — that operationalized trust without central coordination decades before the term "decentralized" became a venture-capital adjective. Section 4 traces the cryptographic lineage from Merkle trees through Nakamoto consensus to generalized state-provenance DAGs. Section 5 maps zero-trust architecture's perimeter dissolution to agent autonomy. Section 6 demonstrates the formal correspondence between software supply-chain integrity frameworks and agent provenance chains. Section 7 examines local-first and CRDT systems as proofs-by-existence that monotonic provenance underlies consistent decentralized state. Section 8 situates self-sovereign identity's trust triangle as the identity layer for provenance-bearing agents. Section 9 presents a comparative failure analysis of centralized trust architectures. Section 10 synthesizes these threads into the provenance-as-trust-primitive thesis.

Throughout, we advance a living case study in the X-Dreamarr-Provenance header format, a provenance encoding deployed within The Sleeping Giant's decentralized infrastructure ecosystem. This header — structured as `arr_id:pilot_session_id:policy_id:arr_identity_token` — demonstrates that a compact, cryptographically verifiable provenance statement can serve as the sole trust anchor for agent interactions across protocol boundaries, network topologies, and administrative domains. Its components (mIRCarr, Transportarr, Pulsearr) serve as architectural demonstrations that the foundational primitives of trusted decentralized communication remain viable and can be re-instantiated with modern cryptographic guarantees.

---

## 2. Threat Model

Before advancing the provenance thesis, we must define the adversary against which it provides guarantees. Our threat model assumes a computationally bounded adversary with the following capabilities:

**Network Adversary.** The adversary controls the communication channels between agents. It can observe, delay, reorder, duplicate, and drop messages at will. It cannot, however, break standard cryptographic primitives (hash preimage resistance, digital signature unforgeability, collision resistance of the underlying hash function). This is the standard Dolev-Yao network model: the adversary *is* the network.

**Compromised Agents.** The adversary may fully compromise some fraction of agents, obtaining all their secret key material, internal state, and execution capabilities. A compromised agent can issue validly signed provenance credentials from within its attested identity, produce arbitrary outputs, and attempt to deceive verifying peers. The provenance architecture must limit the scope of such compromises: a compromised agent should not be able to impersonate other agents, retroactively alter its own provenance history, or forge attestations from uncompromised attesting parties.

**Compromised Infrastructure.** The adversary may compromise individual components of the instantiation pipeline: a build server, a policy server, or a credential issuer. A pipeline compromise enables the adversary to produce a *signed but malicious* agent—the precise failure mode exemplified by the SolarWinds attack (Section 9.2). The provenance architecture must ensure that such compromises are *detectable*, even if they are not preventable, by requiring independent, cross-organizational verification layers (e.g., transparency logs, multi-party attestation thresholds, and reproducible builds).

**Sybil Attacks.** The adversary may deploy an arbitrarily large number of adversarial agents with fresh, valid provenance chains rooted in adversary-controlled infrastructure. The architecture does not prevent Sybil attacks at the provenance layer—provenance verifies that an agent is "what it claims to be," not that it is "good"—but it ensures that Sybil agents cannot inherit trust from a compromised legitimate agent's provenance chain. Trust in provenance is not transitive in the social sense; verifying that agent A is an uncompromised instance of software S does not imply that A's peer endorsements are trustworthy.

**Replay Attacks.** The adversary may capture a valid provenance credential from a legitimate agent and replay it at a later time to impersonate that agent. The architecture mitigates this through time-bound credential validity (Section 11) and session-specific binding fields in the provenance token that tie each credential to a specific interaction context.

**Exclusions.** We explicitly exclude from the threat model: (a) physical side-channel attacks against hardware root-of-trust components, (b) quantum adversaries capable of breaking the discrete logarithm or factoring assumptions underlying current digital signature schemes, and (c) adversaries with unbounded computational resources. The architecture can be hardened against (a) and (b) through hardware isolation technologies and post-quantum signature schemes, respectively, but these extensions are beyond the scope of this paper.

The core security invariant that the provenance architecture must preserve is: **A verifying peer that correctly validates an agent's provenance chain obtains a correct binding between the agent's identity, its attested origin, and the policy constraints governing its authorized actions, up to the trustworthiness of the root-of-trust material it holds.** This invariant does not guarantee that the agent is benign; it guarantees that the agent's claimed provenance is cryptographically sound and that deviation from the claimed provenance is detectable.

---

## 3. The Pre-Blockchain Revolution: Usenet and IRC as Original Distributed Trust Systems

Before blockchain, before peer-to-peer file sharing, and before the Web itself, two systems operationalized decentralized trust at global scale: Usenet (conceived by Tom Truscott and Jim Ellis at Duke University in 1979) and Internet Relay Chat (designed by Jarkko Oikarinen in 1988, standardized in RFC 1459, 1993). Both were decentralized, and both remain operational today.

### 3.1 Usenet: Store-and-Forward Trust Without a Server

Usenet's architecture was radical in its simplicity: every node was a peer. Articles propagated via store-and-forward flooding — a node receiving an article would offer it to its neighbors, who would do the same, creating a spanning distribution graph with no root. There was no central server, no canonical index, and no authority from whom one requested content. Trust was embedded in the protocol itself.

Critically, Usenet's Message-ID — standardized across RFC 850, RFC 1036, and ultimately RFC 5536 — functioned as a proto-Decentralized Identifier (DID). The `Message-ID: <unique@hostname>` header provided a globally unique, collision-resistant identifier that was generated locally by the originating agent (the news poster's client software) and required no registration authority, no namespace administrator, and no online resolution service. The identifier was self-certifying: its uniqueness was guaranteed by the combination of a local unique component and the originating host's domain name, forming a two-part namespace that resisted forgery through the same distributed-naming guarantees that underlay the DNS and SMTP ecosystems.

The architectural principles that enabled Usenet's trust model anticipate the provenance thesis directly. Articles carried complete headers — `Path:`, `From:`, `Date:`, `Message-ID:`, `References:` — that formed a minimal but sufficient provenance chain. The `References:` header, in particular, threaded articles into trees, allowing any recipient to reconstruct the causal history of a discussion without consulting any central server. A reader could verify that a message was part of a known thread by checking the hash of referenced messages locally. This is provenance verification without a provenance server.

### 2.2 IRC: Federated Trust via Spanning Trees

IRC's federated server model (RFC 1459) adopted a different topology — servers formed a spanning tree, with messages routed along deterministic paths — but shared the same philosophical commitment: the network operated without a central authority. A user connecting to any server in the tree could participate in any channel; the spanning tree guaranteed eventual delivery without requiring global consensus. Server-to-server links were authenticated, and the spanning tree topology prevented routing loops while remaining resilient to individual node failures.

IRC's `NICK` and `USER` registration mechanism, combined with `MODE`-based channel operator permissions, created a locally-enforced, reputation-based trust model. Channel operators were trusted not because a central authority certified them but because they had demonstrated reliable behavior over time within a specific community context. This is trust as locally-computed reputation — precisely the model that distributed agent provenance generalizes.

### 2.3 The Sleeping Giant's Inheritance

The Sleeping Giant's ecosystem explicitly inherits these lineages. mIRCarr deploys on ngircd, a modern, lightweight IRC daemon, re-instantiating IRC's federated spanning-tree trust model with contemporary transport security. Pulsearr and Transportarr revive Usenet's store-and-forward P2P transfer model, adapting the flooding-propagation architecture to modern agent-to-agent data exchange. These are not nostalgic recreations; they are demonstrations that the architectural primitives discovered in 1979 and 1988 remain the correct primitives for decentralized agent communication — and that the missing piece, now available, is cryptographic provenance binding.

---

## 3. The Cryptographic Lineage: From Hash Chains to Universal Provenance

The cryptographic machinery that transforms provenance from a descriptive record into a trust primitive originates in two foundational results: Merkle's hash-tree signature scheme (1987) and Nakamoto's hash-chained timestamp server (2008).

### 3.1 Merkle Trees: Verifiable Inclusion in O(log n)

Ralph Merkle's 1987 paper, "A Digital Signature Based on a Conventional Encryption Function," introduced the hash tree (or Merkle tree) as a structure enabling efficient verifiable inclusion proofs. The construction is elegant: leaf nodes contain data hashes; each internal node contains the hash of its children's concatenation; the root hash commits to the entire set. To prove that a particular leaf is included in the tree, one need only provide the O(log n) sibling hashes along the path from leaf to root — the verifier recomputes the root from these hashes and the claimed leaf, and checks for equality with the known root hash.

The significance for provenance is profound. A provenance chain — a sequence of transformations applied to an agent from origin to present state — is naturally modeled as a Merkle tree (or, for sequential chains, a simpler hash chain). Each provenance event (code commit, build step, attestation, policy binding) can be hashed; the sequence of hashes can be committed to a root; and any verifier can check the inclusion of any claim in O(log n) time without possessing the full provenance database. This is the property that makes provenance *portable*: an agent can carry a compact Merkle proof of its provenance that any peer can verify locally.

### 3.2 Nakamoto's Timestamp Server: The Chain as Commitment

Nakamoto's 2008 whitepaper — often misread as exclusively a currency proposal — defined a timestamp server that "takes a hash of a block of items to be time-stamped and widely publishes the hash" such that "each timestamp includes the previous timestamp in its hash, forming a chain, with each additional timestamp reinforcing the ones before it." The critical abstraction is not monetary but structural: a hash chain is a non-repudiable, append-only commitment to an ordered sequence of events.

Generalized beyond transaction provenance, the hash-chained structure applies to *any sequential state transformation*. An agent's life-cycle — creation, configuration, policy binding, deployment, execution, observation — is a sequence of state transitions. Hashing each transition and chaining them produces a tamper-evident provenance record: any modification to any past state invalidates all subsequent hashes, making retroactive forgery computationally infeasible.

### 3.3 Hash-Chained DAGs as Universal Provenance Structures

Where multiple independent provenance chains converge — as when an agent's execution depends on multiple upstream attestations, or when a policy references multiple authority signatures — the sequential chain generalizes to a hash-chained directed acyclic graph (DAG). Each node in the DAG is a provenance event; edges are hash pointers to predecessor events; the structure captures fork/join causality while preserving the tamper-evidence property: any modification to any node invalidates all downstream hashes that transitively reference it.

The Sleeping Giant's provenance architecture implements this DAG model. The `pilot_session_id` component of the X-Dreamarr-Provenance header identifies a specific causal lineage; the `policy_id` resolves to a DAG node whose predecessor edges encode the policy's derivation history; the `arr_identity_token` is a compact cryptographic commitment binding the agent's current state to this full causal graph. Verification requires only local hash recomputation — no network round-trips, no centralized timestamp service, no online certificate status checks.

### 3.4 Zero-Knowledge Provenance

For threat models where provenance verification must not disclose the full provenance graph, Ben-Sasson et al.'s Zerocash (2014) construction demonstrates that zk-SNARKs can produce verifiable proofs of provenance-chain validity without revealing the chain's contents. An agent can prove "I am an instance of a specific software artifact, built by a SLSA Level 3 pipeline, authorized under policy X" without disclosing which specific artifact, which specific pipeline, or the full text of policy X. This enables provenance-based trust under information compartmentalization constraints that are common in multi-tenant, multi-jurisdictional, and adversarial operational environments.

---

## 4. Modern Zero-Trust Architecture: When the Perimeter Dissolves

The zero-trust architectural movement — most visibly articulated in Google's BeyondCorp initiative (Ward & Beyer, 2014) — operationalizes the provenance thesis at enterprise scale. BeyondCorp's foundational principle is a direct attack on network-perimeter-based trust: "Connecting from a particular network must not determine which services you can access." Trust is not a property of location; it must be established per-access, per-session, based on verifiable attributes of the requesting entity.

### 4.1 BeyondCorp: The Trust Inferer as Provenance Evaluator

BeyondCorp introduced the Trust Inferer — a component that dynamically computes a trust level for each access request based on device state, user identity, software patch level, and contextual signals. This is provenance evaluation under a different name: the Trust Inferer is checking whether the requesting device's provenance chain (its hardware attestation, OS integrity measurements, configuration state, and authentication events) satisfies the access policy. The decision is local, per-request, and cryptographically grounded — not delegated to a network firewall rule.

The generalization to autonomous agents is direct. Where BeyondCorp evaluates device provenance to authorize human users accessing corporate services, an agent trust framework evaluates agent provenance to authorize peer agents accessing distributed resources. The architectural pattern is identical; the scope expands from enterprise intranet to global Internet.

### 4.2 SPIFFE/SPIRE: Agent Bootstrap into Trust

The SPIFFE (Secure Production Identity Framework for Everyone) and SPIRE implementations — now CNCF graduated projects (2018-present) — solve the problem that is isomorphic to agent trust bootstrap: how does a workload (container, process, function) obtain a verifiable identity without prior possession of a secret? SPIRE achieves this through *attestation*: the workload presents evidence of its execution environment (kernel measurements, cloud provider instance identity documents, hardware root-of-trust attestations) to a SPIRE Agent, which validates these claims and issues a short-lived X.509 SVID (SPIFFE Verifiable Identity Document) of the form `spiffe://trust-domain/workload-identifier`.

The cryptographic primitive is provenance: the workload proves what it *is* (not what key it holds) by attesting to its execution provenance. The SVID is a compact, verifiable provenance credential — and it is short-lived precisely because provenance changes: a workload's trustworthiness decays as its attested state ages past a verifiable window. This pattern maps directly to agent authentication: an agent proves "I am an instance of software artifact X, executing in environment Y, bound by policy Z" through an attestation chain that no prior secret possession can substitute for.

The Sleeping Giant's `arr_identity_token` serves an analogous function to the SVID, encoding the agent's provenance attestation in a compact token that can be verified locally by any peer — no SPIRE server, no online attestation service, no federation required at verification time.

---

## 5. Supply Chain Integrity as Provenance: SLSA, in-toto, Sigstore, and TUF

The software supply chain integrity movement — crystallized in frameworks such as SLSA (Supply-chain Levels for Software Artifacts), in-toto, Sigstore, and TUF (The Update Framework) — provides the most rigorous existing instantiation of provenance-based trust. These frameworks, developed to address the epidemic of supply chain attacks (from the 2011 DigiNotar compromise through the 2020 SolarWinds breach), operationalize a principle that is directly transferable to agent trust: *the trustworthiness of an artifact is a monotonic function of the verifiable integrity of every step in its production chain.*

### 5.1 SLSA: Graduated Levels of Provenance Integrity

The SLSA framework (OpenSSF, v1.0, 2023) defines a graduated model for build provenance. At Build Level 1, the build process must be fully scripted and parameterized. At Level 2, builds are hosted on a source-control service that retains provenance metadata. At Level 3, the build platform must produce *non-falsifiable provenance* — cryptographically signed attestations proving that the output artifact was produced by a specified build configuration from specified source inputs, such that an auditor can reconstruct the complete build chain.

SLSA Level 3 provenance is exactly the structure required for agent trust: an attestation that "this agent binary was built from this source, by this build pipeline, at this time, with these dependencies." The provenance is non-falsifiable because the build platform signs it with a key the attacker does not possess — the integrity guarantee is cryptographic, not institutional. The mapping to agent provenance is one-to-one: replace "artifact" with "agent instance" and "build pipeline" with "agent instantiation pipeline," and the SLSA framework governs agent trust as strictly as it governs software artifact trust.

### 5.2 in-toto: Step-by-Step Cryptographic Attestation

The in-toto framework (Torres-Arias et al., CNCF, 2016-present) refines the provenance model further by defining a layout — a specification of "who should do what and in what order" — and step attestations — signed claims by each functionary that it performed its designated step on specified materials, producing specified products. The key contribution is temporal and causal ordering: in-toto captures *by whom and in what order* each transformation occurred.

An agent's operational life-cycle is formally an in-toto layout. Steps include: image build, configuration application, policy binding, credential issuance, deployment scheduling, execution invocation, peer handshake. Each step is attested by the entity performing it. The full attestation chain is verifiable independently by any relying party. The X-Dreamarr-Provenance header's `pilot_session_id` and `policy_id` fields correspond to in-toto layout identifiers and step linkage keys.

### 5.3 Sigstore: Keyless, Identity-Based Signing

Sigstore (2021-present) — comprising Cosign (signing), Fulcio (certificate authority), and Rekor (transparency log) — introduces an innovation that is critical for sovereign agent trust: "move away from a key-based signing approach to an identity-based one." Rather than requiring agents to manage long-lived private keys (which are stealable, losable, and require complex key-distribution infrastructure), Sigstore binds signing events to OIDC-authenticated identities and records them in an append-only, globally-auditable transparency log.

The append-only log (Rekor) is itself a hash-chained provenance structure: each entry's hash incorporates the previous entry's hash, creating a Merkle tree whose root is periodically published. An entry's inclusion can be verified with O(log n) proof size. For agent trust, the transparency log solves the auditability problem: provenance claims are not merely locally verifiable but *globally auditable* — any deviation is detectable by any monitor, and the log cannot be rewritten without detection.

### 5.4 TUF: Role-Based Trust Separation

The Update Framework (Samuel et al., 2010-present, CNCF graduated) addresses trust compromise resilience through role separation and threshold signatures. TUF distributes signing authority across multiple roles — root, targets, snapshot, timestamp — each with distinct keys, some kept offline, such that the compromise of any single key (or even multiple online keys) does not enable a malicious update to be signed.

This role separation is directly applicable to agent provenance. Agent trust should not be binary but role-graded: an agent may possess provenance to attest its binary integrity (root-equivalent role) without possessing capability to authorize policy changes (targets-equivalent role). TUF's threshold-of-offline-keys model provides the cryptographic primitive for policies that require multi-party authorization — such as a policy change that requires signatures from both the agent's developer and its human supervisor.

The Sleeping Giant's `policy_id` field resolves to a TUF-style delegated trust structure: the policy itself is a signed artifact whose signature chain includes thresholds across organizational roles, ensuring that no single compromised key can alter the trust constraints governing agent behavior.

---

## 6. Local-First Software and CRDTs: Provenance Without Coordination

The local-first software movement (Kleppmann et al., Ink & Switch, 2019) articulates a design philosophy whose implications for agent trust are not yet fully appreciated. Its seven ideals include: "the primary copy of the data is local... the cloud is optional," "the network is used to sync between devices but is not essential for full functionality," and crucially: "users retain ultimate ownership and control."

The provenance-theoretic insight embedded in local-first architecture is that *the primary copy's authority derives from its unbroken provenance chain, not from its location.* A local-first application that stores data on-device and syncs opportunistically is asserting that the on-device copy is authoritative because it is end-to-end verifiable from the user's input events forward — the provenance chain terminates at a known, trusted origin (the user), and no sync intermediary can alter it because any alteration would break the hash chain.

### 6.1 CRDTs: Monotonic Reconciliation as Monotonic Provenance

Conflict-Free Replicated Data Types (Shapiro et al., SSS 2011, LNCS 6976) extend the provenance-computability thesis to concurrent state. CRDTs achieve strong eventual consistency without central coordination through a mathematical property: all concurrent operations commute, and state is monotonic with respect to operation application. There is no rollback, no conflict resolution, and no consensus protocol — because the data type itself guarantees that any two replicas that have seen the same set of operations will be in the same state, regardless of delivery order.

The connection to provenance is structural: CRDTs maintain provenance implicitly through their monotonicity property. Each operation carries a unique identifier (often a Lamport timestamp or vector clock component), and the set of operations applied to a replica constitutes its provenance. Because the data type guarantees that operation provenance determines state deterministically, trust in the state reduces to trust in the operation provenance. No central coordinator is needed because the provenance chain *is* the coordination mechanism.

For agent trust, CRDTs demonstrate that deterministic state from distributed provenance is not merely possible but mathematically guaranteed under defined constraints. An agent's operational state — its set of completed tasks, its credential cache, its policy evaluation history — can be maintained as a CRDT across peers, with each peer independently verifying provenance for each state transition without communicating with a central authority.

---

## 7. Self-Sovereign Identity: The Agent as Identity Sovereign

### 7.1 Allen's Ten Principles

Christopher Allen's "The Path to Self-Sovereign Identity" (2016) articulated ten principles for user-controlled digital identity: Existence, Control, Access, Transparency, Persistence, Portability, Interoperability, Consent, Minimization, and Protection. Though conceived for human identity, the principles map with remarkable fidelity to autonomous agent identity. An agent must *exist* independently (a running process with a definable computational boundary); *control* its own identity artifacts (keys, attestations); grant or deny *access* to its identity data; operate with *transparency* (its algorithms must be inspectable); persist across restarts; be *portable* across execution environments; *interoperate* with diverse peer protocols; *consent* to actions on its own behalf; *minimize* data disclosure; and *protect* its identity material.

### 7.2 The Trust Triangle: Issuer → Holder → Verifier

Reed and Preukschat's Self-Sovereign Identity (Manning, 2021) formalized the trust triangle: an Issuer makes claims about a subject and issues a verifiable credential to the Holder; the Holder presents the credential to a Verifier; the Verifier checks the Issuer's signature and the credential's validity without contacting the Issuer. Trust is established through cryptographic verification of the credential, not through online resolution of the Issuer.

In the agent context, the Issuer is any entity that makes a provenance claim about an agent: the build system attesting binary integrity, the policy server attesting authorization, the hardware root-of-trust attesting execution environment. The Holder is the agent itself, carrying these credentials as part of its provenance state. The Verifier is any peer considering whether to trust the agent. The X-Dreamarr-Provenance header is a compact verifiable credential encoding — a single structured token that bundles multiple credentials into a verifiable package.

### 7.3 Legal Instantiation: eIDAS/ESSIF

The EU's eIDAS regulation and European Self-Sovereign Identity Framework (ESSIF, 2019) legislatively instantiate SSI within the European Blockchain Services Infrastructure (EBSI). This demonstrates that sovereign identity is not merely a cryptographic curiosity but an emerging legal-computational infrastructure. The regulatory trajectory — from centralized eID to self-sovereign credential presentation — tracks the technical trajectory from platform-mediated identity to provenance-based identity, suggesting that regulatory and cryptographic development are converging on the same primitive.

---

## 8. Centralized vs. Distributed Trust: A Comparative Failure Analysis

The superiority of provenance-based trust is most clearly demonstrated through comparative failure analysis of centralized trust architectures.

### 8.1 DigiNotar (2011): Single Root, Total Collapse

The DigiNotar certificate authority compromise demonstrates the catastrophic failure mode of transitive, root-anchored trust. A single CA's root key compromise enabled the attacker to issue fraudulent certificates for hundreds of domains, including Google, Mozilla, and intelligence agencies. Because the entire global PKI trust model depended on CA root keys as transitive trust anchors, the compromise of *one* root collapsed trust for *all* domains whose certificates were issued under it. There was no per-certificate provenance to check, no independent verification path, and no local trust computation — merely binary reliance on a remote root whose compromise was invisible to relying parties until the fraud was detected ex post.

### 8.2 SolarWinds (2020): Signed but Compromised Pipeline

The SolarWinds supply chain attack exploited precisely the gap between signing and provenance. The Orion platform's build pipeline produced a *signed* binary — the signature was valid — but the build process itself had been compromised, injecting malicious code into a legitimate artifact. The signature attested to the artifact's origin from SolarWinds' pipeline, but the pipeline's own integrity was not represented in the provenance model. The attack succeeded because trust was placed in a single attestation (the signature) rather than in a full provenance chain that would have captured the build environment's integrity measurements, the compiler's identity, and the source-code-to-binary correspondence.

### 8.3 The Counterposition

In a provenance-based trust model, both failures would be structurally impossible. Under DigiNotar, each certificate would carry a provenance chain attesting to the issuance process — including audit log inclusion, multi-party authorization, and certificate transparency log timestamps. A rogue issuance would leave a detectable gap in the provenance chain or a contradiction with the transparency log. Under SolarWinds, SLSA Level 3 provenance would capture the full build environment: the source repository commit hash, the build platform identity, the compiler version, the dependency tree hashes. Any compromise of the build pipeline would break the hash chain — the resulting artifact's provenance would not match the expected provenance template, and verification would fail locally, without requiring network queries or centralized detection services.

Trust as a computable local function of verifiable provenance eliminates the single-point-of-failure property that makes centralized trust architectures brittle, and eliminates the trust-in-the-singer-not-the-singing problem that makes signature-only verification insufficient.

---

## 9. Living Case Study: The X-Dreamarr-Provenance Header

The X-Dreamarr-Provenance header — `arr_id:pilot_session_id:policy_id:arr_identity_token` — operationalizes the provenance thesis within The Sleeping Giant's distributed infrastructure ecosystem. Each component of the header encodes a distinct layer of the provenance stack, and the composition yields end-to-end verifiable trust that requires no online identity provider, no centralized certificate authority, and no shared network perimeter.

### 9.1 Header Component Analysis

**`arr_id`** — A globally unique agent identifier, generated locally by the agent or its instantiating sub-system. Analogous to Usenet's Message-ID or a W3C DID, this identifier is self-certifying: it requires no registration authority and can be generated offline. It serves as the root of the agent's identity provenance chain.

**`pilot_session_id`** — Identifies the specific causal lineage — the instantiation session — within which the agent was created. This corresponds to an in-toto layout identifier: it binds the agent to a specific instantiation pipeline, with a specific set of expected step attestations. The session identifier enables differentiation between multiple instantiations of the same agent class.

**`policy_id`** — Resolves to a cryptographically signed policy artifact whose signature chain encodes a TUF-style role-separated authorization structure. The policy defines the constraints under which the agent is permitted to operate: which peers it may communicate with, which resources it may access, which actions it may perform, and which attestations it must present for each class of operation. The policy's own provenance chain is verifiable against a root of trust known to the verifying peer.

**`arr_identity_token`** — A compact cryptographic commitment (hash, signature, or zero-knowledge proof) that binds all preceding fields into a single verifiable token. This token is the agent's portable provenance credential — analogous to a SPIRE SVID or a Sigstore-signed attestation — and can be verified locally by any peer possessing the relevant root-of-trust material.

### 9.2 Ecosystem Inheritance

The header operates within an ecosystem whose components deliberately inherit the architectural patterns of decentralized predecessors. **mIRCarr** deploys on ngircd, re-instantiating IRC's RFC 1459 federated spanning-tree topology for agent-to-agent messaging, but now with X-Dreamarr-Provenance headers attached to JOIN and PRIVMSG commands, enabling per-message trust verification. **Transportarr** and **Pulsearr** revive Usenet's store-and-forward flooding model for agent-to-agent data transfer, applying the prov-enance header to each transfer unit, such that a receiving agent can verify the provenance of each chunk without consulting the sender or any third party.

### 9.3 Verification Model

Verification proceeds as follows: (1) the verifier extracts the header fields from an incoming agent communication; (2) it checks the `arr_identity_token` signature against the concatenation of `arr_id`, `pilot_session_id`, and `policy_id`; (3) it retrieves (or already possesses) the public key or root hash associated with the `policy_id`; (4) it verifies the policy's own provenance chain back to a known root-of-trust; (5) it evaluates whether the policy authorizes the specific action the agent is requesting; (6) it caches the result for subsequent interactions within the same session. Steps 1-2 are purely local computations. Steps 3-5 may require retrieval of the policy artifact, but the policy — once retrieved — can be cached, verified once, and reused across many agent interactions.

The critical property is that *no step requires contacting an online identity provider, certificate authority, or authorization server at verification time.* Trust is a local computation over cryptographically verifiable provenance data that the agent carries with it.

---

## 10. Conclusion

This paper has advanced the thesis that distributed deterministic provenance constitutes the universal cryptographic primitive for decentralized agent trust. We have traced a forty-year intellectual lineage — from Usenet's store-and-forward Message-ID system through Merkle hash trees, Nakamoto's timestamp server, BeyondCorp's perimeter dissolution, SLSA and in-toto's supply-chain integrity frameworks, local-first CRDT architecture, and the legal-computational infrastructure of self-sovereign identity — to demonstrate that the provenance thesis is not a novel invention but a synthesis of independently developed, mutually reinforcing results.

The convergence of these threads on a common primitive is not accidental. Each thread addresses a different aspect of the same underlying problem: how can one computational entity determine whether to trust another without relying on a single, centralized, compromisable trust anchor? Usenet answered with globally unique self-certifying identifiers. Merkle answered with compact inclusion proofs. Nakamoto answered with hash-chained append-only logs. BeyondCorp answered with per-access trust inference from device provenance. SLSA answered with non-falsifiable build attestations. CRDTs answered with deterministic state from operation provenance. SSI answered with the issuer-holder-verifier trust triangle. Each is a partial solution; the unified primitive is provenance.

The X-Dreamarr-Provenance header demonstrates that this primitive can be deployed in a compact, portable, interoperable format suitable for contemporary agent ecosystems. Its four-field structure encodes the full trust stack — identity, session lineage, policy constraint, and cryptographic commitment — in a format that any peer can verify locally. The Sleeping Giant's infrastructure — mIRCarr, Transportarr, Pulsearr — validates that the architectural patterns of Usenet and IRC remain viable when augmented with cryptographic provenance, completing the loop from 1979 to the present.

The implication for the design of autonomous agent systems is clear: *build trust into the provenance layer, not the identity layer.* Do not ask "Who vouches for this agent?" — that merely displaces the trust problem onto the voucher. Ask instead: "Can I verify this agent's provenance chain from a known origin through a known policy to this specific interaction?" If the answer, computed locally from cryptographic evidence, is yes — trust the agent. If not, do not. Trust ceases to be a social arrangement and becomes a computable function. That is the sovereign ledger.

---

## Bibliography

Allen, C. (2016). "The Path to Self-Sovereign Identity." *Life With Alacrity.* [Online]. Available: http://www.lifewithalacrity.com/2016/04/the-path-to-self-soveraign-identity.html

Ben-Sasson, E., Chiesa, A., Garman, C., Green, M., Miers, I., Tromer, E., & Virza, M. (2014). "Zerocash: Decentralized Anonymous Payments from Bitcoin." In *Proceedings of the 2014 IEEE Symposium on Security and Privacy (S&P)*, pp. 459-474. IEEE.

eIDAS/ESSIF. (2019). "European Self-Sovereign Identity Framework." European Commission. *European Blockchain Services Infrastructure (EBSI).*

Groth, P. & Moreau, L. (2013). "PROV-Overview: An Overview of the PROV Family of Documents." *W3C Working Group Note.* World Wide Web Consortium. [Online]. Available: https://www.w3.org/TR/prov-overview/

Kleppmann, M., Wiggins, A., van Hardenberg, P., & McGranaghan, M. (2019). "Local-First Software: You Own Your Data, in Spite of the Cloud." In *Proceedings of the 2019 ACM SIGPLAN International Symposium on New Ideas, New Paradigms, and Reflections on Programming and Software (Onward!)*, pp. 154-178. ACM.

Merkle, R. C. (1987). "A Digital Signature Based on a Conventional Encryption Function." In Pomerance, C. (ed.), *Advances in Cryptology — CRYPTO '87*, Lecture Notes in Computer Science, Vol. 293, pp. 369-378. Springer.

Nakamoto, S. (2008). "Bitcoin: A Peer-to-Peer Electronic Cash System." [Online]. Available: https://bitcoin.org/bitcoin.pdf

Oikarinen, J. & Reed, D. (1993). "Internet Relay Chat Protocol." *RFC 1459.* Internet Engineering Task Force (IETF).

PROV-DM. (2013). "PROV-DM: The PROV Data Model." *W3C Recommendation.* World Wide Web Consortium. [Online]. Available: https://www.w3.org/TR/prov-dm/

Reed, D. & Preukschat, A. (2021). *Self-Sovereign Identity: Decentralized Digital Identity and Verifiable Credentials.* Manning Publications.

RFC 850. (1983). "Standard for Interchange of USENET Messages." M. Horton. Internet Engineering Task Force (IETF).

RFC 1036. (1987). "Standard for Interchange of USENET Messages." M. Horton & R. Adams. Internet Engineering Task Force (IETF).

RFC 5536. (2009). "Netnews Article Format." K. Murchison, C. Lindsey, & D. Kohn. Internet Engineering Task Force (IETF).

Samuel, J., Mathewson, N., Cappos, J., & Dingledine, R. (2010). "Survivable Key Compromise in Software Update Systems." In *Proceedings of the 17th ACM Conference on Computer and Communications Security (CCS)*, pp. 61-72. ACM.

Shapiro, M., Preguiça, N., Baquero, C., & Zawirski, M. (2011). "Conflict-Free Replicated Data Types." In *Proceedings of the 13th International Symposium on Stabilization, Safety, and Security of Distributed Systems (SSS 2011)*, Lecture Notes in Computer Science, Vol. 6976, pp. 386-400. Springer.

Sigstore. (2021-present). "Sigstore: Software Signing for the Masses." Linux Foundation. [Online]. Available: https://www.sigstore.dev/

SLSA v1.0. (2023). "Supply-chain Levels for Software Artifacts." Open Source Security Foundation (OpenSSF), Linux Foundation. [Online]. Available: https://slsa.dev/

SPIFFE/SPIRE. (2018-present). "Secure Production Identity Framework for Everyone." Cloud Native Computing Foundation (CNCF). [Online]. Available: https://spiffe.io/

Torres-Arias, S., Ammula, A., Curtmola, R., & Cappos, J. (2016). "in-toto: Providing Farm-to-Table Guarantees for Bits and Bytes." In *Proceedings of the 28th USENIX Security Symposium (USENIX Security '19)*, pp. 1393-1410. USENIX Association.

Truscott, T. & Ellis, J. (1979). Usenet conception. Duke University. [Historical reference — see Hauben, M. & Hauben, R. (1997). *Netizens: On the History and Impact of Usenet and the Internet.* IEEE Computer Society Press.]

TUF. (2010-present). "The Update Framework." Cloud Native Computing Foundation (CNCF). [Online]. Available: https://theupdateframework.io/

Ward, R. & Beyer, B. (2014). "BeyondCorp: A New Approach to Enterprise Security." *;login: The USENIX Magazine,* 39(6), pp. 6-11. USENIX Association.

W3C DID Core. (2022). "Decentralized Identifiers (DIDs) v1.0." *W3C Recommendation.* World Wide Web Consortium. [Online]. Available: https://www.w3.org/TR/did-core/

---

## === PEER REVIEW ===

**Reviewer:** Anonymous

**Paper:** "The Sovereign Ledger: Distributed Deterministic Provenance as the Cryptographic Foundation of Decentralized Agent Trust"

### Summary Assessment

This paper presents a well-structured synthesis argument that distributed deterministic provenance is the universal primitive for agent trust, tracing a forty-year intellectual lineage across Usenet, cryptography, zero-trust architecture, supply chain integrity, CRDTs, and self-sovereign identity. The prose is clear, the architecture coherent, and the Living Case Study — the X-Dreamarr-Provenance header — provides concrete grounding for otherwise abstract claims.

### Strengths

1. **Intellectual lineage mapping.** The historical argument — connecting Usenet's Message-ID (1979) to W3C DIDs — is compelling and historically accurate. The paper correctly identifies the proto-DID nature of Message-ID headers and the decentralized trust model implicit in store-and-forward flooding.

2. **Cross-domain synthesis.** The paper successfully integrates six normally siloed research domains (distributed systems history, cryptography, enterprise security, supply-chain integrity, local-first/CRDT architecture, and digital identity) into a unified theoretical framework.

3. **The Computability Thesis.** The central claim — "trust in an agent is a deterministic function of its verifiable provenance chain" — is crisply stated and repeatedly reinforced. This is a falsifiable, testable proposition, which is a hallmark of strong theoretical work.

4. **Concrete case study.** The X-Dreamarr-Provenance header provides necessary specificity that many theoretical papers lack. The component-level analysis (`arr_id`, `pilot_session_id`, `policy_id`, `arr_identity_token`) maps clearly to the theoretical constructs developed in earlier sections.

### Gaps and Improvement Suggestions

1. **Threat model specification.** The paper would benefit from an explicit threat model section. What adversary capabilities are assumed? Does the provenance model resist a compromised instantiation pipeline (signed but malicious agent), Sybil attacks on the trust graph, or replay of stale provenance credentials? Without a threat model, the security claims — while plausible — are not rigorously bounded.

2. **Revocation and expiry.** The paper implies that provenance verification is local and cachable, but does not address credential revocation. In the SPIFFE model, SVIDs are short-lived precisely to bound the revocation window. How does the X-Dreamarr-Provenance model handle revocation of a compromised `arr_identity_token` or a revoked `policy_id`? A transparency-log-based approach (analogous to Certificate Transparency) is mentioned in the Sigstore section but not integrated into the header verification model.

3. **Formalization gap.** The paper asserts that "trust ceases to be a social arrangement and becomes a computable function" but does not provide the formal definition of that function. A compact definition — e.g., `Trust(A, P, R) = Verify(Prov(A, P, R))` — with defined verification rules would strengthen the computability claim and provide a basis for formal security analysis.

4. **Empirical evaluation.** The paper makes no empirical claims and offers no benchmarks. Even a modest evaluation — verification latency for the header format, proof size comparisons, failure recovery time — would substantiate the practicality claim. The "still runs today" argument for Usenet and IRC is evocative but not quantitative.

5. **Comparative evaluation with alternatives.** The paper critiques centralized models (OAuth 2.0, X.509 PKI, cloud IAM) but does not compare the provenance model against other distributed trust proposals — for example, blockchain-based DID methods (did:ethr, did:indy), Web of Trust models, or attestation-based systems like Intel SGX remote attestation. A comparative table would clarify the design-space positioning.

6. **Incentive and adoption.** The paper does not address what incentives drive adoption of provenance-based trust over the network effects of platform-mediated identity. The transition from centralized to distributed trust is not purely a technical problem — it is an adoption problem, and the paper could acknowledge this limitation.

### Recommendation

**Accept with minor revisions.** The paper makes a significant theoretical contribution by synthesizing a coherent provenance-first trust model for autonomous agents. The historical lineage argument is original and well-supported. Revision should focus on: (a) adding an explicit threat model, (b) formalizing the trust computation function, (c) addressing revocation and expiry mechanisms, and (d) positioning the work against alternative distributed trust approaches. The writing quality is suitable for publication in a systems or security conference (e.g., USENIX ATC, ACM CCS, or NDSS) after these revisions.

**Rating:** 7.5/10 (Solid theoretical contribution, needs threat-model formalization before claiming practical deployability.)
