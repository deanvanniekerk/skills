---
name: execute-test-plan
description: Execute a written manual test plan in a browser, with screenshot evidence and a filled report. Use when asked to execute or run a test plan.
license: MIT
---

# Execute Test Plan

1. **Read** the plan fully, then start the apps it names. With a lane in your brief, first take that lane's slot as the plan's Environment says; the run is that lane's suites only.
2. **Run** every case of your run in plan order by driving the app in a browser. Record PASS / FAIL / BLOCKED, and OBSERVED for an observe case, which has no pass/fail. Wrong behaviour is a FAIL; a label or placement that differs from the plan is a plan correction. A case you cannot drive is BLOCKED with the exact error, including a gesture whose timing the device tool cannot hold, such as a simulator double-tap.
3. **Finish** as below.
4. **Report** in the plan's report template, saved where the plan or your brief says, and return it as your final message.

Application code stays unchanged; nothing is committed or pushed. Prefix every record you create with `QA `.

## Browser

Use the browser tool your environment provides. With none, or when it fails on its first three tries, set one up from [playwright.md](playwright.md).

- Sign in through the UI and use the real controls; assert what is rendered.
- Evidence is a PNG file at `<evidence>/<case ID>.png`, for every 📸 case and every FAIL, taken once the expected result is on screen. Save it and copy it there; a screenshot shown only inline is not evidence.
- Screenshots may be scaled. Confirm a viewport width by measuring it in the page (`innerWidth`), not from the image or the resize reply.
- Element references go stale when the page re-renders or a popup covers the control: take a fresh snapshot and act again.
- Roles share one browser profile's storage. Sign out before signing in as the next role, or give each role its own profile.
- Phone is 390 × 844, a narrow window 800 wide; dark theme is the browser's colour-scheme setting.

## Finish

- Restore no data.
- Stop every server you started, by its PID. Close every browser tab you opened and reset the colour scheme you changed; with the Playwright fallback, remove it as that file says.
- Keep the report and the evidence screenshots.
- **Disposable environment,** such as a slot: once the report and evidence are saved, run the plan's teardown command, such as releasing the slot.
- **Shared environment:** put back each setting a case changed, such as a role or an assignment; leave the `QA ` records.
- List in the report the records you created, any data you edited directly, and the teardown.
