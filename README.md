# Agent Install Ledger

![Skill: Agent](https://img.shields.io/badge/Skill-Agent-111111?style=flat-square)
![License: MIT](https://img.shields.io/badge/License-MIT-2457F5?style=flat-square)
![Works with: Claude Code · Codex · any Skills harness](https://img.shields.io/badge/Works%20with-Claude%20Code%20%C2%B7%20Codex%20%C2%B7%20any%20Skills%20harness-6B5B95?style=flat-square)

**Your agent installs things faster than you can keep track. A year later, nobody can say what any of it is for.**

**Make it keep the receipts.** A three-stage chain: **gate before → ledger after → review periodically.**

**[中文版 →](README.zh-CN.md)**

---

## The problem

An AI agent can install things at any moment: `pip install`, `brew install`, `npm install`, a downloaded binary, `playwright install`. Each install is *just one thing*. A year later your disk holds a pile of software you cannot explain:

- A 700 MB runtime installed for a one-off task, still there
- Installed, used, forgotten, never removed
- You want to delete it but you are not sure what depends on it

The problem was never the installing. It is that **nobody kept the receipts.**

## It covers one column

Cleanup tools remove files; dependency tools produce inventories; security scanners check packages. **What none of them keeps is what a thing is, why it is here, and whether it can go** — or when "think before you install" and "look again periodically" should happen.

That chain is this repo.

---

## Install

### One line

```bash
npx skills add https://github.com/spiritauren/agent-install-ledger --skill agent-install-ledger
```

### Or paste this to your agent

```
Install the skill agent-install-ledger. Clone
https://github.com/spiritauren/agent-install-ledger into
~/.claude/skills/agent-install-ledger, then confirm that SKILL.md and
LEDGER.example.md are both present, and tell me the current commit.
```

### Or manually

```bash
git clone https://github.com/spiritauren/agent-install-ledger.git \
  ~/.claude/skills/agent-install-ledger
```

No Skills support? Paste `SKILL.md` into your agent's system prompt, or into a project rule file (`AGENTS.md`, `CLAUDE.md`, `.cursorrules`).

**Storage is your choice.** The ledger can be a Markdown file, a spreadsheet, an issue tracker, a database. This rule governs the fields and the flow, not where you keep them.

---

## Stage 1 — The gate

Before installing, the agent must answer three questions. Any "no" means: do not install, find another way.

1. **Do we actually need it?**
2. **Is there a lighter substitute?** (user-level beats system-level; small beats large)
3. **Can you write its uninstall command right now?** No command means you do not yet know what you are installing.

Then, the moment it lands: **write the row.** The gate decides whether to install; the row is what proves the install is over.

## Stage 2 — The ledger

One row per install. Eleven fields.

| Field | Meaning |
|---|---|
| Name | What was installed |
| What it does | One sentence, in plain language |
| Installed on | YYYY-MM-DD |
| Why | Which project or task needed it |
| Project | The related project or task |
| Location | Absolute path |
| Footprint | Disk it actually occupies: `du -sh` on the artifact **plus** the dependencies it pulled in and its own cache directories |
| Lifetime | temporary / phased / long-term |
| Why it can go | Why it is safe to delete now (filled at review time) |
| Uninstall command | How to remove it thoroughly |
| Status | in use / removable / deleted (+ date) |

Note the two fields people leave out — **"Why it can go"** and **"Uninstall command"** — are the two that make cleanup possible a year later. A ledger without them is a list, not a ledger.

## Stage 3 — The review

1. **Scan** — read the ledger and re-measure every item with `du`. The ledger is always stale.
2. **Report** — per item: what it does, how much it takes, whether its project is finished, keep or remove, and why.
3. **Ask** — the human decides. Nothing is deleted before that.
4. **Delete thoroughly** — the artifact *plus* the working files it generated (caches, temp data). **Keep the deliverables.** Send it to the system trash, not `rm` — `trash` on macOS 15+, `gio trash` or your platform's equivalent elsewhere.

### The line that matters

> **Delete the working files. Keep the deliverables.**

A cache directory is garbage. The report that cache helped produce is not. Getting this line wrong once — deleting a month of work along with the tool that made it — is how people learn to never clean up again.

Two buckets are obvious. The third is where people get hurt:

- **Deliverables** — keep.
- **Caches and temp files** — delete.
- **Everything else** — the script that produced the deliverable, a tool's own data directory, the source CSV behind a chart. **List these out and confirm each one.** The test is not "is it a final output" but **"can it be regenerated — and is this the only copy?"**

---

## The five lines this draws

Three of these are this repo's own. Two are long-standing practice — they are here because a rule is what makes them actually happen on every install, rather than when someone remembers.

1. **Ask before, not after.** The gate runs *before* the install, so the answer can still be "no".
2. **Measure, don't estimate.** `du` on the real thing — and remember `du` reports the artifact only. Dependencies and caches are counted separately, on purpose. Vendor download sizes lie; your own sum does not.
3. **Write the reason while you still remember it.** "Why it can go" is a note to a future you who will disagree with today's "not needed".
4. **Delete working files, keep deliverables.** The one mistake in this whole document that emptying the trash turns irreversible.
5. **Stop and ask the human before deleting.** Automating the discovery is fine. Automating the deletion is not.

## Use it if / don't use it if

**✅ Use it if** you run an agent with shell access on your own machine, and you have caught yourself wondering what half of your installed software is for.

**❌ Don't use it if** you install software deliberately, rarely, and know what everything is. You already have this ledger — it's in your head.

**Not covered:** reproducing an environment (`brew bundle dump`, `pip freeze`). Different job, well-solved — a ledger is not a lockfile.

---

## FAQ

**Haven't I got 200 things installed already? Do I backfill?**
No. Start the ledger at your next install. Everything before it is exactly what a review is for — and the review does not need a complete ledger to be useful.

**What stops the agent from just ignoring this?**
Nothing. It is a rule, not a sandbox. It makes the right behaviour the path of least resistance. Pair it with whatever enforcement your harness gives you.

**Where do I actually put the ledger?**
Wherever you already look. Markdown file, spreadsheet, an issue, a database. The fields matter; the storage does not. `LEDGER.example.md` is a synthetic example.

**Is keeping something forever a failure?**
No. "Keep" is a legitimate outcome — the review exists so that keeping is a *decision* rather than an oversight. The failure mode isn't owning things. It's not knowing.

**What if the agent installs something at 3am unattended?**
It still writes the row. The ledger is most valuable for exactly those installs — the ones nobody was watching.

---

## Files

| File | What it is |
|---|---|
| `SKILL.md` | The rule, for the agent to read (English) |
| `SKILL.zh-CN.md` | Same rule, Chinese version |
| `LEDGER.example.md` | A synthetic ledger example (English) |
| `LEDGER.example.zh-CN.md` | Synthetic ledger example, Chinese version |
| `README.md` / `README.zh-CN.md` | This page, in both languages |

## A caution

This is a rule for an agent, so it only works if the agent actually reads it. That means it has to live where the agent already looks — a skills directory, a system prompt, a project rule file. A rule nobody loads is a document, not a mechanism.

Same goes for the review. If nobody ever says "review installs", stage 3 never runs and you are back to a pile. **The chain is only as good as its weakest trigger.**

## License

MIT — see [LICENSE](LICENSE).
