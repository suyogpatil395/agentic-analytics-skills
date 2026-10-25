# Menu Combinations skill: example

**Question:** *"Which combinations of the Special bar sold most this month, and how did each topping do?"*

```text
views   → order_item_modifier_metrics + order_item_metrics
method  → group modifiers per item line → pivot Base / Topping / Filling → count
checks  → topping quantities sum to 812 = item total ✓ · revenue sums to ₹1,26,840 ✓
```

**Combination-wise**

| Base | Topping | Filling | Quantity | Revenue |
|---|---|---|---:|---:|
| Mango | Dark chocolate | — | 142 | ₹21,300 |
| Strawberry | White chocolate | Cheesecake | 118 | ₹20,060 |
| Mango | — | Nutella | 97 | ₹15,520 |
| Coconut | Dark chocolate | Nutella | 88 | ₹14,960 |
| … | … | … | … | … |
| **Total** | | | **812** | **₹1,26,840** |

**Topping-wise**

| Topping | Quantity | Revenue |
|---|---:|---:|
| Dark chocolate | 361 | ₹57,760 |
| White chocolate | 244 | ₹39,040 |
| — (none) | 207 | ₹30,040 |
| **Total** | **812** | **₹1,26,840** |

_No menu list prices are shown; revenue is realised sales. Figures are synthetic._
