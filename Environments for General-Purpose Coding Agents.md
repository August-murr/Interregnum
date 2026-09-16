# Environments for General-Purpose Coding Agents
---
#### Written with Sol
---

I want to explore how to build environments for general-purpose AI coding agents that can take advantage of cheaper batch API usage and help automate or accelerate development tasks, possibly overnight.

The basic analogy is a team of junior developers. The agents can work sequentially or in parallel, handle simpler tasks, and ask for feedback or review when necessary. More compute makes it possible to run larger teams.

## Task Bite Size

The **bite size** is the maximum length or complexity of a task that an agent can reliably complete.

It depends mainly on the model and its context window, but it is also affected by prompts, tools, context management, and the surrounding environment.

Some models are simply not designed for long tasks. In those cases, a multi-agent setup may be useful: one planner agent breaks a larger project into tasks that fit within the other agents' maximum bite size.

## Overnight Work

Unlike a team of junior developers working fixed hours, coding agents can run continuously. However, the human supervising them is not available continuously, so feedback and review may be limited or completely unavailable overnight.

This makes overnight work better suited to robust setups with clear guardrails, isolated sandboxes, and reliable verifiers.

## Reducing Cost

If tasks can be queued and completed in the background without urgency, the system can take advantage of cheaper options offered by API providers.

For example, it could use batch processing at a lower price or route suitable tasks through slower and less reliable APIs. The goal is to trade speed and reliability for lower cost when the task allows it.

## Feedback Through Phone Apps

Some ongoing tasks may only need small pieces of feedback before they can continue. A phone app could make these requests easier to answer throughout the day, increasing the amount of time agents can keep working without waiting for the user to return to a computer.

## Open-Source Recipes

The environments and their different task-specific recipes could be released as open source. This would make it possible for others to reuse, test, and improve setups for different kinds of coding work.

