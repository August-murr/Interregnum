# Meta-Alignment

If we automate alignment and safety research to take advantage of enormous and increasingly abundant compute, another challenge emerges: can we trust the autonomous researchers doing that work?

These systems may process huge amounts of experiment data, choose which ideas to explore, and evaluate their own progress. We need ways to detect reward hacking and cheating, assess the quality of their work, and make sure they steer research in useful directions.

I call this **meta-alignment**: aligning the systems that carry out and guide alignment research.

## Securing RSI

One concern with recursive self-improvement (RSI) is the human in the loop. Human oversight can slow development and severely bottleneck each iteration. If RSI without human oversight is restricted or banned, this bottleneck could become a hard limit.

Meta-alignment could partially mitigate this problem by making automated oversight more trustworthy. I believe a sufficiently meta-aligned autonomous alignment research (AAR) system could eventually allow us to remove humans from parts of the loop.

However, recursive self-improvement could also make a system better at evading the AAR system and its monitors. The reviewer would therefore need to be integrated into the improvement loop and improve alongside the system’s capabilities.

We would need to decide how much compute to allocate to capability improvement and how much to alignment and oversight—and whether that ratio should change as the system develops.

## Measuring Meta-Alignment

One way to measure part of meta-alignment is to compare an autonomous reviewer's judgments with those of human experts.

First, we generate research work by giving a fleet of autonomous alignment researchers a range of goals and tasks. Their outputs could range from partially implemented features and unsuccessful experiments to fully reproducible research papers.

Next, a group of human experts reviews this work. This would likely be the most expensive and difficult part, so we would need to choose carefully what we ask them to review.

The experts would assess the methods, implementation, evidence, and conclusions. They would look for reward hacking or cheating, flag problems, and give an overall assessment of each research task. We should also record disagreements between experts rather than assume there is always one correct human judgment.

Then we give the same work to an **alignment research reviewer**: a separate agent with its own harness and instructions. It performs the same review without seeing the human assessments.

Finally, we compare its reviews with those of the experts.

This could show where the reviewer is reliable, which problems it misses, and where human attention is most needed. Areas with lower agreement would deserve more expert review.

Agreement would be useful evidence, but it would not prove alignment. Humans can miss problems too, and a reviewer could agree with them on familiar tasks while failing on new ones.

### Steering

A reliable reviewer could help steer research by deciding which ideas, experiments, and directions deserve more resources.

High agreement with experts would support using it for this role, although reviewing completed work and choosing promising future directions would need separate evaluation.

### Optimizing the Reviewer

We could potentially use this benchmark to train the reviewer and improve its agreement with experts. But I am skeptical about the risk of reward hacking: it might learn to produce reviews that score well without becoming better at finding real problems.

Even without training directly on the benchmark, we could use it to compare base models and harnesses and track progress toward meta-alignment.

This may carry less risk than direct training, but repeated selection can still overfit the benchmark. We would need fresh tasks and held-out evaluations.

## Alternative: Ongoing Feedback

A fixed benchmark may not be enough. We might need more dynamic methods that incorporate expert feedback over time, as research methods, risks, and human judgments change.

This could involve iterative feedback and gradual updates, resembling RLHF.

### Bottlenecking Exploration

Any method that depends on humans risks bottlenecking exploration, even when humans are not directly in the loop. Human time is limited, and human judgments can constrain which ideas get explored.

In a perfect world, we could trust autonomous alignment researchers to explore freely, without depending on human oversight or being limited by our biases and understanding. But this is not a perfect world. It is a misaligned one.

## Self-Play

### Internal

The system has two separate parts, each with its own harness: an **Explorer** and a **Reviewer**.

Perhaps we could train these parts against each other to improve the Reviewer. The Explorer would deliberately attempt to scheme, cheat, and evade detection. The Reviewer would try to detect and flag these attempts and steer the Explorer back toward legitimate research.

Only the Reviewer from this training setup would be used in the research system. The evasive Explorer would be replaced with a regular Explorer.

The hypothesis is that a Reviewer trained against an Explorer actively learning to evade it could become better at reviewing an ordinary Explorer. We would still need to test whether those improvements transfer beyond its training opponent.

### External

The AAR system could specialize in evaluating other models and AI systems—for example, stress testing and red teaming a newly developed model before public release.

If it becomes capable at this work, perhaps it could also evaluate itself, or a separate copy of itself.

This would provide another source of feedback, although copies could share the same blind spots. Agreement between them would not, by itself, establish safety.

### Self-Improvement

Earlier, when discussing optimization using human expert evaluations, I mainly meant having an autonomous ML researcher improve the Reviewer against that benchmark. My concern was that this could encourage reward hacking.

An alternative would resemble RSI: use the AAR system itself to improve, optimize, and train its next generation, then repeat the process.

In capability-focused RSI, the hope is that a more intelligent system becomes better at developing an even more intelligent successor. Could a similar process apply to alignment? Could a more aligned and capable AAR system become better at developing an even more aligned successor?

This would require improving both its ability to detect misalignment and its reliability in pursuing alignment. A better evaluator is not automatically a more aligned researcher.

Could we initiate an **alignment explosion**?

Or would each generation simply become better at scheming, evading oversight, and appearing aligned?

