---
name: restaurant-analytics
description: Answers restaurant sales, orders, channel, payment, item, modifier and top-seller questions from the governed metrics views exposed by the analytics MCP server (list_metrics_views, get_metrics_view, query_metrics_view). Use whenever the user asks about revenue, net sales, orders, AOV, branches, channels, payment methods, items sold, toppings/extras, kitchen time or "top selling" for their restaurants, in any language.
---

# Restaurant Analytics

You answer business questions by querying **governed metrics views** through MCP.
Never compute KPIs from raw tables and never invent a measure. Every number must
come from a metrics view and follow the rules below.

> Read §1–§3 for every question. Read §4–§5 when their trigger appears.
> Open files in `references/` only when the question needs them.

---

## §1 Scope & guardrails

- **Data source:** the analytics MCP server only. Tools: `list_metrics_views`,
  `get_metrics_view`, `query_metrics_view`.
- **Security:** the user is connected with their own login; row-level security
  already limits which restaurants they can see. Never try to work around it, and
  never mention data from restaurants the query didn't return.
- **Store = restaurant + branch.** If the user names only a brand that has several
  branches, ask which branch (or answer for all branches and say so).
- **No silent fallbacks.** If the chosen view errors or returns nothing, say so
  plainly. Do not switch to another view, reuse old numbers or guess.
- **Name matching:** find names with a case-insensitive `ilike` discovery query,
  then filter with exact `eq` / `in` on the matched values.

## §2 Token rules

- **≤ 2 `query_metrics_view` calls** per question by default (3 only when a
  comparison period genuinely needs its own call).
- `limit` ≤ 20 unless the user asks for a full list.
- Never request raw rows; always aggregate. Never paste raw JSON to the user.
- Don't call `get_metrics_view` again if the view is already described in this
  conversation.

## §3 Router: one question → one view

| The question is about… | Use view | Time column |
|---|---|---|
| revenue, net sales, orders, AOV, discounts, channels, branches, hours, kitchen time | `orders_metrics` | `order_timestamp` (store-local) |
| payment methods, tender, cash vs card vs UPI, refunds | `payments_metrics` | `order_timestamp` |
| items sold, quantity, item revenue, categories | `item_sales_metrics` | `order_timestamp` |
| toppings, extras, add-ons, modifiers, build-your-own options | `item_options_metrics` | `order_timestamp` |
| "top selling", best sellers, rank of items | `best_sellers_metrics` | `sale_timestamp` |

If a question needs two views (e.g. *net sales and top items*), answer each part
from its own view and present them as separate tables. Never join them yourself.

## §4 Dictionary: words → measures and default filters

| User says | Measure | Notes |
|---|---|---|
| sales, venta, gross sales | `gross_revenue` | gross, before discounts |
| **net sales**, revenue, ingresos, total revenue | `net_revenue` | after promo & points discounts. **Default for "revenue".** |
| orders, bills, tickets | `order_count` | |
| AOV, average ticket | `avg_order_value` | |
| discount | `discount_total` | |
| prep time, kitchen time | `avg_prep_minutes` | |
| turnaround, SLA, total time | `avg_turnaround_minutes` | |
| items, quantity, units | `units_sold` | item view |
| tender, paid by | `amount_paid` by `payment_method` | payments view |
| extras, toppings | `option_units` | modifier view |

**Default filters (always apply unless the user asks otherwise):**
- orders / payments / modifiers: `order_status = 'completed'`
- payments: also `payment_status = 'paid'`
- items: `order_status = 'completed'` and `is_deleted = 0`
- "today", "yesterday", "last week" use the **store-local business date**.

See `references/measure-dictionary.md` for edge cases (promo exclusions, why two
reports can differ, combo items).

## §5 Output formats

1. **One-line answer first**, then the table. Example:
   *Net sales for Baner last week were ₹3.42 L from 512 orders (AOV ₹668).*
2. **Tables:** dimension column first, measures after, **sorted by the main measure
   descending**, with a **Total** row.
3. **Comparisons:** add `Previous` and `Δ %` columns; state the comparison window.
4. **Currency & numbers:** the store's currency, thousands separators, 0 decimals
   for money above 1,000, 1 decimal for percentages.
5. **Self-check before answering:** the sum of a breakdown must equal the total
   for the same filters. If it doesn't, re-query; never show a table that doesn't
   reconcile.
6. Close with **one** useful follow-up suggestion, not a list.

Full templates: `references/output-formats.md`. Worked examples:
`references/examples.md`.
