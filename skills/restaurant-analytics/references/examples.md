# Worked examples

## 1 · "How did Baner do last week by channel?"
1. Router → `orders_metrics` (channel + revenue).
2. Dictionary → "do" = net sales + orders + AOV.
3. Query (1 call):
```json
{ "metrics_view": "orders_metrics",
  "measures": [{"name":"net_revenue"},{"name":"order_count"},{"name":"avg_order_value"}],
  "dimensions": [{"name":"channel"}],
  "where": {"and":[{"eq":["branch_name","Baner"]},{"eq":["order_status","completed"]}]},
  "time_range": {"start":"2026-10-19","end":"2026-10-26"},
  "sort": [{"name":"net_revenue","desc":true}], "limit": 20 }
```
4. Self-check: channel rows sum to the total → answer with the breakdown table.

## 2 · "Top 5 items this month vs last month"
1. Router → `best_sellers_metrics` (uses `sale_timestamp`).
2. Two calls (current + previous month), each `limit: 5` … then a comparison table.

## 3 · "How much was paid by UPI yesterday?"
1. Router → `payments_metrics`; filters: completed + paid.
2. One call grouped by `payment_method`; answer with UPI first, then the table.

## 4 · View returns an error
> "The payments view isn't responding right now, so I can't give tender totals.
> Net sales from the orders view are available if that helps."
(No switching views silently, no cached numbers.)
