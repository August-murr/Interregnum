# Meta-Alignment

If we automate alignment and safety research to take advantage of enormous and increasingly abundant compute, another challenge emerges: can we trust the autonomous researchers doing that work?

These systems may process huge amounts of experiment data, choose which ideas to explore, and evaluate their own progress. We need ways to detect reward hacking and cheating, assess the quality of their work, and make sure they steer research in useful directions.

I call this **meta-alignment**: aligning the systems that carry out and guide alignment research.

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

