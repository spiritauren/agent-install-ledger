# Install Ledger — example

> **Synthetic.** Every row below was invented to demonstrate the field format.
> Copy the file, delete the rows, and start your own.

| Name | What it does | Installed on | Why | Project | Location | Footprint | Lifetime | Why it can go | Uninstall command | Status |
|---|---|---|---|---|---|---|---|---|---|---|
| Playwright (Python) | Browser automation — drive a real browser from code | 2026-07-22 | Screenshots for a one-off report | Quarterly report | `~/Library/Python/3.9/lib/python/site-packages/playwright` | 134 M | temporary | Report shipped; not used again | `pip uninstall playwright` | in use |
| Chromium runtime | The browser Playwright actually drives (headless shell + ffmpeg) | 2026-07-22 | Same task | Quarterly report | `~/Library/Caches/ms-playwright/` | 554 M | temporary | Screenshots done — largest reclaimable item | `python3 -m playwright uninstall --all` | in use |
| imageio-ffmpeg | Video / frame processing | 2026-07-22 | Time-lapse assembly | Quarterly report | `…/imageio_ffmpeg/binaries/` | 73 M | phased | Needed until the video is delivered | `pip uninstall imageio-ffmpeg` | in use (until 08-12) |
| pip download cache | Package copies pip keeps so it need not re-download | cumulative | By-product of the installs above | — | `~/Library/Caches/pip` | 113 M | temporary | Download cache only; deleting it does not affect installed packages | `pip cache purge` | removable (awaiting user) |
| ffmpeg (static binary) | Turns timed screenshots into a time-lapse video | 2026-07-23 | Time-lapse rendering | Quarterly report | `~/.local/bin/ffmpeg` | 77 M | phased | Delete once the video is delivered | `trash ~/.local/bin/ffmpeg` | in use |
| poppler | PDF command-line tools (pdfinfo / pdftotext / pdftoppm) | 2026-08-21 | Resume HTML→PDF export + QA pipeline | Job search | `/usr/local/Cellar/poppler/26.08.0` | ≈276 M, incl. a freshly installed dependency tree | long-term | — | `brew uninstall poppler && brew autoremove` | in use |
| QR generator (temp deps) | Produced a QR code PNG for a local-network test | 2026-08-11 | Testing a page from a phone | Personal site | `/tmp/qr-gen` | 15 M | temporary | The QR PNG was produced | `trash /tmp/qr-gen` | deleted (2026-08-11) |

## What to notice

**1. Footprint is measured, not guessed.**
`du -sh` beats the vendor's download size. `poppler` is 33 M on its own, ~276 M once you count the dependency tree it dragged in. That gap is where the disk actually goes.

**2. "Why it can go" is written for a future you.**
"The report shipped" is a reason. "Not needed" is not a reason — it is the thing you will disagree with in six months.

**3. "Uninstall command" is a command, not a description.**
If you cannot write the exact command, you do not yet know how to remove the thing. Find out now, while the install is ten minutes old.

**4. Deleted rows stay.**
`deleted (2026-08-11)` with the date is the trail proving the review happened and the decision was carried out. A ledger that silently drops rows cannot be audited.

## Review log

```
2026-08-11  Reviewed 7 items. Verified footprint with du against the table —
            two rows were stale (Chromium had grown to 592 M). Deleted:
            QR generator temp deps (15 M, artifact produced). Kept: poppler
            and ffmpeg (still needed). pip cache flagged removable, awaiting
            the user's go-ahead.
```

One line per review. Date, what was checked, what was deleted, what was kept, what is waiting on a decision.
