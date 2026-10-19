---
name: install-ledger
description: Software install ledger. Before an agent installs anything on the machine (pip / brew / npm / a downloaded binary / playwright install / a skill dependency), it must first pass a four-question gate and immediately register the install in a ledger afterwards. The user can request a review at any time; the agent reports each item's real footprint and a keep-or-remove recommendation, and deletes nothing until the user confirms. Load when an agent is about to run any install, or when the user says "review installs", "install ledger", "clean up disk", "what's installed", or "install X".
---

# Install Ledger

A chain for the "my agent installs things on my machine" problem:
**gate before → ledger after → review periodically.**

Most tooling covers one link. This covers the chain, and it stops to ask a human before deleting.

## Purpose

Record every install an agent makes on the machine, so the disk does not quietly fill up. Every item must stay traceable — what it is, why it was installed, where it lives, how big it is, how to remove it — and support a periodic review that ends in thorough cleanup.

## Triggers

- **Before an install**: the agent is about to run any install (`pip install` / `brew install` / `npm install` / downloading a binary / `playwright install` / installing a dependency for a skill). Read this rule first; register the ledger entry the moment the install finishes.
- **Review**: the user says "review installs" / "install ledger" / "clean up disk" / "what's installed" — run the review flow.

## The gate (four questions, before installing)

Ask all four. If any answer is "no", do not install — find another way.

1. **Do we actually need it?** Is there a way to finish the job without installing anything (an existing tool, an online service)?
2. **Is there a lighter substitute?** A user-level or temporary install beats a system-level one; a small package beats a large one.
3. **Can it be removed later?** For temporary use, prefer user-level installs (`pip install --user`, cache directories) so a thorough cleanup stays possible.
4. **Is it registered?** Write the ledger entry as soon as it is installed. Do not defer.

## Ledger fields (one row per install)

| Field | Meaning |
|---|---|
| Name | What was installed |
| What it does | One sentence, in plain language |
| Installed on | YYYY-MM-DD |
| Why | Which project or task needed it |
| Project | The related project or task — any link form is fine |
| Location | Absolute path |
| Footprint | Real usage including dependencies and caches (measure with `du`) |
| Lifetime | temporary / phased / long-term |
| Why it can go | Why it is safe to delete now (filled in at review time) |
| Uninstall command | How to remove it thoroughly |
| Status | in use / removable / deleted (+ date) |

**Where the ledger lives is your choice** — one Markdown file, a spreadsheet, an issue tracker, a database. This rule governs the fields and the flow, not the storage. See `LEDGER.example.md` for a synthetic example.

## Review and thorough deletion

1. **Scan**: read the ledger, and verify each item's *actual* footprint with `du` — the ledger is usually stale.
2. **Report**: for each item — what it does, how much it takes, whether its project is finished, keep or remove, and why.
3. **Ask the human**: the user decides what gets deleted. Nothing is deleted before that.
4. **Delete thoroughly** (only after confirmation):
   - Delete the **installed artifact plus the working files it generated** (caches, temp files, intermediate data).
   - **Keep the deliverables** — the final outputs: reports, charts, recordings, pages, finished data sets. Never delete those.
   - Send it to the **system trash** (`trash`, the macOS Trash, or the platform equivalent) rather than `rm`. Empty it only after the user agrees.
   - Update the status to "deleted + date", and leave a line in the review log.

## Trail

Every install, review, and deletion leaves a node in the ledger and, when it matters, in the log for that day.
