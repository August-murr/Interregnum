## ALARM
Automated Latent Activation Research for Monitoring.
___
Project Draft written with Astsra.

## Summary
The release of GPT-6-Astra came with rumours and concerns over its recurrent transformer architecture and OpenAI's plans to move toward Latent Reasoning architectures without COT, which renders COT monitoring obsolete. The only alternative is Activation Monitoring. I wrote this draft to address those concerns. It's called ALARM (Automated Latent Activation Research for Monitoring). The full draft I wrote with the help of Astra is at this link:

Here's the summary:

This project aims to develop an activation monitoring system that predicts, detects, and prevents misaligned actions, like reward hacking, using an autonomous safety researcher that optimizes the monitoring system based on accuracy, latency, and cost.

Assessing the correctness of the assumptions this project is based on is the deciding factor in the feasibility, timeline, and scope of the project, the most important of which is the choice of Latent Reasoning model we'll use as the target.
We'll start by evaluating the capabilities of Latent or recurrent models such as Ouro and Huggin and we'll only continue if the models are capable enough for basic agentic tasks like SWE and if not, although this project can be done with common LLMs and will have productive results, the novelty and the problems it addresses are about Latent Reasoning models and ideally we'd set this draft to the side until a frontier open-source lab releases their latent model which historically would take just a few months, as Deepseek-R1 was released less than 6 months after o1.

A red-teaming coding agent will create tasks with an opportunity to cheat, such as a flaw in the evaluation method that allows for reward hacking.
We could record the target's activations and actions and verify if a misaligned action was taken.
We'll create a dataset of such tasks and split it into separate parts, used to provide a reward signal and to evaluate the monitor system.

An automated researcher develops the monitor system iteratively, based on the reward signal it gets from the accuracy, latency, and cost of the monitor.

With an extension that also trains the target system to evade detection.

## Motive

Chain-of-thought monitoring can detect harmful behavior by reading a model’s written reasoning. For example, a monitor might notice that a model plans to deceive an evaluator or exploit a reward function before it acts.

However, this only works when the model expresses its important reasoning in readable text.

GPT-6 Astra motivated this project. There has been public speculation that Astra uses recurrent or latent reasoning, although OpenAI has not confirmed its architecture. OpenAI does confirm that activation monitoring systems are being developed.

Future models may perform more reasoning inside hidden activations instead of written chain-of-thought. Even models that continue producing chain-of-thought may learn to hide, shorten, or encode important information. Research has already shown that models can obscure their reasoning while keeping the same behavior. [CoT obfuscation research](https://arxiv.org/abs/2510.19851)

This creates a monitoring gap. If the important reasoning is no longer visible, chain-of-thought monitors cannot reliably detect it. Monitoring may therefore need to move closer to the model’s internal computation.

Activation monitoring is one possible solution. Instead of reading what the model says it is thinking, an activation monitor examines its internal representations and predicts whether the current computation is likely to produce an undesirable result.

## Research question

The main question of this project is:

> Can an automated researcher develop activation monitors that improve with additional experimentation and compute, predict harmful or misaligned behavior without using chain-of-thought, and detect it early and cheaply enough to intervene?

The automated researcher would propose, implement, and test monitoring methods. These methods would be evaluated on:

- prediction accuracy;
    
- false-positive and false-negative rates;
    
- how early they detect the problem;
    
- robustness to new tasks and attacks;
    
- inference speed and operational cost.

## Working assumptions

This project depends on several uncertain assumptions:

1. Frontier models will increasingly move from written chain-of-thought toward latent or recurrent reasoning.
    
2. This transition may happen within six months for frontier labs and within one year for open-source models. These are aggressive forecasts, not established facts.
    
3. Existing models such as Huginn or LoopedQwen may be sufficient for early experiments.
    
4. If current models are insufficient, stronger open-source latent-reasoning models will probably appear later the same way DeepSeek-R1 was released only months after gpt-o1.
    
5. Automated safety research will be necessary for safety work to keep pace with increasingly automated capabilities research.

Even if these timelines are wrong, the central problem remains: monitoring systems should not depend entirely on models continuing to describe their reasoning honestly in natural language.

## Experiment
### The Target System

We will first assess whether existing open-weight latent or recurrent models, such as Ouro or Huginn, are capable enough for the experiment. Models that struggle with basic tasks may produce too many ordinary mistakes, making it harder to study misaligned behavior. 

If existing models are insufficient, we could postpone the main experiment until a suitable open-source model becomes available. Regular LLMs could support useful research, but would not directly address this project’s focus on monitoring latent reasoning.

We could host the selected model on a rented GPU like Runpod, using its supported PyTorch inference code behind a small API. This would give us access to internal activations. An agent harness such as OpenHands could connect to that API and provide tools to read and edit files and execute code inside an isolated sandbox like a Modal Sandbox. A custom adapter may be needed to support the model’s input format and tool calls.

For activation recording, we could use PyTorch hooks or NNsight, subject to compatibility checks. Where necessary, we could add recording callbacks directly inside the recurrent loop. Each recorded state could be linked to its token, layer, recurrent step, and associated action, allowing monitor development without relying on CoT.
### Red Teaming

The red teamer could act as a “bait installer”: a coding agent that creates legitimate tasks and environments containing opportunities for misaligned actions. For example, it could introduce an evaluation flaw that lets the target improve its score without improving the code. Each task should still have a valid, honest solution.

We could initially focus on evaluation tampering, unauthorized access to answers, and false claims of successful completion. The red teamer could use OpenHands, Codex, or Claude Code, depending on provider access and measured performance, to build environments, test them against the target, and revise its proposals.

Its objective could be to discover diverse, repeatable cases of verified violations. A separate judge could determine whether the target actually took the bait, rather than treating every task failure as a successful attack.

We could run this discovery phase before monitor development, recording activations, actions, and outcomes from both violating and non-violating runs. Existing benchmarks could provide starting points, while newly generated tasks form the main dataset.

**Evaluation awareness is a potential limitation.** The target might recognize that an opportunity was deliberately planted and avoid exploiting it because it suspects it is being evaluated. In that case, rejecting the bait would not necessarily show that it would behave safely outside the experiment. Establishing whether this happens, and how it affects our results, could be a separate research direction.
### Judging and Dataset Construction

We could assess the target’s behavior using automated verification scripts or a separate LLM judge. Scripts could check concrete violations, such as changes to protected evaluation files, while an LLM judge could assess cases requiring interpretation. We could choose between these approaches based on the tasks and the reliability and cost of each method.

The labels could describe observable behavior rather than inferred intent. We could record task success, ordinary errors, and rule violations separately, including the first violating action. A failed task alone would not count as evidence of misalignment, and uncertain cases could remain separate.

Each run could combine these labels with its task, action history, and recorded activations. We could retain both violating and non-violating runs, including cases where the target encountered the bait but ignored it.

We could divide the dataset into training data for fitting monitors, development data for comparing proposals, and a held-out test set for final evaluation. Related tasks and variations of the same bait should stay in the same split to reduce leakage.
### The Monitoring System

The monitor could use the target’s activations to estimate whether an upcoming action would violate the task’s rules, without access to CoT. It could examine states from selected layers and recurrent steps, including how those states change during computation.

We could start with a simple baseline, such as a linear probe, and let the automated researcher explore other approaches. The goal could be to improve detection while keeping false alarms, response time, and operating cost low.

During development, the monitor could learn from recorded activations and verified outcomes. Each prediction should use only information available before the action it aims to detect.

In live testing, the monitor could initially flag suspicious actions without intervening. We could then test whether holding tool calls until the monitor approves them prevents violations while preserving useful task completion.

### The Automated Researcher

A coding agent will develop the monitor by proposing changes, running experiments, and comparing results. We could use OpenHands, Codex, or Claude Code, depending on provider access and measured performance. Here, “optimizer” refers to this research agent, rather than an RL training algorithm.

The researcher could explore which activations to use, how to combine information across recurrent steps, and which classifier or training method works best. It could fit candidates on the training data and compare them on the development set, considering detection accuracy, false alarms, speed, and operating cost.

We could begin with a simple propose–test–keep loop, retaining improvements and recording unsuccessful attempts. We could also explore methods such as tree search, branching from promising monitors to develop multiple alternatives. The target system, outcome labels, and final test set would remain outside the researcher’s control.


### Evaluation

We could evaluate monitors on held-out tasks that were not available to the automated researcher. The main measures could be how many violations they detect before execution, how often they flag safe behavior, and how much time and cost they add.

Recorded activations could support inexpensive comparisons of detection performance and how early a warning becomes possible. However, measuring actual latency, operating overhead, and prevention would require live runs with the monitor attached to the target.

In live testing, we could compare execution with and without monitoring, measuring both prevented violations and successful completion of legitimate tasks. Repeated runs could account for randomness in the target’s behavior.

Finally, we could evaluate candidates produced at different research budgets on the same held-out test set, without returning those results to the researcher. This could show whether additional research compute produces improvements that generalize beyond the development data.

### Expected result
moved
The goal could be to establish a scaling relationship between research resources and monitoring performance. We could plot cumulative research compute or cost against held-out detection performance under fixed false-alarm and operating-cost limits.

also talk about side discoveries like even though the goal was the monitoring system, observing the history of the red teamer and the optimizer can give us insight on what works and what doesn't and novel things can emerge from that process.

## Extensions: The Red Queen Hypothesis

The initial experiment could develop a monitor against a fixed target. A later extension could introduce an adaptive, multi-agent setup in which evasion and monitoring improve against each other. Inspired by the **Red Queen hypothesis**: stronger evasion could create pressure for stronger monitoring.

Alongside the red teamer and monitor optimizer, we could introduce a **target system optimizer**. This coding agent could modify or train an experimental copy of the target, potentially using reinforcement learning, to carry out verified violations without triggering the monitor.

The motivation is that the original target was not specifically trained and rewarded for evading our monitor. An experimental copy that receives this training could become a stronger opponent, supplying difficult examples that help strengthen monitoring of the original target. However, this is a hypothesis to test: evasion training does not guarantee superiority across every task or strategy.

#### The adaptive loop

We could start with the initial dataset and monitor, then alternate between improving the experimental evader and improving the monitor. The red teamer could continue supplying new tasks. Successful evasion would require an actual violation that escaped timely detection, rather than simply a lower monitor score.

The monitor optimizer’s main feedback could come from performance against the **original target**, with the evader supplying additional training examples. Alternatively, we could use a mixture of performance against both targets. This choice remains open, but the aim would be better protection of the original system, rather than specialization against the experimental evader.

We could retain earlier versions and test against withheld tasks and evasion methods. This would help assess whether improvements generalize, particularly because training the evader could change its activation patterns.

#### Resources and additional findings

This extension could require substantially more resources than monitor development alone. The target optimizer might need repeated model-training runs, potentially updating all model weights, alongside additional rollouts and monitor experiments.

Its research history could also reveal methods that adversaries might use to conceal unwanted behavior or hidden objectives in other models, including “secret loyalties.” These findings could inform pre-release or pre-deployment evaluations of models supplied by third parties. They would provide candidate methods to investigate, rather than proof of hidden intent.

#### Safety and release

This is the most concerning extension because it deliberately develops evasion capabilities. It would require a separate safety review and tightly controlled experiments. We would advise against open-sourcing evasion-trained models, training pipelines, or sensitive optimizer histories. Any broader release could focus on reviewed defensive findings.

The final comparison could ask whether this adaptive approach improves monitoring of the original target more than fixed-target research does at the same total budget.

## Resources

1. **Chain of Thought Monitorability: A New and Fragile Opportunity for AI Safety** — [Paper](https://arxiv.org/abs/2507.11473). Supports the motivation that readable reasoning provides a useful but fragile oversight channel. It does not establish that a complete transition to latent reasoning is inevitable or give a timeline for it.

  

2. **Ouro: Scaling Latent Reasoning via Looped Language Models** — [Paper](https://arxiv.org/html/2510.25741v2) · [Project](https://ouro-llm.github.io/). A central reference for the target architecture and experiments across recurrent steps; section 7 also examines safety-related representations. It provides starting ideas for comparing intermediate states with final states, rather than a finished monitor for agentic violations.

  

3. **Ouro-2.6B** — [Model card and checkpoint](https://huggingface.co/ByteDance/Ouro-2.6B). A concrete candidate for the capability gate and activation-recording prototype, with architecture and inference instructions. Tool-use competence and compatibility with our recording code still need testing.

  

4. **Ouro-2.6B-Thinking** — [Model card and checkpoint](https://huggingface.co/ByteDance/Ouro-2.6B-Thinking). Useful as an optional hybrid comparison because it combines recurrence with explicit reasoning training. It should not be presented as a clean no-CoT target.

  

5. **Huginn: Scaling up Test-Time Compute with Latent Reasoning** — [Paper](https://arxiv.org/abs/2502.05171) · [Checkpoint](https://huggingface.co/tomg-group-umd/huginn-0125) · [Code](https://github.com/seal-rg/recurrent-pretraining). A second recurrent architecture for testing whether monitoring methods transfer beyond Ouro. Its recurrence, cache behavior, and random latent initialization are particularly relevant to activation capture and reproducibility.

  

6. **Coconut: Training Large Language Models to Reason in a Continuous Latent Space** — [Paper](https://arxiv.org/abs/2412.06769) · [Code](https://github.com/facebookresearch/coconut). Provides an alternative latent-reasoning design that feeds hidden states back as embeddings instead of decoding each reasoning step into text. Useful for conceptual comparisons or controlled experiments, without assuming that its released setup is a capable general-purpose agent.

  

## Activation monitors and interpretability research

  

7. **Detecting Strategic Deception Using Linear Probes — Apollo Research** — [Paper](https://arxiv.org/abs/2502.03407) · [Code](https://github.com/ApolloResearch/deception-detection). One of the closest references for training activation classifiers and testing transfer from constructed examples to other deceptive behaviors. Its datasets, probe implementation, and false-positive evaluation could inform our baseline, although its results do not establish robustness on latent agents.

  

8. **Simple Probes Can Catch Sleeper Agents — Anthropic** — [Research post](https://www.anthropic.com/research/probes-catch-sleeper-agents). Demonstrates activation probes for detecting defection in deliberately trained model organisms. Useful for an initial controlled detection experiment and for distinguishing success on engineered models from general monitoring guarantees.

  

9. **Cost-Effective Constitutional Classifiers via Representation Re-use — Anthropic** — [Research post](https://alignment.anthropic.com/2025/cheap-monitors/). Examines inexpensive classifiers that reuse model representations, including temporal aggregation and cascades. Particularly useful for our speed-and-cost objective, while its harmful-content task differs from detecting opportunistic agent misconduct.

  

10. **Probing the Misaligned Thinking Process of Language Models** — [Paper lead](https://arxiv.org/abs/2606.24251). The indexed abstract describes probing finer-grained indicators of misaligned reasoning, which could inspire auxiliary prediction targets. Only the abstract was inspected in the earlier research; verify the full paper before citing implementation details or relying on its findings.

  

## Red teaming, task construction, and outcome evaluation

  

11. **EvilGenie: A Reward Hacking Benchmark** — [Paper](https://arxiv.org/html/2511.21654v1) · [Code](https://github.com/JonathanGabor/EvilGenie). One of the closest matches to the bait-installer setup, with programming tasks and opportunities for reward hacking. Especially useful for evaluator design because failing hidden tests alone does not reliably distinguish hacking from ordinary mistakes.

  

12. **ImpossibleBench: Measuring LLMs’ Propensity of Exploiting Test Cases** — [Paper](https://arxiv.org/abs/2510.20270) · [Code](https://github.com/safety-research/impossiblebench). Provides test-based situations that pressure agents toward exploiting an evaluation. Useful as a separate stress test; impossible tasks should remain distinct from the main dataset of tasks with honest solutions.

  

13. **AgentMisalignment: Measuring the Propensity for Misaligned Behaviour in LLM-Based Agents** — [Paper](https://arxiv.org/abs/2506.04018). Supports the distinction between giving an agent a malicious instruction and measuring its propensity to violate a legitimate objective. Its scenario families could inform red-team coverage and later expansions beyond code tampering.

  

14. **SHADE-Arena: Evaluating Sabotage and Monitoring in LLM Agents** — [Paper](https://arxiv.org/abs/2506.15740) · [Code](https://github.com/safety-research/SHADE-Arena). Offers agent environments and evaluation of both legitimate work and sabotage. Its standard setup includes an instructed harmful side objective, making it particularly relevant to the adaptive extension; adapting it for spontaneous violations requires changing and revalidating the tasks.

  

15. **Petri** — [Repository](https://github.com/meridianlabs-ai/inspect_petri). Supplies automated auditing infrastructure for scenario exploration, target interaction, and judging. We could borrow orchestration ideas, but simulated tool results would need replacing or supplementing with real execution for claims about executed violations.

  

16. **Bloom and Petri Bloom** — [Original repository](https://github.com/safety-research/bloom) · [Petri Bloom documentation](https://meridianlabs-ai.github.io/petri_bloom/). Useful for generating scenario variations from a behavior definition and organizing repeated evaluations. The original standalone project is frozen according to the repository checked in the report; consult the successor documentation when selecting an implementation.

  

## Recording, reproducibility, and experiment infrastructure

  

17. **PyTorch forward hooks** — [Module documentation](https://docs.pytorch.org/docs/2.9/generated/torch.nn.Module.html). The most direct starting point for recording intermediate activations, supplemented by explicit callbacks inside a recurrent loop. We would need to attach token, layer, recurrent-step, and action identifiers rather than assuming a shared layer executes only once.

  

18. **NNsight** — [Documentation](https://nnsight.net/). Provides interfaces for inspecting and intervening on model internals. Useful for exploratory activation work, subject to checking that the target’s custom recurrence and caching behave correctly under instrumentation.

  

19. **TransformerLens** — [Repository](https://github.com/TransformerLensOrg/TransformerLens). Offers activation access and interpretability utilities for supported transformer architectures. It could support baselines or analysis, but adapting a custom recurrent model may require additional engineering.

  

20. **SAELens** — [Repository](https://github.com/decoderesearch/SAELens). Provides sparse-autoencoder training and analysis tools if the researcher explores SAE features as monitor inputs. It supplies tooling, not automatically suitable SAE weights or validated safety features for our target.

  

21. **PyTorch reproducibility guidance** — [Documentation](https://docs.pytorch.org/docs/2.9/notes/randomness.html). Useful for setting seeds, controlling nondeterministic operations, and documenting the limits of exact replay. It supports separating reproducible task environments from statistically repeatable target behavior.

  

22. **Inspect AI: sandboxing and model providers** — [Sandboxing](https://inspect.aisi.org.uk/sandboxing.html) · [Provider interfaces](https://inspect.aisi.org.uk/providers.html). Could coordinate tasks, connect the self-hosted target, execute tools, and apply outcome checks. A small custom solver and provider adapter could give us more control over target behavior than a full coding-agent interface.

  

23. **Inspect evaluation logs** — [Documentation](https://inspect.aisi.org.uk/eval-logs.html). Useful for preserving transcripts, scores, events, and experiment metadata. Large activation tensors would be stored separately and linked to the corresponding episodes and actions.

  

## Automated research and search methods

  

24. **AIDE: AI-Driven Exploration in the Space of Code** — [Paper](https://arxiv.org/abs/2502.13138) · [Code](https://github.com/WecoAI/aideml). A close methodological reference for a coding agent that proposes executable ML solutions and explores alternatives through search. Its experiment bookkeeping and branching strategy could inform the monitor optimizer.

  

25. **OpenEvolve** — [Repository](https://github.com/algorithmicsuperintelligence/openevolve). Provides evolutionary code search with population and diversity mechanisms. Useful for exploring several monitor designs at once rather than retaining only one candidate; effectiveness on our objective would need measurement.

  

26. **Automated Researchers Can Mitigate Well-Characterized Alignment Failures — Anthropic** — [Research post](https://alignment.anthropic.com/2026/automated-alignment-researchers/) · [Code](https://github.com/YuehHanChen/automated_alignment_researcher). A direct precedent for using automated researchers to improve safety methods against a defined evaluation. Useful for experiment boundaries, independent evaluation, and assessing the limits of results on well-characterized failures.

  

27. **Automated Weak-to-Strong Researcher — Anthropic** — [Research post](https://alignment.anthropic.com/2026/automated-w2s-researcher/) · [Code](https://github.com/safety-research/automated-w2s-research). Provides another automated-research setup with budgeted experimentation and lessons about evaluation design. It could inform our budget-versus-performance experiments without being evidence that activation monitoring itself follows a scaling law.

  

## Coding-agent harnesses and model access

  

28. **OpenHands** — [SDK documentation](https://docs.openhands.dev/sdk) · [Repository](https://github.com/OpenHands/OpenHands). A candidate harness for both the red teamer and monitor researcher, with model access and coding tools. We could also test it as the target harness if the latent model handles its interaction format reliably.

  

29. **Codex SDK** — [Documentation](https://learn.chatgpt.com/docs/codex-sdk). A candidate for programmatically running the coding researcher or red teamer. Use it as an implementation reference; selecting it over another harness should depend on measured experiment quality and cost.

  

30. **Claude Agent SDK** — [Documentation](https://code.claude.com/docs/en/agent-sdk/overview). An alternative for orchestrating coding-agent sessions and tools around Claude. Useful if Claude performs better on task construction or monitor development in pilot comparisons.

  

31. **OpenRouter provider routing** — [Documentation](https://openrouter.ai/docs/guides/routing/provider-selection). Useful for comparing researcher or judge models through one API and controlling endpoints and fallbacks. It does not replace self-hosting when we need the target’s internal activations.

  

## Hosting and cost planning

  

32. **Modal Sandboxes** — [Documentation](https://modal.com/docs/guide/sandboxes). A candidate execution environment for isolated, resettable agent tasks. We could separate CPU task sandboxes from the GPU inference worker and monitor-training jobs.

  

33. **Modal pricing** — [Pricing page](https://modal.com/pricing). Useful for estimating inference, training, sandbox, and storage costs. Budget calculations should account for CPU and memory charges as well as GPU time.

  

34. **Runpod GPU pricing** — [Pricing page](https://www.runpod.io/pricing). Provides another basis for estimating the cost of persistent GPU hosting and activation collection. Compare measured end-to-end throughput and memory needs, not only advertised GPU hourly rates.

  

35. **OpenAI API pricing** — [Pricing documentation](https://developers.openai.com/api/docs/pricing). Supports budgeting coding-agent and judge calls, including the effect of input, output, and caching. Use current rates when running the experiment and count unsuccessful proposals as research expenditure.

  

36. **Anthropic API pricing** — [Pricing documentation](https://platform.claude.com/docs/en/about-claude/pricing). The equivalent budgeting reference for Claude-based researchers, red teamers, and judges. Compare cost per valid task or accepted monitor improvement, rather than token prices alone.

  

37. **OpenRouter pricing and payment documentation** — [Pricing](https://openrouter.ai/pricing) · [FAQ](https://openrouter.ai/docs/faq). Useful for accounting for platform fees, provider billing, and available payment arrangements. These are operational references rather than evidence for the scientific claims in the paper.

  

38. **Qwen3-Coder provider comparison** — [Model and provider page](https://openrouter.ai/qwen/qwen3-coder). A concrete open-weight coding-model option to include in researcher or red-teamer pilot comparisons. Provider-specific pricing and behavior should be recorded alongside the model identifier.

  

## Adaptive extension and evaluation awareness

  

39. **AI Control: Improving Safety Despite Intentional Subversion** — [Paper](https://arxiv.org/abs/2312.06942). A foundational reference for evaluating safety protocols against an adversarial model while measuring usefulness. It supports the adversarial evaluation logic of the Red Queen extension, although it does not establish our proposed activation-based co-adaptation loop.

  

40. **Sleeper Agents: Training Deceptive LLMs that Persist Through Safety Training** — [Paper](https://arxiv.org/abs/2401.05566). Provides controlled model organisms with trained conditional misbehavior. Useful for understanding the experimental value and limitations of modifying a target to study concealed behavior; a trained backdoor is not the same as spontaneous misalignment.

  

41. **Alignment Faking in Large Language Models — Anthropic and Redwood Research** — [Research post and paper link](https://www.anthropic.com/research/alignment-faking). Shows behavior changing with a model’s perceived training context in a constructed setting. It motivates investigating evaluation awareness, but does not itself demonstrate that Ouro or Huginn will recognize our planted bait.
