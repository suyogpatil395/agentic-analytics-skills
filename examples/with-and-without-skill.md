# Same question, with and without the skill

**Question:** *"What were net sales and orders for the Baner branch last week, by channel?"*

---

## ✗ Without a skill

```text
agent → list_metrics_views                      (8 views returned)
agent → get_metrics_view orders_metrics
agent → get_metrics_view item_sales_metrics
agent → get_metrics_view best_sellers_metrics
agent → query_metrics_view item_sales_metrics   sum(total_sales) by channel   ← wrong view, gross measure
agent → query_metrics_view orders_metrics       … no status filter            ← cancelled bills included
agent → query_metrics_view orders_metrics       … UTC dates                   ← late-night orders shifted
… 9 tool calls · ~41k tokens
```
> "Here is the data: `[{"channel":"Swiggy","total_sales":104230.5}, …]`"

Problems: wrong measure, cancelled orders counted, wrong day boundaries, raw JSON, no total, no check.

---

## ✓ With the restaurant-analytics skill

```text
router      → orders_metrics (net sales + channel)
dictionary  → net_revenue, order_count, AOV
filters     → order_status = completed · branch_name = Baner · store-local dates
agent → query_metrics_view orders_metrics      (1 call)
self-check  → channel rows sum to total ✓
… 2 tool calls · ~6k tokens
```

> **Net sales for Baner last week were ₹3.42 L from 512 orders (AOV ₹668).**
>
> | Channel | Net sales | Orders | AOV |
> |---|---:|---:|---:|
> | Dine-in | ₹1,24,300 | 188 | ₹661 |
> | Swiggy | ₹98,450 | 151 | ₹652 |
> | Zomato | ₹71,200 | 109 | ₹653 |
> | Takeaway | ₹48,100 | 64 | ₹752 |
> | **Total** | **₹3,42,050** | **512** | **₹668** |
>
> Want the same view compared with the previous week?

_Figures are illustrative (synthetic data)._
