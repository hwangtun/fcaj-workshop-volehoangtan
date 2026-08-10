---
title: "Agent Force - Deepdive Day 2"
date: 2026-08-08
weight: 2
chapter: false
pre: " <b> 4.2. </b> "
---

---

# Report: "Agent Force - Deep Dive Day 2 - Personalization, Evaluation & Optimization"

## Objectives of the Event

- Explore the advanced capabilities of **Amazon Bedrock AgentCore**.
- Learn how to build AI Agents that can retain conversational context and provide personalized user experiences.
- Understand the tools used to evaluate, monitor, and optimize AI Agents in production environments.
- Gain hands-on experience with the core components of **Amazon Bedrock AgentCore**.

## Speakers

- **Nghia Tran** – Agentic Solutions Architect
- **Anh Pham** – Cloud Consultant, G-Asia Pacific Vietnam

---

# Key Topics

## Amazon Bedrock AgentCore L300

The session focused on the core capabilities required to build production-ready AI Agents using Amazon Bedrock AgentCore.

### Memory

Memory enables AI Agents to retain conversational context through both short-term and long-term memory, allowing them to deliver more personalized responses.

With Memory, an AI Agent can:

- Remember user preferences and previous interactions.
- Maintain context across multiple conversations.
- Reduce repetitive user input.
- Generate more relevant and personalized responses.

This feature is particularly valuable for customer service chatbots, virtual assistants, and enterprise AI applications.

### Evaluations

AgentCore Evaluations provide a framework for measuring the quality and effectiveness of AI Agents using standardized evaluation metrics.

Key capabilities include:

- Measuring response accuracy.
- Evaluating helpfulness.
- Comparing multiple prompt or Agent versions.
- Using Judge Models to automatically score responses.

These evaluation tools enable developers to improve AI Agents based on objective metrics rather than subjective judgment.

### Observability

Observability provides complete visibility into the execution of AI Agents.

Developers can monitor:

- Execution logs.
- Request traces.
- Performance metrics.
- Agent execution flow.
- Runtime errors.

These capabilities simplify debugging, performance analysis, and system optimization.

---

# AgentCore Components

In addition to the three core capabilities, the session introduced several supporting components within AgentCore.

## Registry

Registry serves as a centralized repository for managing AI Agents, Tools, and Skills.

It enables:

- Component reuse across projects.
- Version management.
- Easier collaboration among development teams.

## Harness

Harness provides a simplified environment for developing and testing AI Agents.

It supports:

- Rapid environment setup.
- Running evaluation datasets.
- Comparing different Agent implementations.

This helps reduce deployment risks before releasing AI applications into production.

## Tools

AgentCore supports integration with external tools that extend the capabilities of AI Agents.

Examples include:

- Browser Tool.
- Code Interpreter.
- Payment Tool.
- External APIs and enterprise services.

These integrations allow AI Agents to perform practical tasks beyond natural language generation.

## Policy

Policy enables developers to define governance rules using the **Seda** policy language.

Policies can be used to:

- Control access permissions.
- Restrict Agent behavior.
- Enforce security requirements.
- Ensure compliance with organizational policies.

## Optimization

Optimization focuses on improving the overall efficiency of AI Agents.

Areas of optimization include:

- Reducing response latency.
- Improving prompt quality.
- Lowering model inference costs.
- Enhancing response quality.

---

# Hands-on Lab

The hands-on session provided practical experience with several AgentCore capabilities.

## Configuring Memory

Participants integrated Memory into an AI Agent, enabling it to retain information across multiple interactions.

This demonstrated the difference between stateful and stateless AI Agents.

## Exploring Agent Observability

Participants used the Observability tools to inspect the Agent execution process.

They observed:

- Agent invocation flow.
- Reasoning steps.
- Execution logs.
- Request traces.
- Processing time.

## Testing AI Agents

Participants used pre-built templates to develop an AI Agent capable of handling customer refund and return requests.

This exercise demonstrated a practical workflow for developing and testing enterprise AI Agents.

---

# Key Takeaways

## Understanding Amazon Bedrock AgentCore

- Learned how AgentCore abstracts many complex engineering tasks into reusable services.
- Understood the importance of Memory, Evaluations, and Observability throughout the AI Agent lifecycle.

## AI Agent Development

- Realized that building production-ready AI Agents is primarily a Software Engineering challenge.
- Memory, Tool Integration, Observability, and Evaluation are just as important as the underlying foundation model.
- Large Language Models represent only one component of a complete Agentic AI system.

## Enterprise Deployment

- Understood the importance of cost optimization and operational efficiency.
- Recognized the role of Policy in ensuring security and compliance.
- Learned how to design scalable and maintainable AI Agents for enterprise environments.

---

# Applying the Knowledge

The concepts learned during this session can be applied in future AI projects by:

- Following a **Design → Evaluate → Deploy** development workflow.
- Using Observability to monitor Agent performance and optimize token usage.
- Building reusable Agent and Tool repositories with Registry.
- Designing AI Agents that retain conversational context through Memory.
- Applying Policies to improve security and governance.

---

# Personal Reflection

This session significantly changed my perspective on AI Agent development.

Before attending the workshop, I primarily viewed AI Agents as intelligent chatbots capable of answering user questions. However, the session demonstrated that developing enterprise-grade AI Agents involves much more than prompt engineering or selecting a powerful foundation model.

Building production-ready AI systems also requires memory management, observability, evaluation, governance, optimization, and proper software engineering practices.

## Learning Experience

During the session, I gained a deeper understanding of:

- How Memory enables personalized user experiences by preserving conversation history.
- How Observability provides complete visibility into Agent execution and simplifies debugging.
- How Evaluations objectively measure AI quality before deployment.
- How Registry, Harness, Policy, and Optimization contribute to building scalable enterprise AI systems.

## Lessons Learned

- Developing AI Agents is a combination of Artificial Intelligence and Software Engineering.
- Monitoring, evaluating, and optimizing an AI Agent are just as important as choosing the underlying language model.
- A production-ready AI Agent should be designed with scalability, maintainability, security, and governance in mind.

## Event Photos
![Agent Force Deepdive day 2 images](/images/event-img/AFDD2_1.jpg)
![Agent Force Deepdive day 2 images](/images/event-img/AFDD2_2.jpg)
![Agent Force Deepdive day 2 images](/images/event-img/AFDD2_3.jpg)

> Overall, this session deepened my understanding of Amazon Bedrock AgentCore and its advanced capabilities for building enterprise AI Agents. The combination of theory and hands-on practice provided valuable experience in personalization, monitoring, evaluation, and optimization, which are fundamental for developing reliable and production-ready Agentic AI solutions.