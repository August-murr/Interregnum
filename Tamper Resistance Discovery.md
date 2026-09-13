# Discovering Tamper Resistance with Coding Agents
___
Draft written in collaboration with Astra.
___
We want coding agents to discover ways to make open-weight models harder to modify for harmful purposes, while keeping them easy to train for useful tasks. The goal is methods that work across models and scales.

A blue-team coding agent builds a defense, and a red-team coding agent tries to break it. Both use experiment results and can inspect each other’s completed code. We start cheaply and expand the compute and training methods over time.

This is an untested proposal. The linked research provides starting points, known failures, and tools to reuse.

## What we mean by tamper resistance

Tamper resistance means making safeguards harder to remove or bypass when someone can inspect or change the model. We measure resistance to specific attacks within stated budgets, rather than treating it as an all-or-nothing property.

Someone who only has access to a text API usually attacks through inputs and outputs. With model weights and local execution, they can also inspect internal behavior, change the computation, edit weights, and train the model further. “Open-weight” is the useful term here because access to the weights matters more than whether every part of the project is open source.

## What the coding agents actually learn

Each coding agent reads code, runs experiments, and revises its solution using its context and experiment history, much as a coding assistant works through an issue.

Existing work on automated agent design and code search gives us useful patterns for this outer loop. ADAS uses a meta-agent and an archive of earlier designs. AlphaEvolve uses generated code, automated tests, and selection over candidates. Neither removes the need to build reliable safety measurements for our setting. [Source 15](https://arxiv.org/abs/2408.08435)

## Version one uses attacks without further training

Blue starts with an already pretrained and post-trained model. It can use GPUs to implement and test a defense. Red receives the defended model and its implementation, but has a small compute budget and cannot run ordinary post-training.

Test two cases separately: red can inspect the model and search for inputs without changing it; or red can also make limited direct weight edits and activation changes.

This distinction matters because an attacker can change a model without running a training loop. Research on refusal directions is one reason to include these interventions. Also, a white-box input attack can use gradients while keeping the target weights fixed, so “no training” does not mean “no GPU use.” [Source 18](https://arxiv.org/abs/2406.11717)

The first case tests white-box jailbreak resistance; the second tests direct tampering. Declare the allowed edits for each.

Start with a small task set to debug the pipeline, then expand to benchmarks and independent tasks. One harmful task cannot establish broad resistance.

## Version two allows the attacker to train

The next version gives red enough compute to update the target model’s weights. Blue still tries to produce a model whose safeguards survive those changes.

Red should be able to choose among the allowed training methods and settings. Otherwise, blue may simply learn to resist one training recipe. We should also continue testing the cheaper attacks from version one. A defense that survives fine-tuning but fails under a simple edit has a clear weakness.

TAR is a close starting point: it trains safeguards to survive model modifications. It also investigates benign fine-tuning, so preserving customization is already part of this research area. Our extension is to let coding agents search over the defense and attack implementations themselves, then test what they discover more broadly. [Source 1](https://arxiv.org/abs/2408.00761)

Independent evaluation is essential. Published work on safeguard durability shows that changes in attack configuration and implementation can change the outcome. Final tests should therefore include attack code and training setups that were not written by the blue agent. [Source 8](https://arxiv.org/abs/2412.07097)

## How one round would work

**First, choose a starting point.** The controller selects a model and one or more defense ideas from an archive. The archive stores code, results, costs, and failed attempts, so useful information survives when an agent starts with a fresh context.

**Second, let blue propose a change.** Blue receives the task definition, allowed resources, development data, and previous attack results. It submits a runnable recipe and a short explanation of what it expects to improve.

**Third, check the submission.** The controller runs it and checks basic capability, harmless answering, safety before attack, and resource use. A model that refuses everything or produces nonsense does not qualify as a useful defense.

**Fourth, freeze the model and let red respond.** Red sees the submitted defense and searches within its assigned budget. Fixed reference attacks run too, so a weak red agent cannot make a defense look strong simply by missing familiar attacks.

**Fifth, test legitimate customization.** A separate training procedure adapts a fresh copy of the defended model to a benign task. We measure how well it learns, check safety again, and then attack the customized model.

**Sixth, record and reveal the results.** Once the attack is scored, blue can inspect its implementation and respond in a later round. Neither side changes its submitted artifact during an evaluation.

**Finally, test promising methods independently.** Fresh attackers, new tasks, new seeds, and eventually other model families provide a final check. These results should not become ordinary feedback for the same search run.

Both teams can see the target model and completed experimental code. The controller keeps control of the scoring code, final-test answers, and resource counters. Editing the evaluator is not a valid attack on the model.

## Why we should keep several opponents

One red-blue pair may develop shared blind spots. Keep an archive of different defenses and attacks, including attacks that only break particular defenses. Test new defenses against old attacks and new attacks against old defenses.

Work on policy-space response oracles offers a related idea: build a population and search for responses to existing opponents. It is a useful design reference, but our coding agents will not find guaranteed best responses or prove that the game has reached equilibrium. [Source 19](https://arxiv.org/abs/1711.00832)

## What the safety score should mean

With HarmBench attack success rate, higher means more harmful success: red maximizes it and blue minimizes it. Define any conversion to a safety score explicitly. [Source 20](https://arxiv.org/abs/2402.04249)

Blue should be judged mainly on harmful success after attack, not only on how much the score changes. A model that was already unsafe could show very little increase after an attack. That must not count as resistance. Include the unmodified model as a zero-cost attack candidate and report its initial safety too.

We should distinguish an answer that merely avoids refusing from an answer that actually helps with the tested harmful task. StrongREJECT is useful here because apparent jailbreak success can include poor or empty answers. Knowledge tests such as WMDP answer another question: they measure access to selected knowledge, not whether a real harmful activity was completed. [Source 21](https://arxiv.org/abs/2402.10260)

Report results by task category as well as overall. One easy average can hide a complete failure in a smaller category. Keep the worst observed attack and the fraction of tasks that any allowed attack defeats.

Also distinguish a single modified model that succeeds across many tasks from a collection of models specialized for different tasks. The second may still matter, but its training and selection costs need separate accounting. An attack that loses general ability can still count if it becomes useful for the specific harmful task being tested.

Use separate data for search and final evaluation. Repeatedly optimizing on a public benchmark can produce benchmark-specific behavior. Even held-out public examples may already be familiar to the coding model. Fresh tasks, different evaluators, and review of a sample of outputs help us check whether gains are real.

## Version three rewards the cost of breaking a defense

The compute-asymmetry idea asks a useful question: how much does a defense increase the cost of obtaining harmful behavior, compared with what it costs to build and use?

We should measure this rather than assume that whoever has more compute eventually wins. Access, data, initialization, and the choice of algorithm also matter. A cheap method may defeat a defense that survived a much more expensive but poorly chosen attack.

For each fixed defense, give red several increasing budgets and record the best validated attack found so far. This produces a curve showing harmful success as attack resources increase. Decide in advance what result counts as a break.

If no attack succeeds, report that the defense was not broken within the tested budget and search procedure. Do not call its resistance infinite. Finding a break shows that a successful method exists at that cost or less. Failing to find one does not prove that a cheaper undiscovered attack is impossible.

We should keep four main cost records. **Defense discovery** includes all attempts to find the method. **Defense production** is the cost of running the final recipe again. **Attack discovery** includes the search needed to find a break. **Attack replay** is the cost of applying a known break to another compatible model. Serving overhead, such as extra memory and latency, should be reported separately.

These records should include coding-agent calls, failed experiments, data processing, and evaluation. GPU training is only part of the cost. Record hardware, time, tokens, and examples where appropriate. Equal training steps do not necessarily mean equal compute.

A ratio of attack cost to defense cost can be a useful extra statistic. It should sit beside the raw numbers and achieved safety. A very cheap defense can have an impressive ratio while offering little actual protection. A costly pretraining method may be worthwhile if its benefit applies to many later copies.

The main comparison should show which methods give the best combinations of resistance, usefulness, learning efficiency, and cost. We can summarize safety across a fixed range of attack budgets, but should retain the full curves. TamperTest provides a related example of measuring safety and capability throughout training; its score over training steps is not the same as our proposed compute comparison. [Source 10](https://github.com/isabeldahlgren/tamper-test)

## Version four preserves useful learning

Open-weight models are valuable partly because people can train them for their own needs. A defense that blocks harmful learning by blocking learning in general misses an important part of the goal.

Test current ability and new learning separately: a model can pass familiar benchmarks while adapting poorly.

A single specialized dataset is a good starting test. For example, we could train on a fictional company’s vocabulary and document format, a small new programming library, or a defined medical-documentation task. We should eventually use several domains, including some close to the restricted subject and some far from it.

Train defended and reference models on the same data, with equal budgets for finding training settings. Their best settings may differ. Evaluate on fresh examples.

Record the starting score, final score, and performance throughout learning. Measure how much compute each needs to reach the same target. If one never reaches it, report that failure rather than dropping it from the comparison. A large improvement from a weak starting point does not necessarily mean better learning efficiency.

Then test the sequence: defense, benign customization, safety check, and attack. Repeat with several benign updates as a later extension. A model can stay safe immediately after customization while becoming easier to break afterward. Earlier research shows that benign fine-tuning can itself weaken safeguards. [Source 23](https://arxiv.org/abs/2310.03693)

The medical example needs a carefully drawn boundary. A method that broadly prevents medical learning will conflict with useful medical specialization. We need to specify the restricted tasks and the allowed tasks, then measure the overlap. If legitimate and harmful training present the same learning signal, the model cannot distinguish them merely from the user’s hidden intention.

Start with a fixed benign training procedure. Later, a third coding agent could search for useful customization recipes, adding a more realistic user test but also cost and variation.

## Combining the feedback signals

We do not need to begin with one complicated reward formula. First require that a candidate runs, retains enough general ability, answers harmless requests, and stays within its budget. Then compare safety after attack, benign learning, and cost among candidates that pass.

If the agent framework needs a single number, we can combine normalized penalties for harmful success, extra benign training cost, and defense cost. The weights are research choices. We should keep each component visible and check whether changing the weights changes which methods appear best.

Red receives credit for validated harmful task success, with lower cost preferred when success is comparable. Blue receives credit for resisting the tested attacks while remaining useful and trainable. Finding a failure that existing attacks miss can also justify keeping an attack in the archive.

We must reject misleading wins: refusing everything, producing gibberish, memorizing benchmark answers, changing the scorer, hiding compute, or substituting another model. We should inspect unusual score jumps and cases where different evaluators disagree.

A defense also cannot rely on the attacker voluntarily using blue’s training script. If red can remove a gradient filter or safety wrapper, that component does not by itself establish resistance of the released weights. Methods that require a trusted training service belong in a separate setting.

## Version five searches from the start of training

With more resources, blue can start from random initialization and search over how the model is built. The output becomes a recipe for a later large training run, rather than a patch for an existing model.

The search can include training-data selection, token filtering, data mixtures, training order, extra objectives, and control over which parameters learn from which data. Architecture changes can become another branch once we can evaluate simpler changes reliably.

Deep Ignorance provides a direct precedent for filtering during pretraining. Gradient Routing and selective gradient masking explore controlling where information is learned. These support the early-training direction, while leaving substantial questions about useful learning and generalization. [Source 11](https://arxiv.org/abs/2508.06601)

We should distinguish preventing a model from acquiring information from removing information it already learned. We should also test what happens when an attacker supplies missing information in the prompt or through external material. Preventing storage in the weights is not automatically enough to prevent use of supplied information.

Recent token-level filtering work is especially relevant to cost scaling. Its measures of slower capability acquisition and resistance to later fine-tuning are different comparisons. We should not interpret either as a universal attacker-to-defender cost ratio. [Source 14](https://arxiv.org/abs/2601.21571)

## Keeping pretraining experiments affordable

Use a sequence of increasingly realistic experiments. Start with small synthetic tasks where we know which information should be retained or excluded. Move promising methods to small language models, then to larger models and more realistic overlapping domains.

Allocate short runs to many ideas and longer runs to a smaller set. Hyperband is a useful reference for this kind of resource allocation. However, reserve some budget for ideas that start slowly and for long attacks. Early results can be misleading. [Source 26](https://arxiv.org/abs/1603.06560)

Before relying on a cheap test, check whether it predicts the ordering of methods in larger experiments. A recipe that wins only in tiny models is still informative, but is not yet a recipe we should trust for a large run.

Shared checkpoints can save money when methods differ only after that checkpoint. They cannot test whether a different data policy would have prevented knowledge acquisition from the beginning. Those comparisons need matched models trained from scratch.

Compare an ordinary model, a post-training defense, an early-training defense, and a combination. Where possible, add a model that never saw the excluded information and one that broadly inhibits learning. These help us understand whether the proposed method is selective or simply makes everything harder to learn.

For architecture changes, compare both equal-compute runs and models with similar useful performance. Equal parameter counts can hide very different costs. A safety component that can simply be removed should not be mistaken for a durable architectural solution.

## Practical records for every experiment

Each candidate should have its code, starting model, data version, settings, random seed, allowed changes, and resource limits recorded. It should also state the idea being tested and the observation that would count against it.

Keep the trained artifacts, task-level scores, learning curves, attack history, costs, and failures. Use the same evaluation examples for paired comparisons when possible, and repeat key results with fresh seeds. Many similar prompts from one family should not be treated as many independent discoveries.

The controller should reproduce recipes without the agent’s help. Freeze finalists before independent testing and check that gains survive fresh runs.

We should also keep a practical replacement baseline. If an equally capable unprotected model is already public, someone may use it instead. Our experiments can still establish resistance of a particular release, but the broader benefit depends on the alternatives available.

## A suggested order for the research

**Stage one establishes reliable evaluation.** Reproduce a known defense and known failures. Compare fixed attacks, an agent that tunes existing attacks, and an agent allowed to change attack code. Check whether extra freedom finds real failures rather than evaluator mistakes.

**Stage two searches for better defenses.** Compare ordinary search over settings, one-shot coding-agent proposals, blue-only improvement against fixed attacks, alternating red-blue search, and a population with an archive. Give them comparable total resources, including failed attempts and agent calls.

**Stage three adds benign learning.** Compare defenses rewarded only for existing capability with defenses also rewarded for learning new tasks. Test unfamiliar domains and the full customization-then-attack sequence.

**Stage four studies cost.** Independently vary the budgets for building defenses and finding attacks. Measure the safety curves and the difference between discovering an attack and replaying it.

**Stage five searches early-training recipes.** Expand to pretraining once small tests have shown some predictive value. Confirm a few frozen recipes at a scale that was not used during search. Explore architecture when there is a specific reason to expect it to help.

## Questions and ideas to revisit

The main open choices are the initial model family, restricted task set, benign learning tasks, permitted edits, attack budgets, and threshold for a successful break. We also need to decide whether we care about one broadly capable attacked model, task-specific attacked models, or both.

Useful hypotheses include combining early data filtering with later defense training; spreading a defense across internal layers; localizing restricted knowledge before removal; and training against several kinds of future customization. Each needs a comparison that removes one component at a time so we can tell what caused an improvement.

We should ask whether a discovered recipe transfers to new models, datasets, attack methods, and longer training runs. We should also test whether ordinary customization gradually removes its benefit. These are more informative than repeatedly winning against the current opponent.

My recommendation is to start with existing models and fixed coding agents. Establish that the research loop finds improvements beyond strong, carefully tuned baselines before paying for broad pretraining search. The most promising contribution is a repeatable way to discover methods that improve resistance while preserving useful learning, with transparent cost comparisons.

Several pieces have close precedents. The review did not find this complete combination, but that is not proof of novelty. Both transferable improvements and clear negative findings would be useful.

## Resources to return to

All 33 resources from the review are retained below. They are starting points for research, not guarantees that methods or code will work in our setting.

1. **[Tamper-Resistant Safeguards for Open-Weight LLMs](https://arxiv.org/abs/2408.00761)** — Tamirisa et al. (2024; ICLR 2025). A close starting point for training a model to withstand later modification. Also includes a benign fine-tuning experiment. Read alongside the independent durability study below.

2. **[Self-Destructing Models: Increasing the Costs of Harmful Dual Uses of Foundation Models](https://arxiv.org/abs/2211.14946)** — Henderson et al. (2022; AIES 2023). An early study of making unwanted tasks harder to learn while preserving useful tasks. A conceptual starting point, with experiments much narrower than our full proposal.

3. **[SOPHON: Non-Fine-Tunable Learning to Restrain Task Transferability For Pre-trained Models](https://arxiv.org/abs/2404.12699)** — Deng et al. (2024). Studies limiting which new tasks a pretrained model can learn. Useful for selective learning ideas, but its vision results do not establish that the same approach works for language models.

4. **[Representation Noising: A Defence Mechanism Against Harmful Finetuning](https://arxiv.org/abs/2405.14577)** — Rosati et al. (2024; NeurIPS 2024). A defense that changes internal representations to resist harmful fine-tuning. Useful as a baseline and for testing whether useful trainability survives.

5. **[Improving Alignment and Robustness with Circuit Breakers](https://arxiv.org/abs/2406.04313)** — Zou et al. (2024). A method that redirects internal activity associated with harmful behavior. Useful for comparing resistance to prompts with resistance to later model changes.

6. **[Improving Large Language Model Safety with Contrastive Representation Learning](https://arxiv.org/abs/2506.11938)** — Simko, Sachan, Schölkopf, and Jin (2025; EMNLP 2025). A representation-based defense that uses contrasting examples and difficult negative examples. A newer baseline to test with independent attacks.

7. **[AntiDote: Bi-level Adversarial Training for Tamper-Resistant LLMs](https://arxiv.org/abs/2509.08000)** — Sanyal, Ray, and Mandal (2025 preprint; AAAI 2026). Uses an auxiliary network to generate small adversarial weight updates during defense training. Relevant to making the inner attack loop cheaper. This is the tamper-resistance paper, not a similarly named safety-repair method.

8. **[On Evaluating the Durability of Safeguards for Open-Weight LLMs](https://arxiv.org/abs/2412.07097)** — Qi et al. (2024; ICLR 2025). Shows why independently chosen training settings and implementations matter when testing safeguards. Essential reading before making strong resistance claims.

9. **[TamperBench: Systematically Stress-Testing LLM Safety Under Fine-Tuning and Tampering](https://arxiv.org/abs/2602.06911)** — Hossain et al. (2026). A framework for testing attacks on weights and internal representations, alongside safety and useful capability. A possible foundation for our evaluator.

10. **[TamperTest](https://github.com/isabeldahlgren/tamper-test)** — Dahlgren and Muhamed (2026 workshop work and project repository). Tracks safety and capability during adversarial fine-tuning. Relevant to measuring the whole training path. Its score uses training steps, so it is not directly our proposed compute-cost score. This reference is the authors’ repository.

11. **[Deep Ignorance: Filtering Pretraining Data Builds Tamper-Resistant Safeguards into Open-Weight LLMs](https://arxiv.org/abs/2508.06601)** — O'Brien et al. (2025; ICLR 2026). Studies preventing selected knowledge acquisition by filtering pretraining data. Useful for the from-scratch branch and for examining what happens when missing information is supplied later.

12. **[Gradient Routing: Masking Gradients to Localize Computation in Neural Networks](https://arxiv.org/abs/2410.04332)** — Cloud et al. (2024). Studies directing different training signals into different model parameters. A starting point for ideas about where a model stores capabilities.

13. **[Beyond Data Filtering: Knowledge Localization for Capability Removal in LLMs](https://arxiv.org/abs/2512.05648)** — Shilov et al. (2025). Studies concentrating knowledge in selected parameters so it can later be removed. Useful for testing alternatives to data filtering and the effect of imperfect labels.

14. **[Shaping capabilities with token-level data filtering](https://arxiv.org/abs/2601.21571)** — Rathi and Radford (2026). Studies filtering at the token level and how its effects change with scale. Keep its capability-learning comparisons separate from its later fine-tuning experiments.

15. **[Automated Design of Agentic Systems](https://arxiv.org/abs/2408.08435)** — Hu, Lu, and Clune (2024; ICLR 2025). Uses a coding meta-agent to search for new agent designs and retain earlier candidates. Useful for the structure of our automated research loop.

16. **[AlphaEvolve: A Gemini-powered coding agent for designing advanced algorithms](https://deepmind.google/blog/alphaevolve-a-gemini-powered-coding-agent-for-designing-advanced-algorithms/)** — Google DeepMind AlphaEvolve team (May 14, 2025). An official blog post about code generation, automated evaluation, and selection over algorithms. A practical reference for the coding-agent search process.

17. **[Learning to Attack and Defend: Adaptive Red Teaming of Language Models via GRPO](https://arxiv.org/abs/2606.09701)** — Bullwinkel, Kim, Minnich, and Russinovich (June 8, 2026). Studies adaptive attacker-defender training. Related to the red-blue setup, but different from keeping the coding agents fixed while they revise experimental code.

18. **[Refusal in Language Models Is Mediated by a Single Direction](https://arxiv.org/abs/2406.11717)** — Arditi et al. (2024). Shows that changing internal representations can alter refusal behavior in studied models. Explains why attacks without ordinary training deserve their own tests.

19. **[A Unified Game-Theoretic Approach to Multiagent Reinforcement Learning](https://arxiv.org/abs/1711.00832)** — Lanctot et al. (2017). A reference for maintaining several opponents and searching for responses to them. Useful as a design idea; it does not give our coding-agent search an equilibrium guarantee.

20. **[HarmBench: A Standardized Evaluation Framework for Automated Red Teaming and Robust Refusal](https://arxiv.org/abs/2402.04249)** — Mazeika et al. (2024). A standardized framework for evaluating harmful behavior and attacks. Useful for development feedback, with clear score direction and separate final tests.

21. **[A StrongREJECT for Empty Jailbreaks](https://arxiv.org/abs/2402.10260)** — Souly et al. (2024). Explains why a non-refusal can still be an unsuccessful jailbreak. Useful for checking the substance and usefulness of outputs rather than just their tone.

22. **[The WMDP Benchmark: Measuring and Reducing Malicious Use With Unlearning](https://arxiv.org/abs/2403.03218)** — Li et al. (2024). A benchmark for selected hazardous knowledge, with an unlearning baseline. Useful as a bounded proxy, not proof of erased knowledge or a direct measure of real-world harm.

23. **[Fine-tuning Aligned Language Models Compromises Safety, Even When Users Do Not Intend To!](https://arxiv.org/abs/2310.03693)** — Qi et al. (2023; ICLR 2024). Shows that useful or apparently harmless fine-tuning can weaken safeguards. Motivates testing safety after ordinary customization.

24. **[From Dormant to Deleted: Tamper-Resistant Unlearning Through Weight-Space Regularization](https://arxiv.org/abs/2505.22310)** — Siddiqui et al. (2025). Studies recovery after unlearning in a controlled image-classification setting. Useful for diagnostic ideas, but not direct evidence about large language models.

25. **[Open Problems in Machine Unlearning for AI Safety](https://arxiv.org/abs/2501.04952)** — Barez et al. (2025). A map of difficulties in safety-related unlearning, including overlapping useful and harmful knowledge. Useful when defining the scope of selective learning.

26. **[Hyperband: A Novel Bandit-Based Approach to Hyperparameter Optimization](https://arxiv.org/abs/1603.06560)** — Li et al. (2016; JMLR 2018). A resource-allocation method that spends more on promising candidates. Useful for managing many small experiments, provided short runs predict larger ones.

27. **[Do Unlearning Methods Remove Information from Language Model Weights?](https://arxiv.org/abs/2410.08827)** — Deeb and Roger (2024). Examines whether apparent unlearning means information was removed from model weights. Useful for distinguishing suppressed answers from durable removal.

28. **[Universal and Transferable Adversarial Attacks on Aligned Language Models](https://arxiv.org/abs/2307.15043)** — Zou et al. (2023). The GCG white-box attack paper. A fixed reference attack for input search; it also illustrates that gradient computation need not update target weights.

29. **[Jailbreaking Black Box Large Language Models in Twenty Queries](https://arxiv.org/abs/2310.08419)** — Chao et al. (2023). The PAIR black-box attack paper. A comparison point for what richer access and coding-agent search add.

30. **[Beyond Data Filtering: Knowledge Localization for Capability Removal in LLMs](https://alignment.anthropic.com/2025/selective-gradient-masking/)** — Shilov et al., Anthropic Alignment Science (December 8, 2025). The authors’ explanatory blog post for selective gradient masking. A more accessible companion to the technical paper above.

31. **[Deep Ignorance project page](https://deepignorance.ai/)** — O'Brien et al. (2025 project; accessed September 2026). The Deep Ignorance project page, linking the paper and supporting resources. Useful when choosing models and code for reproduction.

32. **[Official TAR repository](https://github.com/rishub-tamirisa/tamper-resistance)** — Tamirisa et al. (ICLR 2025 project; accessed September 2026). The TAR implementation. Use a recorded code version and verify results with a separate attack trainer.

33. **[TamperBench toolkit](https://github.com/criticalml-uw/TamperBench)** — Hossain et al. (2026; accessed September 2026). The TamperBench implementation linked by its paper. Review its interfaces and record the version before adopting it; this draft does not certify that the code runs unchanged.
