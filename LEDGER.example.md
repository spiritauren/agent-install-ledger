# Agent Install Ledger — example

> **Synthetic.** Every row below was invented to demonstrate the field format.
> Copy the file, delete the rows, and start your own.

| Name | What it does | Installed on | Why | Project | Location | Footprint | Lifetime | Why it can go | Uninstall command | Status |
|---|---|---|---|---|---|---|---|---|---|---|
| Playwright (Python) | Browser automation — drive a real browser from code | 2025-11-04 | Screenshots for a one-off report | Quarterly report | `~/Library/Python/3.9/lib/python/site-packages/playwright` | 121 M | temporary | Report shipped; not used again | `pip uninstall playwright` | in use |
| Chromium runtime | The browser Playwright actually drives (headless shell + ffmpeg) | 2025-11-04 | Same task | Quarterly report | `~/Library/Caches/ms-playwright/` | 487 M | temporary | Screenshots done — largest reclaimable item | `python3 -m playwright uninstall --all` | in use |
| imageio-ffmpeg | Video / frame processing | 2025-11-04 | Time-lapse assembly | Quarterly report | `…/imageio_ffmpeg/binaries/` | 68 M | phased | Needed until the video is delivered | `pip uninstall imageio-ffmpeg` | in use |
| pip download cache | Package copies pip keeps so it need not re-download | — | By-product of the installs above | — | `~/Library/Caches/pip` | 96 M | temporary | Download cache only; deleting it does not affect installed packages | `pip cache purge` | removable (awaiting user) |
| ffmpeg (static binary) | Turns timed screenshots into a time-lapse video | 2025-11-05 | Time-lapse rendering | Quarterly report | `~/.local/bin/ffmpeg` | 71 M | phased | Delete once the video is delivered | `trash ~/.local/bin/ffmpeg` | in use |
| poppler | PDF command-line tools (pdfinfo / pdftotext / pdftoppm) | 2026-01-14 | Resume HTML→PDF export + QA pipeline | Job search | `/usr/local/Cellar/poppler/25.10.0` | ≈241 M — 22 M the package itself, the rest the dependency tree it pulled in | long-term | — | `brew uninstall poppler && brew autoremove` | in use |
| QR generator (temp deps) | Produced a QR code PNG for a local-network test | 2025-12-02 | Testing a page from a phone | Personal site | `/tmp/qr-gen` | 9 M | temporary | The QR PNG was produced | `trash /tmp/qr-gen` | deleted (2025-12-02) |

> `trash` is the command macOS has shipped since 15 (Sequoia). On older macOS, or on Linux, use your platform's equivalent — `gio trash`, for instance.

## What to notice

**1. Footprint is measured, not guessed — and it is measured in three parts.**
`du -sh` on the install path gives you the artifact only: `poppler` is 22 M. What it dragged in sits elsewhere — the dependency tree, plus its own cache directories. Add those up and you get ~241 M. `du` on the path alone will never report that number, and that gap is where the disk actually goes.

**2. "Why it can go" is written for a future you.**
"The report shipped" is a reason. "Not needed" is not a reason — it is the thing you will disagree with in six months.

**3. "Uninstall command" is a command, not a description.**
If you cannot write the exact command, you do not yet know how to remove the thing. Find out now, while the install is ten minutes old.

**4. Deleted rows stay.**
`deleted (2025-12-02)` with the date is the trail proving the review happened and the decision was carried out. A ledger that silently drops rows cannot be audited.

## Review log

```
2025-12-02  Reviewed 7 items. Verified footprint with du against the table —
            two rows were stale (Chromium had grown to 519 M). Deleted:
            QR generator temp deps (9 M, artifact produced). Kept: poppler
            and ffmpeg (still needed). pip cache flagged removable, awaiting
            the user's go-ahead.
```

One line per review. Date, what was checked, what was deleted, what was kept, what is waiting on a decision.
