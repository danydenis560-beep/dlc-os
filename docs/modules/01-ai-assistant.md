# Module 01 · AI Assistant

> The intelligent shell of DLC OS — a voice + text assistant with memory that can
> *see* the whole business and *act* on it. The product's defining differentiator.

**Phase:** Core in MVP (text support + recommendations + memory); voice & advanced
automation in Phase 2; forecasting in Phase 3.
**Related:** [AI Architecture](../10-ai-architecture.md) · [Security](../09-security-architecture.md)

## Features

| Feature | Notes | Phase |
|---|---|---|
| Text conversations | Streaming chat on dashboard + every channel | MVP |
| Voice conversations | STT in / TTS out around the same agent loop | P2 |
| Memory (short-term) | Session context in Redis with rolling summary | MVP |
| Long-term memory | Facts/summaries per subject in `ai_memories` (pgvector) | MVP |
| Business assistant | Answer questions, navigate, act via tools | MVP |
| Sales assistant | Guide purchases, upsell, recover carts | MVP→P2 |
| Customer support assistant | Grounded answers, order help, handoff | MVP |
| Product recommendation engine | Retrieval over catalog + history + behavior | MVP |
| AI workflow automation | Multi-step tasks with confirmation | P2 |
| AI reporting | Generate narrative reports from analytics | P2 |
| AI analytics | Natural-language questions over metrics | P2 |
| AI inventory forecasting | Demand prediction over movement history | P3 (needs data) |
| AI marketing suggestions | Segments, offers, copy | P2 |

## How it works (summary)
The agent loads relevant **memory** + **retrieved context**, reasons with a
pluggable **LLM** (Claude/OpenAI/local), and calls **tools** that are thin wrappers
over real Core APIs — all under **RBAC + guardrails** (confirmations, limits,
audit). Full design in [AI Architecture](../10-ai-architecture.md).

## Data model
`ai_conversations`, `ai_messages`, `ai_memories` (+ `embedding`), `ai_actions`
(audit). Reads across catalog/orders/customers via permissioned tools.

## Key endpoints
`POST /ai/chat` (stream), `POST /ai/voice`, `GET /ai/memory?subject=`,
`POST /ai/actions/confirm`. See [API Design](../06-api-design.md).

## Tools the assistant can call (subset)
`search_products`, `recommend_products`, `get_order`, `issue_refund` *(high
sensitivity → confirm)*, `get_customer`, `add_note`, `segment_customers`,
`get_analytics`, `generate_report`, `create_campaign`, `forecast_inventory`.

## Guardrails (non-negotiable)
Runs under an RBAC role; sensitive actions need confirmation; per-role limits on
refunds/messaging; prompt-injection defense (content = data, not instructions); full
audit in `ai_actions`; PII controls + local-LLM option. See [Security](../09-security-architecture.md).

## UX
Dashboard `⌘K`/chat surface that can answer *and act*; inline AI brief on Overview;
assistant embedded in each channel for customers. See [UI/UX](../08-ui-ux-system.md).
