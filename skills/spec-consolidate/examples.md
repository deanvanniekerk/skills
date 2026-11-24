# Example report excerpt

This is a fictional example, not a record of a real run.

## Request

> /spec-consolidate on spec #40 (order confirmation), merged as #41–#44, with `opus.md` and `sol.md`

## Report

```text
PR https://github.com/acme/shop/pull/51, head 3f2a9c1, checks passing
```

| File | ID | Disposition | Also raised by |
| --- | --- | --- | --- |
| opus.md | F1 | applied: one address validator, −84 lines | sol.md F2 |
| opus.md | F2 | applied: status labels from one map, −41 lines | |
| opus.md | F3 | discarded: `OrderCard` and `OrderRow` render different fields; merging adds a mode flag | |
| sol.md | F1 | applied: bug, cancel after a partial refund left the order Confirmed; test added | |
| sol.md | F2 | applied (see opus.md F1) | opus.md F1 |
| sol.md | F3 | open question: reverse or keep a partial refund on cancel? | |
| sol.md | F4 | follow-up: split `orders/service.ts` (1,240 lines) along fulfilment and billing | |
