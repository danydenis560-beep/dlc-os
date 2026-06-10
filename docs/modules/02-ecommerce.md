# Module 02 · E-Commerce Platform

> The commerce core — the kernel every channel and module builds on. Products,
> pricing, cart, checkout, orders, returns.

**Phase:** MVP (the foundation everything else needs).
**Related:** [Database Schema](../05-database-schema.md) · [API Design](../06-api-design.md) · [Payments](./10-payments.md)

## Features

| Feature | Notes | Phase |
|---|---|---|
| Product catalog | Products + sellable variants (SKUs) | MVP |
| Product variants | Options (size/color), per-variant price & stock | MVP |
| Categories | Self-referential tree, M:N products | MVP |
| Digital products | Files/licenses via `digital_assets`, delivery on purchase | MVP |
| Physical products | Weight/dimensions for shipping | MVP |
| Product bundles | Bundle = set of variants w/ quantities | MVP→P2 |
| Inventory management | See [Inventory module](./08-inventory.md) | MVP |
| Coupon system | Codes with conditions/limits | MVP |
| Discount engine | Automatic + code-based (%, fixed, free-ship, BxGy) | MVP |
| Customer accounts | Profiles, addresses, order history | MVP |
| Wishlist | Saved items per customer | P2 |
| Shopping cart | Channel-agnostic carts | MVP |
| Checkout system | Cart → checkout session → payment → order | MVP |
| Product reviews & ratings | 1–5★, verified-purchase, moderation | MVP→P2 |
| Order management | Lifecycle: pending→paid→fulfilled→… | MVP |
| Returns & refunds | Return flow + (partial) refunds | MVP |

## Data model
`products`, `product_variants`, `categories`, `product_categories`,
`product_images`, `product_bundles`/`bundle_items`, `digital_assets`, `carts`,
`cart_items`, `discounts`, `orders`, `order_items`, `refunds`, `returns`, `reviews`.
Full definitions: [Database Schema](../05-database-schema.md).

## Pricing & money rules
- Money = integer **minor units** + ISO `currency` (never floats).
- Pricing pipeline: line subtotal → discounts → tax → shipping → total. Deterministic & auditable.
- Inventory is **reserved** at checkout to prevent oversell, released on expiry/cancel.

## Key endpoints
`/products`, `/products/{id}/variants`, `/categories`, `/carts`, `/carts/{id}/items`,
`/carts/{id}/discounts`, `/checkout`, `/checkout/{id}/complete`, `/orders`,
`/orders/{id}/refunds`, `/orders/{id}/returns`. See [API Design](../06-api-design.md).

## Channel-agnostic by design
A cart/checkout/order means the same thing on web, Discord, Telegram, WhatsApp — the
core owns the logic; [channels](./03-discord-commerce.md) are adapters. This is the
architectural keystone (see [Architecture](../04-architecture.md)).

## Events emitted
`product.published`, `price.changed`, `cart.updated`, `checkout.completed`,
`order.placed`, `order.fulfilled`, `refund.issued` — consumed by inventory,
marketing, analytics, shipping, and AI memory.
