# Applying the Bitter Lesson to AI Governance

*Written in collaboration with Sol.*

The title does not refer to a lesson learned from the history of AI governance. It refers to Richard Sutton's **Bitter Lesson**: methods that rely on general computation and scale tend to outperform systems built around large amounts of human-designed knowledge.

The idea I want to explore is whether the same principle can be applied to AI governance.

One way to express the Bitter Lesson is:

> Translate a problem into something that can be optimized with compute, then use compute—the most scalable resource available—to solve it.

## Three Areas

For this draft, it is useful to separate three related areas:

### Technical Safety

Technical safety develops methods for making AI systems safer. For the most part, it focuses on problems within the systems themselves rather than the institutions governing them.

### Governance

AI governance develops policies, laws, institutions, and coordination frameworks for controlling how AI is developed and deployed.

### Technical Governance

Technical governance develops technical systems that make governance policies possible or enforceable. Compute verification is one example: a policy may place limits or reporting requirements on large training runs, while a technical mechanism makes it possible to verify whether those requirements are being followed.

## Translating Governance into Technical Governance

Many governance proposals depend heavily on human implementation. They require monitoring, negotiation, coordination, interpretation, enforcement, and continued political cooperation. Each of these requirements can make a policy more difficult to apply reliably.

The central idea of this research direction is that a policy becomes more practical when some of those burdens can be translated into technical problems that are scalable with compute.

The main question is:

> What compute-optimizable technical solution would make this policy practical, verifiable, or enforceable?

The goal would be to examine governance proposals and identify where their main bottlenecks can be moved into technical governance. Instead of relying entirely on people and institutions to monitor compliance or coordinate behaviour, we would look for technical systems that can perform part of that work reliably and at scale.

## A Research Direction

The research would begin with existing or proposed AI policies and ask:

1. What human coordination or enforcement does this policy currently require?
2. Which part of that burden could be expressed as a technical problem?
3. Can the technical problem be turned into an objective that improves with additional compute?
4. Would solving it make the original policy substantially more practical?
5. Which governance problems benefit most from this translation?

This would help identify policies with the greatest potential to move from ordinary governance into technical governance.

## Pushing the Burden Toward Superalignment

The longer-term idea is to push as much of the implementation burden as possible away from slow and fragile processes of human coordination, negotiation, and enforcement—and toward technical systems that can improve through scalable computation.

This does not necessarily remove humans from choosing political goals or deciding what rules should exist. Instead, it reduces how much the success of those rules depends on continuous human attention and cooperation after they have been adopted.

If superalignment produces systems capable of solving difficult technical problems at scale, it may also become a tool for governance. Governance researchers would define the problems, constraints, and desired outcomes, while compute-scalable systems would help make the resulting policies verifiable and enforceable.

The broader research question is therefore:

> How many important AI governance problems can be converted into technical-governance problems that become easier to solve as more compute is applied?

Applying the Bitter Lesson to AI governance means searching for those translations—and prioritizing policies whose hardest implementation problems can be moved onto scalable technical systems.

