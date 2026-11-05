# Playwright fallback

For an environment with no browser tool. Make a new, empty temporary folder outside the repository for Playwright and your test scripts, and note its path. Nothing in it is kept:

```bash
PW_DIR=$(mktemp -d) && echo "$PW_DIR" && cd "$PW_DIR"
npm init -y && npm i playwright
npx playwright install chromium
```

Write each case as a script in `$PW_DIR` and run it with `node`:

```js
import { chromium } from 'playwright';

const browser = await chromium.launch();
const page = await browser.newPage({ viewport: { width: 1440, height: 900 } });
await page.goto('http://localhost:<port>/login');
await page.getByLabel('Email').fill('<email>');
await page.getByLabel('Password').fill('<password>');
await page.getByRole('button', { name: 'Sign in' }).click();
// …the case's steps, through real clicks and typing
await page.screenshot({ path: '<evidence>/<case ID>.png', fullPage: true });
await browser.close();
```

Find controls with `getByRole`, `getByLabel` and `getByText`. Dark theme is `page.emulateMedia({ colorScheme: 'dark' })`. Each `browser.newContext()` has its own storage, so roles can run side by side.

## Remove Playwright when done

```bash
pkill -f "$PW_DIR" || true                  # leftover scripts; their headless Chromium exits with them
cd "$PW_DIR" && npx playwright uninstall    # removes the Chromium build unless another Playwright install still uses it
cd / && rm -rf "$PW_DIR"                    # the Playwright package and your scripts
```

Chromium lives in the shared cache (`~/Library/Caches/ms-playwright` on macOS, `~/.cache/ms-playwright` on Linux), not in `$PW_DIR`, so deleting that folder alone leaves it behind. Run `npx playwright uninstall` without `--all`, so other projects keep the browsers they use.
