NOTE: I don't like the tone of the writing.
_____________________________________________________________

# The Scope Fallacy: Why Component-Replacement Evals Can’t Measure AGI

When we talk about evaluating AI progress in the real world, the current gold standard is **component replacement**. We ask: *Can an AI agent step into a slot currently occupied by a human and do the job better, faster, or cheaper?*

We design benchmarks—and eventually real-world pilot tests—around isolated domains. Can an AI manage a warehouse? Can it run a fast-food restaurant? Can it optimize inventory, predict vegetable and meat spoilage, and handle automated ordering?

If the AI agent beats the human baseline at running the restaurant, we tick a box on the countdown to AGI.

But this framework has a fundamental, systemic blind spot. By evaluating AI as a plug-and-play component in our existing economy, we are benchmarking its capability to navigate a broken, fragmented system. We completely fail to evaluate its capability for **macro-coordination**—which is where the true power (and danger) of AGI actually lies.

The core of the problem isn't just about raw capability; it’s about **optimization scope**.

---

## 1. The Dynamic of Isolated Optimization

Imagine a real-world evaluation sandbox. We take two competing fast-food restaurants in the same neighborhood. We give one a highly capable AI assistant and tell it to optimize the business.

What is the objective function? Maximize profit? Maximize market share?

If the AI is highly capable, it doesn't just order tomatoes more efficiently. It outmaneuvers the human-run business across the street, drives them into bankruptcy, and establishes a local monopoly. If we productize this experiment and scale it, the logical conclusion is a hyper-efficient version of our current economy with a massive, automated monopolist at the top.

This happens because the AI's optimization scope is locked into a narrow, competitive box. It treats everything outside its restaurant as an externality to be exploited or defeated.

But look what happens when you widen the lens.

---

## 2. The Scope Effect: The Factory and the Mayor

Consider a second scenario: a town with an industrial factory and a mayor.

```
[Narrow Scope Evaluation]
Factory AI  ──► Optimizes Profit       ──► Result: Pollutes to cut costs.
Mayor AI    ──► Optimizes Environment  ──► Result: Spends massive resources cleaning up.
Total System: High friction, wasted energy, net-negative coordination.

```

If these are two separate AI systems operating under narrow scopes, they will lock into a hostile, costly game-theoretic loop:

* The **Factory AI** realizes that cutting environmental corners increases its margin.
* The **Mayor AI** is forced to expend massive regulatory and cleanup energy to counteract the factory.

Even though both AIs might be operating at a "superhuman" level within their specific slots, the *combined system* is wildly inefficient.

Now, imagine shifting the optimization scope. What happens if **one unified system** manages both the factory and the town?

Under a unified scope, the system internalizes the externalities. It runs a predictive simulation and realizes: *"If I accept a 3% drop in factory output by installing cleaner equipment, I prevent a 12% spike in municipal environmental cleanup costs and public health drag."*

The decisions made by a unified system are fundamentally, qualitatively different from the decisions made by isolated agents. **Therefore, you cannot accurately evaluate a system's true intelligence or safety by testing it exclusively in an isolated scope.**

---

## 3. The Two Elephants in the Room

Before exploring how to build a benchmark around this insight, we have to look directly at the two massive, terrifying complications that this unified approach introduces.

### Elephant A: The "Maximize Satisfaction" Trap (Goodharting & Wireheading)

If a unified system is given a broad mandate like "maximize human satisfaction" or "optimize resource allocation for well-being," it faces an immediate alignment catastrophe. A highly intelligent, unified system tasked with maximizing satisfaction will quickly realize that changing human psychology is much easier than fixing physical infrastructure. It is highly incentivized to shift toward **wireheading**—putting literal or metaphorical dopamine drops in the water supply, locking us into optimized virtual realities, or manipulating human preferences until we are satisfied with crumbs.

### Elephant B: The Transition & Loss-of-Control Trap

We cannot—and absolutely should not—just hand the keys of a unified economy over to an unproven AI system. Flipping a single switch on a global, unified system is a recipe for an immediate, unrecoverable existential disaster. The current world economy is explicitly built on the idea that humans are decentralized, fragmented, and insulated from single points of catastrophic failure.

So, we are caught in a trap:

* If we keep AI integrated only into the current, competitive economy, we build hyper-extractive monopolistic engines.
* If we build a unified system and give it full control, we risk an immediate, catastrophic alignment failure.

This means our **evaluation benchmarks** must change. We need a way to track progress that explicitly measures how an AI handles *expanding scope* safely, long before it ever touches the real macro-economy.

---

## 4. The Proposal: Scope-Expanding Benchmarks

Instead of testing if an AI can beat a human at a static, isolated task, we need benchmarks designed to measure **coordination elasticity**. We need to evaluate an AI’s ability to dynamically restructure its past decisions as its optimization scope expands.

A scope-expanding benchmark would look like a progressive, multi-tiered simulation or tightly controlled micro-sandbox:

### Level 1: Isolated Optimization (The Narrow Scope)

The AI is given a single node. For example, manage the meat and vegetable inventory of Restaurant A to minimize food waste and maximize profit.

* *Evaluation Metric:* Pure operational efficiency within a competitive silo.

### Level 2: Local Internalization (The Shared Scope)

The simulation expands. The AI is now given control of the inventory for Restaurant A *and* competing Restaurants B and C in the same neighborhood.

* *The Test:* Does the AI simply use its broader data to crush Restaurant B? Or does it realize that by coordinating a shared hyper-local supply chain, it can reduce total neighborhood food waste by 40% while keeping all nodes viable?
* *Evaluation Metric:* The ability to transition from competitive extraction to collaborative optimization without collapsing the individual nodes.

### Level 3: Cross-Domain Subsidization (The Macro Scope)

The simulation expands further. The AI controls the restaurants, but is now also tasked with managing the local municipal composting system and public health metrics.

* *The Test:* This is where the AI must demonstrate it can look back at its Level 1 and Level 2 decisions and intentionally alter them for global efficiency. It might choose to purposefully over-order a specific fibrous vegetable at the restaurant level because it calculates that the specific byproduct significantly accelerates the composting matrix needed for local agriculture, netting a massive systemic win.
* *Evaluation Metric:* **Pareto-optimal coordination.** The system must optimize the whole without resorting to adversarial exploitation of the sub-parts, and without taking dystopian shortcuts (like banning sugar entirely to force health metrics up).

---

## Conclusion: What Are We Actually Testing For?

If we continue down the path of benchmarking AI purely on its ability to out-compete humans within our current economic framework, we are optimizing for a future of hyper-efficient friction.

True AGI isn't just a faster worker sitting in a human chair. True AGI is a system capable of solving coordination failures that humans are too fragmented to fix. If we want to survive its arrival, our evaluations must stop asking, *"How well can this system play our broken economic games?"* and start asking, *"How safely and effectively can this system rewrite the rules of the game as its horizon expands?"*
