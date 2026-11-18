# Examples

Synthetic subjects. Model names are families; the chair seats the newest of each the host lists.

## A game mechanic

Request: "Council this stamina mechanic before I build it." The design lives in chat.

1. Seat Claude Opus and Codex GPT Astra at effort `high`.
2. Write the full design to `<scratchpad>/stamina.md`: the loop (sprint, dodge and climb spend stamina; it regenerates after 1.5 s idle), the goal (exploration that rewards route planning, not button-mashing), the numbers (100 max, sprint 12/s, dodge 25, regen 30/s), and one worked fight: two dodges and a sprint leave 38 stamina, enough for one more dodge.
3. Brief both with the path, the goal, "already decided: stamina exists; out of scope: combat damage", and the questions "Is regen too forgiving? What exploits does the idle delay open?"
4. Opus: `revise`, 6/10, dodge-cancelling the idle delay resets regen for free. Astra: `revise`, 7/10, same exploit; also proposes a stamina-upgrade skill tree.
5. Adopt the dodge-cancel fix (both found it, and it breaks the goal). Decline the skill tree as a later idea: it is new scope.
6. Round 2 on the revised file, with what changed and why the tree was declined. Both `ship`.

## A UI

Request: "Get a council on the new settings page." The app runs locally.

Evidence: `settings-390.png`, `settings-1280.png`, `settings-dark-1280.png`, `settings-error-390.png`, and `http://localhost:5173/settings`. The goal: a returning user changes their notification email in under ten seconds. Members may browse the page and change nothing.

Opus scores 8 and wants the save button pinned at phone width; Astra scores 6, finds the error text failing contrast in dark mode, and wants a different font. Adopt both layout fixes, decline the font (taste the goal does not settle), retake the four screenshots, and reconvene.

## Council review in a PR

```markdown
## Council review

Rounds: 2

| Member | Round 1 | Round 2 |
| --- | --- | --- |
| Claude Opus | revise 7/10 | ship 9/10 |
| GPT Astra | revise 6/10 | ship 9/10 |

Declined: font change (taste the goal does not settle).
```
