# Household Receipts — what's done and what's left

**Done**
- App is live: **https://proespaul.github.io/receipts/** (served from the `receipts` repo, root of `main`). The old address `/feedcalc/receipts/` redirects here.
- The join word `prairie482` is baked into the app and the backend — nothing to type on either phone.
- Backend = the "Household Receipts" Apps Script project (code in `backend/`), deployed as a web app (deployment `AKfycbwmez6dyUMTubrqBMnQ6ehYslyQbhS6mXGPGBAP5OJ8P4sU_OsZZJEDba2sKaIT5amI`); its `/exec` URL is baked into `index.html` as `DEFAULTS.url`.
- Sheet: https://docs.google.com/spreadsheets/d/18bVhLyeGozW-Z4pB32g_damnmpNo11LNLQa4V6h7HaE/edit · Photos: Drive folder "Household Receipts (photos)".
- `ANTHROPIC_API_KEY` is set in the project's Script Properties; the full pipeline (photo → read → filed) has been verified live.

**Left** — nothing on the backend. On the phones: install the app at the new address (see Phones).

**Phones**
- Paul (Android): open the link in Chrome → ⋮ → *Add to Home screen*. Once installed, "Receipts" also appears in the Share menu (screenshots and PDF receipts can be shared straight in).
- Hannah: open the same link; on iPhone, Share → *Add to Home Screen*. Pick her name under More once. If Safari asks about the camera every time, allow the camera for the site in Settings → Safari.

**Day to day**
- The app opens straight to the camera. Tap the shutter; it reads the lines and files them; no signal = queued until later.
- Zoom: slider with 1× / 2× / 3× taps under the viewfinder; the app reopens at the last zoom used. Below 1× needs the phone's wide lens, which Chrome on Android (incl. Pixel 9) does not expose to web apps — so the **0.5× 📱** chip opens the phone's own camera app instead (tap 0.5× there). Where a browser does expose the wide lens (iPhone labels it), the slider goes down to 0.5× directly. More → Camera shows what the phone exposes.
- The app moved out from under the Feed Calculator's address (Android was opening Receipts links inside the Feed Calculator app).
- *Long receipt* mode: take the top, then the rest, tap Done — filed as one receipt.
- *Photo / PDF* button (or the share sheet): screenshots of online orders (long ones are sliced automatically) and PDF receipts from email.
- *Spending* tab: Month / Quarter / Year / All with ‹ › arrows, "vs last period", tap a category or store to drill in, tap a bar to jump to that period.
- *Needs a look* (top of Spending): receipts to check (one tap "Looks right"), possible duplicates (Remove / Keep both), and bank charges with no receipt.
- *Bank check* (bottom of Spending): upload a CSV export, a PDF statement, or screenshots of the banking app. Every charge is matched to a receipt by amount + date (+ store name). Charges with no receipt get "Add as expense" (with a category; "Always file this way" remembers a rule in the sheet's **Bank Rules** tab) or "Not spending" (transfers, card payments).
- *Budget* tab: proposes a monthly budget per category from the last 3 months, editable, saved to the sheet's **Budgets** tab; shows left / over and "on pace for" live through the month.
- *Recent* tab: what this phone has sent (60 days), what is still waiting, and anything the reader wants a decision on. The Snap tab is camera only, with a one-line strip underneath ("✓ Staples $60.87 filed · 2 to tag? ›") that opens Recent.

**Tags (e.g. Homeschool) and the reimbursement policy**
- The sheet's **Tags** tab holds, per tag: the category claimed items go to, the claim rules (*Notes*), and **What counts / What does not count**. The Homeschool row carries the 2026–27 WISDOM / Gilbertine funding policy, distilled. The receipt reader gets these with every receipt, so it flags claimable lines even when nobody tagged the receipt — edit the text there and the reader follows it from the next receipt on.
- A flagged receipt shows **Homeschool?** in Recent and under Spending → *Needs a look* → "Could be Homeschool", with the lines it thinks qualify. **Tag as Homeschool** opens a checklist (flagged lines pre-ticked) — tick what you are claiming and save: those lines go to the *Homeschooling* category and onto the claim list, the rest keep their categories. **Not Homeschool** waves the hint off on every phone.
- Camera → **🏷️ Tag** → pick a tag; it stays on for every snap until turned off (🏷️ → *No tag*). The reader then claims the lines that fit the rules (everything, if it can't tell). Already-filed receipt → tap it → **🏷️ Tag / Change tag** → the same checklist.
- A tagged receipt gets: the tag on the Receipts row, the `Tag` mark on each claimed line in Items (`Tag Hint` is what the reader suggested), and its photo shortcut in Drive under **Household Receipts (photos) / Tagged / Homeschool**.
- Spending → *Tagged receipts* → Homeschool: the claim total (ticked lines, before tax), the claim rules, the lines, and links to the Drive folder and the sheet — that's the reimbursement pack (filter the Items tab on Tag = Homeschool for the itemized list; cross off unclaimed lines on the printed photo, as the policy asks).
- More tags: 🏷️ Tag → **＋ New tag…** (name, category for the claimed items, optional what-counts notes) — it gets its own Drive folder and the reader starts flagging for it too. Rows can also be added or edited in the **Tags** tab; categories in the **Categories** tab.

**Duplicates**
- Same photo twice: caught on the phone and on the server (hash).
- Same purchase from two phones or a photo + a screenshot: same store, date and total → if the printed time matches or the items match, it is filed as `duplicate` (not counted, "Keep both" undoes); if it can't be sure, it is counted but flagged "possible duplicate".

**Sheet tabs** — Receipts, Tags (editable: tag, category, claim rules, what counts / what does not count), Items, Bank, Bank Rules, Budgets, Categories (editable), Log.
Receipts with Source = `bank` are expenses filed from a statement; `superseded` means a real receipt replaced one later.

**Script properties** — `ANTHROPIC_API_KEY` (required), `CLAUDE_MODEL` (default claude-sonnet-5-5), `CLAUDE_THINKING` (`off` = no up-front thinking, the default; `adaptive` if receipts keep getting misread), `CLAUDE_EFFORT` (`medium` default; `low` / `high`). `APP_TOKEN`, `SPREADSHEET_ID`, `FOLDER_ID` are set by `setup`.

**If something's off**
- "Join word is wrong" → the backend's `APP_TOKEN` script property differs from the app; run `rotateJoinWord` only together with an app update.
- "did not answer as a web app" → deploy with access *Anyone* and use the `/exec` URL, not `/dev`.
- Stuck on "Will retry" → backend can't reach Claude (key missing/outage); it retries on its own, and the sheet's `Log` tab says why.
- Bank matching looks wrong → run `rematchAll` in the editor (safe any time).
- GitHub Pages for this repo publishes the `gh-pages` branch (that branch's existence is what switched Pages on). `main` is the source of truth: after each change, push `main` and then `git branch -f gh-pages main && git push -f origin gh-pages` — or switch Pages to `main` in the repo's Settings → Pages and drop the extra branch.
- Updating the app: edit the files at the repo root, bump `VERSION` in `sw.js` (and `APP_VERSION` in `index.html`); phones pick it up on next open. Updating the backend: paste `Code.gs`, then Deploy → Manage deployments → edit → New version (keeps the same URL).
