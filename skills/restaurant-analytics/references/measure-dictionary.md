# Measure dictionary: edge cases

## Gross vs net
- **Gross sales** = net sales + promo discount + points discount.
- **Net sales / revenue** = what the restaurant actually earned after promotions.
- When a user says just "sales" in a report context, show **net** and mention gross
  in one line if the gap is above 5%.

## Why two reports can disagree
| Symptom | Usual cause | What to say |
|---|---|---|
| Item total ≠ order total | items exclude service charge and delivery fee | "Item revenue excludes delivery and service charges." |
| Top-selling qty ≠ items qty | top-selling counts linked extras separately | "Top-selling includes extras sold as separate lines." |
| Yesterday differs from POS by a few orders | scheduled orders placed today for tomorrow | "One order was scheduled for the next day." |
| Day totals shift after midnight | UTC vs store-local time | always use store-local business date |

## Combos and build-your-own items
- A combo line's revenue belongs to the combo, not split across components.
- Free modifiers (price 0) still count in quantity.

## Kitchen time
- Use preparation time for kitchen performance; turnaround for customer wait.
- Exclude orders with missing timestamps; say how many were excluded.
