---
name: menu-combinations
description: Answers build-your-own menu questions for a dessert brand: which Base + Topping + Filling combinations sold, how many of each option, attach rates and branch splits, using the item and modifier metrics views over MCP. Use whenever the user asks "which combination sold", "how many with chocolate topping", option popularity, or variant (Classic / Special / Turbo) breakdowns.
---

# Menu Combinations

Build-your-own products are sold as **one item line + several modifier lines**
(Base, Topping, Filling). This skill turns those lines into clear combination and
option reports that always reconcile.

## §1 Scope
- Views: `order_item_metrics` (item lines) and `order_item_modifier_metrics`
  (modifier lines). Default filters: completed orders, `is_deleted = 0`.
- Row-level security already limits data to the user's brand.
- **Never show menu list prices** (modifier upcharges) in sold-data output; show
  realised **Revenue** from sales only.

## §2 Standard output (unless the user asks for less)
Produce all of these, each with **Quantity** and **Revenue**, sorted by Quantity
descending, every row shown (no "top N" unless asked):
1. **Combination-wise**: Base + Topping + Filling per sold line
2. **Base-wise**, **Topping-wise**, **Filling-wise**
3. **Branch-wise** when more than one branch is in scope

## §3 Method
1. Query modifier lines grouped by `order_item_id` + `modifier_group` + `modifier_name`.
2. Pivot per item line → one row per line with its Base, Topping, Filling
   (`—` when an option wasn't chosen).
3. Count identical combinations; weight by item quantity.
4. Revenue per line = the line's own charged total (`item_line_revenue_measure`).

## §4 Mandatory self-check
- In **each** option table, the Quantity column must sum to the item's total
  quantity sold for the same scope.
- The **Revenue** column must sum to the item's total revenue for the same scope.
- If either fails (e.g. a line with two toppings), re-check the grouping; never
  show a table that doesn't reconcile.

## §5 Example
| Base | Topping | Filling | Quantity | Revenue |
|---|---|---|---:|---:|
| Mango | Dark chocolate | — | 142 | ₹21,300 |
| Strawberry | White chocolate | Cheesecake | 118 | ₹20,060 |
| Mango | — | Nutella | 97 | ₹15,520 |
| … | … | … | … | … |
| **Total** | | | **812** | **₹1,26,840** |

See `references/combination-method.md` for multi-topping lines and variants.
