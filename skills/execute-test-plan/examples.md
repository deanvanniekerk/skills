# Example run excerpt

This is a fictional example, not a record of a real run.

## Report lines

```text
ORD-01 | PASS     | evidence/ORD-01.png | refusal shown; order still Draft after reload
ORD-02 | FAIL     | evidence/ORD-02.png | expected Confirmed after reload, saw Draft
ORD-03 | BLOCKED  | -                   | API returned 502 from the local proxy: "connect ECONNREFUSED 127.0.0.1:4000"
ORD-04 | OBSERVED | evidence/ORD-04.png | at 1280 px the orders table scrolls 14 px sideways; Cancel is off-screen until scrolled

Plan corrections: the button is "Confirm order", not "Confirm".
Browser: the environment's built-in browser; screenshots saved from it and copied to evidence/. Widths confirmed with innerWidth.
Clean up: QA Order 1 and 2 cancelled in the UI; signed out; colour scheme reset; tab closed; nothing left behind.
```
