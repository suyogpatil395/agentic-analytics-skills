# Skill design patterns

These are the patterns that make an analytics skill reliable. They follow the
standard agent-skill format: a folder with a `SKILL.md` (YAML frontmatter + body)
and optional `references/`, loaded progressively.

| # | Pattern | Why it matters | In this repo |
|---|---|---|---|
| 1 | **Trigger-rich description** | the agent decides to load the skill from the description alone | frontmatter of both skills |
| 2 | **Progressive disclosure** | small core, details in `references/` → fewer tokens per question | `references/*.md` |
| 3 | **Scope lock** | one project, one source of truth | §1 |
| 4 | **Router table** | one question → one view; no guessing across views | §3 |
| 5 | **Business dictionary** | "net sales" always maps to the same measure | §4 |
| 6 | **Default filters** | completed orders, paid payments, not deleted, applied every time | §4 |
| 7 | **Token budget** | ≤2 queries, small limits, no raw rows | §2 |
| 8 | **Locked output format** | answers look the same every time and every tool | §5 |
| 9 | **Self-check / reconciliation** | breakdowns must sum to totals before answering | §5, menu §4 |
| 10 | **No silent fallback** | errors are reported, never patched with guesses | §1 |
| 11 | **Worked examples** | show the exact query shape the agent should produce | `references/examples.md` |
| 12 | **Explain differences** | dictionary of "why A ≠ B" answers common client doubts | `measure-dictionary.md` |

## Writing tips
- Write rules as tables and short imperatives; agents follow them more reliably than prose.
- Name exact measures and dimensions; never "the sales measure".
- Put the most frequently needed rules first (§1–§3).
- Version the skill with the semantic layer: when a measure changes, the skill changes in the same release.
- Test with a fixed question set and compare answers against the POS before every release.
