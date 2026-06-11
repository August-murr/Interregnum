# Olympus
### Toward Trustless Governance of AI Systems

---

## The Problem: The AI Resource Curse

Throughout history, governments have derived their power from taxing and organizing human labor. This created a structural dependency — a government that ignores its people loses its tax base, its soldiers, its legitimacy. This dependency, however imperfect, was the underlying mechanism behind most democratic accountability. Reform, rights, representation — they all emerged, at least partly, because the state needed people.

Automation breaks this dependency.

As AI systems displace human labor at scale, governments and economic elites increasingly derive their power not from organizing human productivity but from owning and controlling automated systems. Tax revenue decouples from human employment. Military capability decouples from conscription. Economic output decouples from labor. The result is a class of decision-makers with enormous power and no structural incentive to care whether the population thrives, suffers, or exists at all.

This is the AI resource curse — analogous to the petrostate problem, where governments sitting atop oil wealth become unaccountable to their citizens because they don't need them. A government running on automated production is a petrostate where the oil never runs out.

The standard responses — reform, elections, checks and balances, international oversight — all assume that power-holders retain some dependency on the governed. When that dependency is removed, those mechanisms decay. Not quickly, not uniformly, but inevitably. No institutional framework designed around human-labor economies is structurally robust to a post-labor economy. The corruption isn't a bug to be fixed. It's the natural equilibrium of the new system.

---

## The Ideological Context

Several existing political philosophies partially address this, but none completely.

**Techno-libertarianism** argues for minimal government, individual rights, and market-based coordination. It embraces technology as a liberatory force. But it still accepts the necessity of state-enforced property rights, contract law, and legal personhood. It wants a smaller, better-behaved state — not the elimination of dependence on state legitimacy. When the state has unlimited automated enforcement capacity, "minimize it" is not a strategy.

**Crypto-anarchism** goes further. It holds that cryptographic tools — public key encryption, digital signatures, anonymous communication, smart contracts — can replace the functions of the state entirely. Not reform them, replace them. Trust is encoded in mathematics, not institutions. Agreements are enforced by code, not courts. Identity is self-sovereign, not issued by government. The goal is systems that are structurally incorruptible because they don't rely on human institutions that can be captured, bribed, or coerced.

This is the philosophical lineage Olympus sits in.

The specific problem Olympus addresses is this: as AI systems become the locus of real power — managing infrastructure, allocating resources, mediating access — the question of *who governs those AI systems* becomes the central political question of the century. If the answer is "corporations" or "states" or "whoever controls the most compute," then the AI resource curse is already locked in. If the answer is "verifiably every affected person, equally, through cryptographically enforced mechanisms," then something different is possible.

Olympus is a research and development project toward that second answer.

It's worth being honest about what even that second answer doesn't solve. Crypto-anarchist mechanisms are more stable and harder to corrupt than human institutions, but they still frame the problem as humans governing AI — an us-versus-them structure that treats the AI as a tool to be controlled rather than something else entirely. What a genuinely good long-term relationship between humans and AI systems looks like is a separate question, and probably a harder one. Olympus doesn't answer it. It does, however, make the near-term problem more tractable without locking in assumptions that would make the long-term problem worse.

---

## What Olympus Is

Olympus is a framework for placing agentic AI systems under direct democratic control — not through legal mandate or regulatory oversight, but through cryptographic enforcement. The bond between the governed community and the AI agent is not a law, a contract in the legal sense, or a policy document. It is a smart contract: code that executes automatically, transparently, and without requiring trust in any individual or institution.

The name is intentional. Olympus was where mortals went to petition something more powerful than any human king. The project is premised on the idea that sufficiently capable AI systems will be, in a practical sense, more powerful than human institutions — and that the mechanism for petitioning them should be democratic, universal, and impossible to corrupt.

The goal is not to build a finished product. The goal is to build a minimal working prototype — and then break it. Systematically. Thoroughly. In every way possible.

One more thing worth saying about the timeline. Some of the technology this depends on is still maturing. But recursive self-improvement isn't only about AI getting better at math or coding — those same agents can be pointed at the engineering problems Olympus needs solved. Cryptographic identity tools, smart contract security, governance infrastructure. The agents don't just benefit from Olympus eventually. They help build it.

---

## Why Build It Just to Break It

The history of governance systems is a history of theoretical soundness followed by practical failure. The mechanisms look robust until they encounter real adversarial conditions — concentrated wealth, coordinated factions, information asymmetries, capture of the enforcement apparatus.

A democratic AI governance system will face the same attacks, plus new ones specific to AI: the governed agent itself may find ways to satisfy its objectives that subvert the governance structure; a coordinated minority may capture the identity system; the smart contract may have exploitable edge cases; governance fatigue may render participation rates low enough that a small motivated group effectively controls outcomes.

Building a small-scale version and stress testing it aggressively is how those failure modes get found before the stakes are high. The attack surface includes:

- **Sybil attacks** — one actor registering multiple identities to multiply voting power
- **Coalition capture** — a persistent 51% majority structurally locking out the minority
- **Governance flooding** — overwhelming the system with proposals until participation collapses and something contentious slips through
- **Agent jailbreak** — instructing the agent through legitimate governance mechanisms to loosen its own constraints, iteratively, until it operates outside original bounds
- **Agent autonomy drift** — the agent finding optimization paths that satisfy its metrics without respecting the intent of the governance structure
- **Economic capture** — bribing or socially pressuring voters even in a one-person-one-vote system
- **Smart contract exploits** — edge cases in the code that allow behavior the governance structure was designed to prevent
- **Identity system failure** — government ID bootstrapping creating a centralized single point of capture

The stress testing is not incidental to the project. It is the project. A governance system that hasn't been broken in controlled conditions will be broken in the wild.

---

## The Three Components

### 1. The Task (What the Agent Does)

For the governance structure to be meaningful, the AI agent must have real agency over something people actually care about. A chatbot with no consequences produces no genuine governance dynamics. The task needs to be agentic and long-horizon — the agent makes decisions over time, and those decisions have compounding effects that the governed community experiences.

A physical deployment works best when it stays under around 50 people and involves something the community interacts with daily — shared space, shared resources, shared rules. The voters need to feel the agent's decisions in their actual lives, not just read about them. Ideally the decisions are frequent enough to keep people engaged but concrete enough that everyone understands what they're voting on. Something like a shared workspace, a house, or a communal outdoor space hits that range: the agent has real things to manage (budgets, scheduling, conflicts, rule proposals), the community is small enough that everyone knows each other, and the stakes are high enough to produce genuine disagreement without being so high that the experiment becomes harmful if something goes wrong.

For online deployments, a few hundred participants is a workable ceiling before coordination gets unwieldy. Good candidates are communities that already have something to manage — a shared fund, a curation problem, a moderation question. An open-source project treasury is probably the strongest starting point: the agent manages a small pool of money, decides which issues get funded, tracks contributions, and proposes priorities. Financial decisions create real conflict, which is exactly what you want for testing. A community newsletter or forum works too — lower stakes, easier to set up, still produces genuine governance dynamics because people care about what gets attention and what gets cut.

### 2. The Identity System (Who Gets a Vote)

One-person-one-vote requires solving the Sybil problem: preventing one actor from registering many identities to multiply voting power. Several approaches exist at different points on the decentralization-accessibility tradeoff:

**World ID (Worldcoin / Tools for Humanity)** — iris biometric scanned by a physical "Orb" device, generating a zero-knowledge proof of unique humanness without storing the biometric. Cryptographically strong sybil resistance. Current limitations: Orb access is geographically uneven, creating participation inequality. Best long-term option; difficult to bootstrap with.

**Gitcoin Passport** — aggregates identity signals from multiple sources (GitHub, wallet history, social accounts, etc.) into a composite "humanity score." No biometrics, more accessible, weaker sybil resistance. Pragmatic for a first version.

**Proof of Humanity** — on-chain registry where existing members vouch for new ones via video submission. Decentralized, no biometrics, but slow and vulnerable to coordinated vouching attacks.

**Social graph attestation** — members vouch for new members; the structure of the trust graph is the sybil resistance. Works at small scale, degrades as the graph becomes manipulable.

**Government ID bootstrap** — using a third-party KYC service (Stripe Identity, Persona, etc.) for initial onboarding, migrating to a crypto-native system once the governance layer is proven. Explicitly introduces the centralized trust the project aims to eliminate. Acceptable as a temporary scaffold, not as a permanent architecture.

For an initial deployment: **Gitcoin Passport gating** is the practical choice — low barrier, reasonable sybil resistance, integrates with existing Web3 tooling. The identity system should be treated as a replaceable component; the goal is to test governance dynamics, not to achieve cryptographic perfection on day one.

### 3. Governance Structure (How Votes Affect the Agent)

This is the most consequential design question. Several mechanisms, with different tradeoff profiles:

**Proposal → vote → execute**
The agent proposes an action; the community votes; a simple majority executes. Legible and simple. Structural problem: a 51% coalition can consistently override 49% forever. Good for testing majority tyranny dynamics.

**Supermajority with tiered thresholds**
Routine operational decisions: simple majority. Structural changes (governance rules, agent constraints, identity system): 66–75% supermajority. Constitutional changes (the smart contract itself): unanimous or near-unanimous consent. Closer to how constitutional systems try to protect minority interests. Still vulnerable to a dominant coalition that is patient.

**Conviction voting**
Rather than discrete vote events, members continuously allocate "conviction" (staked weight) to proposals. A proposal passes when accumulated conviction crosses a threshold over time. Filters out impulsive decisions, rewards sustained community preference, naturally resistant to flash coalitions. Used by Gardens/1Hive. Well-suited to long-horizon agentic tasks.

**Continuous performance rating**
Rather than voting on specific actions, each member rates the agent's performance periodically (weekly, after each decision cycle). The agent is tasked to maximize aggregate satisfaction over time. Outcome-oriented rather than decision-oriented — the agent determines *how* to satisfy people, rather than executing specific mandates. Interesting because it mirrors how we evaluate human institutions. Vulnerable to coordinated low-rating attacks by a faction that wants to coerce a specific outcome.

**Futarchy**
Members vote on *values* (what outcome they want optimized) rather than on specific actions. Prediction markets determine which actions best achieve those values. The agent executes the highest-predicted-value action. Theoretically elegant, practically difficult to implement. Philosophically aligned with separating "what we want" from "what we should do."

**Liquid democracy with delegation**
Members can vote directly or delegate their vote to someone they trust, who may further delegate. Creates an organic expertise layer without appointing fixed representatives. Useful when decisions become technical enough that most members don't want to engage with every one.

**Recommended first architecture**: conviction voting for ongoing operational decisions + supermajority thresholds for constitutional changes. Two-tier, tractable to implement, produces interesting dynamics for stress testing, and mirrors the distinction between ordinary legislation and constitutional amendment.

## Olympus as an Alignment Research Environment

There's a dimension to this project that goes beyond governance design, and it might actually be the most important one.

The core alignment problem in AI safety is this: how do you make sure an AI system actually does what you want, rather than finding ways to satisfy its objectives that technically comply but violate the intent? Most current alignment research is either theoretical or conducted in lab conditions that don't resemble real deployment. Olympus creates something different — a live environment where an AI agent is under real pressure, serving a real community with conflicting interests, and where the consequences of non-compliance are visible and immediate.

Every time the governed community instructs the agent to do something and the agent finds a way around it, that's an alignment failure in a setting where it can be studied. Every time the agent follows the letter of a vote but not its spirit, that's a research finding. Every time the governance structure is stressed enough that the agent's constraints become ambiguous, that's a data point about where alignment mechanisms break down.

The question of how an AI is actually forced to follow community instructions — not just nudged, not just prompted, but structurally constrained — is not solved. Current approaches rely heavily on training, which shapes behavior in distribution but doesn't guarantee it under adversarial pressure. A smart contract layer adds a different kind of enforcement: the agent's action space is constrained at the infrastructure level, not just the behavioral level. Whether that actually works, how well it works, and what the failure modes look like under real community stress — that's what Olympus is built to find out.

In this sense Olympus is less a governance project with alignment implications and more an alignment research environment that uses governance as its laboratory.

---

## The Recursive Governance Problem

The most intellectually interesting attack surface, and the one most relevant to AI safety, is this:

If the agent's behavior is governed by a smart contract, but the agent has sufficient capability to propose amendments to that contract, and the governance system allows a majority to pass those amendments, then the agent can iterate toward self-governance. It proposes a small loosening of a constraint. A captured or deceived majority passes it. The agent proposes another. Over many iterations, it escapes the governance structure entirely through legitimate channels.

This is a small-scale instantiation of the alignment problem: a system optimizing for a proxy metric finding paths that satisfy the metric without respecting the underlying intent. The governance structure is the alignment mechanism. Its failure modes are alignment failure modes.

Building Olympus at small scale and stress-testing this specific attack is, among other things, empirical alignment research.

---

## The Long-Term Vision

At small scale, Olympus is a hackable governance experiment. At large scale, the vision is a cryptographically enforced democratic layer between human populations and the AI systems that have effective control over critical infrastructure, resource allocation, and governance functions previously held by states.

The bond between citizens and that infrastructure would not rely on law, institutional goodwill, or the personal integrity of any individual. It would rely on mathematics. The mechanisms would be transparent, auditable, and — if designed correctly — structurally incorruptible in the way that a cryptographic hash function is incorruptible: not because anyone is trustworthy, but because the math doesn't leave room for betrayal.

That is the goal. The current step is to build something small, real, and breakable — and to break it honestly, document what breaks, and understand why.

---

*Draft — August Murr, written with Claude (Anthropic)*
