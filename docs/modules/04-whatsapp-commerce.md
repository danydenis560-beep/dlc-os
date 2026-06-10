# Module 04 · WhatsApp Commerce

> Commerce and support over WhatsApp — the highest-reach messaging channel, handled
> with eyes open about Meta's approval and template rules.

**Phase:** Phase 2 (approval started early in parallel).
**Related:** [Phase 2 Roadmap](../13-phase-2-roadmap.md) · [Marketing](./11-marketing.md)

## Features

| Feature | Notes | Phase |
|---|---|---|
| AI customer support | Grounded assistant over WhatsApp | P2 |
| Product browsing | Interactive lists/catalog messages | P2 |
| Order placement | Conversational + checkout link | P2 |
| Order tracking | Status & delivery updates | P2 |
| Automatic replies | FAQ, hours, order status | P2 |
| Customer segmentation | Shared CRM segments | P2 |
| Broadcast campaigns | Template-based, opt-in | P2 |
| Delivery updates | Triggered shipment notifications | P2 |

## Reality: this channel has gates (and we design for them)
The **WhatsApp Business API** is not an open bot platform:
- Requires a **verified business** and **Meta review** (lead time — start early).
- Outbound/marketing messages must use **pre-approved templates**.
- **Per-message pricing** and **24-hour customer-care window** rules apply.
- Strict **opt-in** requirements for broadcasts.

DLC OS builds these in: a **template manager**, **opt-in/consent tracking**, the
24-hour session-window logic, and approval-status surfacing. We treat compliance as a
feature, not a footnote.

## Architecture
Same **channel adapter** pattern as Discord/Telegram: a webhook receiver normalizes
WhatsApp messages into canonical commands; responses render as WhatsApp interactive
messages. No business logic in the adapter.

## Data model
Core commerce + `customers.channel_identities.wa_phone`, `communications`,
`campaigns` (type=`whatsapp`), template/consent records, `ai_*`.

## Key config (`.env`)
`WHATSAPP_PHONE_NUMBER_ID`, `WHATSAPP_BUSINESS_ACCOUNT_ID`,
`WHATSAPP_ACCESS_TOKEN`, `WHATSAPP_WEBHOOK_VERIFY_TOKEN`.

## Compliance notes
Opt-in before messaging; respect the care window; only approved templates for
outbound; honor STOP/opt-out; align with local laws. See [Security](../09-security-architecture.md).
