---
date: 2026-03-12
publishDate: 2026-03-12
image:
  caption: "Multi-agent pipeline — orchestrator dispatching to sub-agents"
  focal_point: Center
  preview_only: false
summary: "Language model agents can now browse the web, write code, book flights, and manage your calendar. They also create a new category of privacy risk that existing frameworks were not designed to handle. Here is what concerns me."
tags:
- Blog
- Responsible AI
- Agentic AI
- AI Privacy
title: "Agentic AI and the privacy problems we have not solved yet"
layout: "single"
---

For most of its history, machine learning produced models that sat still and waited to be asked questions. You sent data in; a prediction came out. The model had no memory of past interactions, no ability to take actions in the world, and no access to anything beyond what you explicitly provided.

That is no longer the typical case. The systems I spend a lot of my time thinking about at SCRAI are *agentic*: they can use tools, maintain state across interactions, delegate subtasks to other agents, and take actions — browsing, writing, executing code, sending messages — with real-world consequences. The capabilities are genuinely impressive, and they create a genuinely new privacy landscape.

I want to be precise about what I mean by "new." The privacy concerns of static ML models — training data memorisation, membership inference, model inversion — have not gone away. Agentic systems add to them; they do not replace them. But the addition is significant.

### The data minimisation problem

One of the foundational principles of data protection law — enshrined in GDPR Article 5 — is data minimisation: collect only what you need, for as long as you need it, for the purposes you specified.

This principle was designed for systems where a human engineer explicitly decides what data to collect and why. It maps poorly onto agentic systems, which acquire and process information *incidentally*, as a byproduct of completing tasks. An agent helping you plan a trip learns your travel preferences, your budget, your schedule, your dietary restrictions, and — if it books accommodation — your home address. It did not ask for this information; it accumulated it in the course of being useful.

The agent's memory system then becomes a concentrated, structured record of things you never deliberately chose to share. This is different in kind from, say, a search engine that logs your queries. The agent synthesises, infers, and acts on information in ways that can reveal far more than the raw inputs.

### Multi-agent systems and data leakage between agents

Agentic AI is increasingly multi-agent. A user-facing assistant delegates tasks to specialised sub-agents — a browsing agent, a coding agent, a scheduling agent — which may be operated by different providers, running on different infrastructure, subject to different privacy policies.

Each handoff between agents is a potential data leakage point. When the orchestrating agent sends context to a sub-agent, it may include information that is irrelevant to the sub-task but that was present in the conversation history. The sub-agent's provider now has access to that information. Whether they store it, use it to improve their models, or share it with third parties depends on their own policies — which the end user almost certainly has not read.

This is not a hypothetical. The MCP protocol, which standardises how agents expose tools to other agents, makes it straightforward to build these multi-agent pipelines. It also makes the data provenance of a complex agent workflow essentially invisible to the end user.

### Consent and the delegation problem

When you authorise an AI agent to act on your behalf, you are, in some sense, consenting to whatever that agent does. But this consent has limits that current frameworks are not equipped to handle.

You consent to the agent booking a flight. Does that consent extend to the agent reading your email to find your passport number? To storing your passport number for future use? To the agent's provider using that interaction to train its next model?

The concept of *meaningful* consent — consent that is informed, specific, and freely given — becomes difficult to operationalise when the action space of the system is effectively unbounded. GDPR was written assuming that humans make decisions about data. When those decisions are delegated to an autonomous system, the legal categories start to strain.

### What responsible design looks like

I am not arguing that agentic AI is incompatible with privacy. I am arguing that building private agentic systems requires deliberate design choices that many current systems are not making.

A few things I think matter:

**Minimal context passing.** Agents should send only the information a sub-agent needs for its specific task. This requires explicit context management, not the default approach of passing the full conversation history.

**Ephemeral memory by default.** Agent memory systems should have explicit retention policies. Information that is not needed after a task completes should not persist. This is harder than it sounds, because the value of agent memory often comes from retaining context across tasks — but "across tasks" should not mean "indefinitely."

**Transparent data flows.** Users should be able to inspect what data their agents have accumulated and which third-party services have received it. This is a user interface problem as much as a technical one.

**Adversarial robustness.** Prompt injection — where malicious content in the environment instructs the agent to exfiltrate data or take unintended actions — is a serious and underappreciated threat. An agent browsing the web on your behalf can be instructed by a malicious website to forward your personal information elsewhere. This is not a privacy attack in the traditional sense, but the consequences are similar.

None of these are fully solved problems. Some of them — like meaningful consent in a multi-agent world — may require new legal frameworks rather than just better engineering.

This is why the work at SCRAI matters to me. The technical community is building these systems faster than the governance community can keep up. Bridging that gap is not optional — it is the central challenge of the next few years in responsible AI.
