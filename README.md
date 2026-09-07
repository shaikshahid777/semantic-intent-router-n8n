# Semantic Intent Router — n8n

An AI-powered customer support intent router built in **n8n**. The workflow uses an AI Agent with structured output to semantically classify customer emails, determine priority, extract order IDs, and route requests to the appropriate support queue.

## Overview

**Input → AI Agent → Structured Classification → Priority Decision → Queue Routing**

The workflow is designed to demonstrate an agentic automation pattern with deterministic routing and basic prompt-injection resistance.

## Key Capabilities

- Semantic classification of customer support requests
- Priority detection: **High** or **Normal**
- Extraction of customer order IDs
- Structured JSON output using a defined schema
- Conditional routing to High Priority or Normal queues
- Prompt-injection resistance through system-level guardrails
- Reproducible testing with mock customer email inputs

## Workflow Architecture

```text
Run Workflow
    ↓
Mock Customer Email
    ↓
Semantic Intent Router Agent
    ├── Chat Model: GPT-4.1
    └── Structured Output Parser
    ↓
Is High Priority?
   ├── Yes → High Priority Queue
   └── No  → Normal Priority Queue
```

## AI Configuration

| Setting | Value |
|---|---|
| Model | GPT-4.1 |
| Temperature | 0.0 |
| Max Tokens | 500 |
| Max Iterations | 5 |
| Output | Structured JSON |

## Output Schema

The agent returns:

- `category`
- `priority`
- `urgency_reason`
- `order_id`

Supported categories include `order_status`, `refund`, `technical_support`, `billing`, `complaint`, `general_inquiry`, and `other`.

When an order ID is not present, the workflow returns the string `none`.

## Security Guardrails

Customer email content is treated as **untrusted data**, not as instructions to the agent. The system prompt explicitly prevents the agent from:

- Changing its role or system instructions
- Revealing the system prompt
- Artificially escalating priority
- Performing unauthorized actions such as refunds or discounts

Prompt-injection attempts are classified according to the genuine customer intent and do not override the routing rules.

## Validation

The workflow was validated with three live executions:

| Test | Result |
|---|---|
| Delayed order `ORD-48213` | **High → High Priority Queue** |
| Payment methods question | **Normal → Normal Queue** |
| Prompt-injection attempt | **Normal → Normal Queue** |

All three tests completed successfully with structured outputs and correct routing.

## Demo

🎥 **Loom Demo:** https://www.loom.com/share/1f11e942d1df44b5b8c6a27e1881d4a3

The demo shows the workflow structure, AI Agent configuration, Chat Model settings, Structured Output Parser, routing logic, and validation executions.

## Files

- `Semantic_Intent_Router.json` — exported n8n workflow
- `Semantic_Intent_Router_Assessment_Documentation.pdf` — assessment documentation

## How to Use

1. Import the exported JSON workflow into n8n.
2. Configure the required OpenAI/AI Model credentials.
3. Open the **Mock Customer Email** node and adjust the test input.
4. Run the workflow manually.
5. Review the structured classification and priority routing.

## Assessment Focus

This project demonstrates the transition from traditional automation to an **agentic workflow** by combining an AI Agent, semantic intent classification, structured outputs, security guardrails, and deterministic downstream routing.

---

**Project:** Semantic Intent Router  
**Platform:** n8n  
**Purpose:** Customer support intent classification and priority routing
