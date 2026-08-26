# KikiWiki (聞き書きWiki)

**English | [日本語](README.ja.md)**

A knowledge base that pulls your own hard-won experience out of conversation and stores it in a form you can trace back to its source.

> **Ask it, write it down, keep it traceable.**

This is an **empty starter kit**. It ships with zero real notes — just one sample note showing the expected shape.

> **Want the feel of it first?** [EXAMPLE-CONVERSATION.md](EXAMPLE-CONVERSATION.md) (Japanese) contains the full conversation that produced that sample note. It shows how rough your input is allowed to be faster than any explanation can.

> **Note on language:** the shipped agent instructions, templates, and prompts are written in Japanese, and the sample conversation is in Japanese. The structure itself is language-neutral — translating `wiki-config/` and `.github/prompts/` is enough to run the whole thing in English. See [Running it in English](#running-it-in-english).

## About the name

*Kikigaki* (聞き書き) is the name of a technique from Japanese folklore studies and local history: interview a person, and write the account down in their own words. It points at what feeds this system — **not** external articles, but what a person knows. Those accounts are then consolidated into a structured layer (`wiki/`), hence `聞き書き` + `Wiki` = **KikiWiki**.

## What this is

Capturing know-how is normally heavy work. No time to write it, no way to find it once written, and because nobody can find it, nobody writes it. That's the order in which these systems die.

This one removes the weight by **letting the AI do the formatting and structuring, so the human only has to talk.** You tell the agent what happened, in whatever shape you remember it. The agent drops it into a note that follows the template, tags it, and updates the catalog. The only human job left is checking the facts.

It starts from Andrej Karpathy's [LLM wiki pattern](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f), with three deliberate changes:

| Karpathy's original | KikiWiki |
|---|---|
| The LLM ingests external sources (articles, papers) | **Conversation and interview draw out a person's tacit knowledge** |
| The LLM is the wiki's primary author | **The LLM is the formatter.** `knowledge_origin` / `evidence_level` record provenance and confidence; a human verifies the facts |
| Flat management via index.md / log.md | **Two layers: raw (`notes/`, append-only) and wiki (`wiki/`, the overwritable canonical copy)** |

## What you need

| | Required | Notes |
|---|---|---|
| VSCode | Yes | Hosts the AI agent extension |
| An AI agent (one of the below) | Yes | Claude Code extension or GitHub Copilot Chat |
| Git | Optional | Works fine without it ([see below](#if-git-isnt-available)) |
| Python / Node.js / any runtime | **No** | It is Markdown files, nothing else |

Instruction files for the major tools are included and load automatically when you open the folder.

| Tool | Instruction file loaded automatically |
|---|---|
| Claude Code | `CLAUDE.md` |
| GitHub Copilot Chat | `.github/copilot-instructions.md` |
| Codex and other general agents | `AGENTS.md` |

All three point at the same shared ruleset, `wiki-config/ai-agent-rules.md`. **Change the shared rules and every tool picks it up.**

## Getting started

### 1. Put the files in place

Place the contents of this repository at the root of a new folder, so that `README.md` and `wiki-config/` sit directly inside it. Watch out for `.github` and `.claude` — they are dot-prefixed and easy to miss when copying by hand.

```bash
# A. Use it as your own wiki (don't inherit this repo's history)
git clone --depth 1 https://github.com/ReoT-donzuba/kikiwiki.git my-wiki
cd my-wiki
rm -rf .git
git init && git add -A && git commit -m "init: set up KikiWiki"
```

```bash
# B. You intend to contribute improvements back (keep the history)
git clone https://github.com/ReoT-donzuba/kikiwiki.git
```

**A is the normal path.** Your notes are yours; keep them off this repository's history.

#### If Git isn't available

Git is not required. **Download the ZIP and use the folder as-is.** Adding and searching notes is just the agent reading and writing files, so nothing degrades without Git.

What you lose is change history. Substitute one of these:

- **Use `notes/log.md` as the history.** It exists to record what was added or updated and when. Without Git it becomes your only history, so have the agent append to it every time (the shipped instruction files already tell it to).
- **Copy the folder periodically.** Placed in a synced folder such as OneDrive, its version history can serve as generational backup.

Adopting Git later is fine — run `git init` whenever, and history starts from that point.

### 2. Set up an AI agent

Either one is enough. Installing both and switching between them also works.

#### A. VSCode + Claude Code extension

1. In the Extensions view (`Ctrl+Shift+X`), search for **Claude Code** and install it.
2. Open the panel from the Claude icon in the sidebar and sign in. **A paid Claude plan (Pro / Max / Team) or an API key is required** — check what's available to you before starting.
3. Open the folder from step 1 in VSCode (`File > Open Folder`). The root `CLAUDE.md` loads automatically.
4. Slash commands (`/knowledge-add-interview` and friends) work with no extra setup. Type `/` in the chat to see them.

#### B. GitHub Copilot Chat

1. In the Extensions view, install **GitHub Copilot** and **GitHub Copilot Chat**.
2. Sign in with your GitHub account. **A Copilot license must be assigned to it.**
3. Open the folder from step 1 in VSCode. `.github/copilot-instructions.md` loads automatically.
4. To invoke the prompts in `.github/prompts/` via `/`, **enable the prompt files feature** in VSCode settings. Search settings (`Ctrl+,`) for `prompt files` and turn it on. The exact setting name shifts between VSCode versions.

### 3. Verify the setup

Send this to the chat to confirm the instruction files were read:

```
Summarize this wiki's rules. List the metadata fields a note must carry.
```

If `knowledge_origin` and `evidence_level` come back, the shared rules were reached. If not, check that you opened the right folder root — the folder that *contains* `wiki-config/`, not its parent.

### 4. Write your first note

Take something that tripped you up today and just say it, unpolished:

```
Hit a Docker volume permission error today and fixed it like this. Turn it into a note.
```

When you don't have your thoughts organized enough to narrate, let the agent ask instead:

```
Add knowledge in interview mode
```

Questions come one at a time. Short answers are enough to produce a note.

Once you've written one note of your own, feel free to delete the sample at `notes/20260513-docker-recover-volume-linux.md`.

## Day-to-day use

Start a new session by having the agent read the current state, so you don't re-explain where you left off:

```
Read notes/log.md and notes/index.md and get up to speed on this wiki
```

After that, just say what you want.

| Goal | What to say |
|---|---|
| Add knowledge | `Add knowledge in interview mode` / just narrate what happened |
| Find know-how | `Search for X, but only my own first-hand experience` (filters by origin and confidence) |
| Check health | `Run a lint` (finds contradictions, staleness, orphans, promotion backlog) |
| Clean up prose | `Make this note easier to read` (the `humanize-text` skill) |

The first three are also slash commands: `/knowledge-add-interview`, `/knowledge-search`, `/lint`. The same names work in both Claude Code and Copilot Chat. **When you want the exact same procedure every time, these are more stable than free-form phrasing.**

The procedures themselves live in `.github/prompts/` — that is the source of truth. `.claude/commands/` is a thin entry point that just references it, so edit `.github/prompts/` when changing a prompt.

## Layout

**Write into `notes/`, read out of `wiki/`.**

| Layer | Location | Unit | Updates |
|---|---|---|---|
| raw | `notes/` | one conclusion per note, dated | append-only |
| wiki | `wiki/` | one theme per page (several notes consolidated), undated | overwritten |

```
.
├── README.md                  this file
├── README.ja.md               Japanese version
├── EXAMPLE-CONVERSATION.md    sample conversation (how one note came to be)
├── CLAUDE.md                  instructions for Claude Code
├── AGENTS.md                  instructions for general-purpose agents
├── .github/
│   ├── copilot-instructions.md    instructions for Copilot
│   └── prompts/                   canned prompts (add / search / lint)
├── .claude/
│   ├── commands/                  Claude Code slash commands (reference the prompts above)
│   └── skills/                    Claude Code skills (humanize-text)
├── wiki-config/
│   ├── ai-agent-rules.md          shared rules (every tool points here)
│   ├── TEMPLATE.md                standard note format
│   ├── WIKI-TEMPLATE.md           wiki page format and writing rules
│   ├── tags.md                    controlled tag vocabulary
│   ├── operations.md              operations and quality rules
│   ├── conversation-workflow.md   how a conversation becomes a note
│   └── chat-prompts.md            example chat prompts
├── notes/                     raw layer — write here
│   ├── index.md                   note catalog
│   └── log.md                     change log
└── wiki/                      structured layer — read here
    └── README.md                  reverse index (goal → page)
```

## Operating principles

Five rules do the work of keeping this from going stale:

1. **One conclusion per note.** Narrow each note to a single claim. Long notes go unread and unfound.
2. **Always record origin and confidence.** `knowledge_origin` (first-hand / heard from someone / synthesized from several) and `evidence_level` (verified / unverified) go in the metadata. **Without these you will eventually mistake hearsay for your own experience.**
3. **Write into notes, read out of wiki — never backwards.** Editing the wiki must not rewrite notes. When they disagree, **the note wins** and the wiki gets fixed.
4. **No paragraph without a traceable source.** Every wiki page lists the notes it came from.
5. **Never make the unresolved look resolved.** Questions a note left open belong in the wiki's "open questions" section.

## Growing it

| Note count | What to do |
|---|---|
| under 10 | Run on `notes/` alone. Don't build the wiki layer yet |
| 10 | Run a lint once. Kill orphans and formatting drift |
| 3 on one theme | Write one wiki page for that theme |
| 30 | Revisit the tag vocabulary. Consider splitting `notes/index.md` into categories |
| monthly | Run a lint. Have the agent propose improvements to the templates and rules themselves |

Templates and rules capture "the best we know right now" — they are not fixed. Update `wiki-config/` when you notice something. Lint pulls older notes toward the new shape gradually; you don't have to revise them all at once.

## Running it in English

Nothing in the structure depends on Japanese. To run it fully in English, translate these and you're done:

1. `wiki-config/*.md` — the shared rules and templates (the bulk of it)
2. `.github/prompts/*.prompt.md` — the three canned procedures
3. `CLAUDE.md` / `AGENTS.md` / `.github/copilot-instructions.md` — thin pointers to the above
4. `notes/index.md`, `notes/log.md`, `wiki/README.md` — the scaffolding headers

The metadata field names (`knowledge_origin`, `evidence_level`, `status`, `portable`) are already ASCII; keep them as they are so the prompts keep matching. In practice you can ask the agent to do this translation pass for you in one go.

## Cautions

- **Decide up front how you handle client-specific material.** Notes containing customer names or bespoke design decisions can't leave the organization. `wiki-config/WIKI-TEMPLATE.md` uses `portable: yes | no` to make that explicit per page.
- **Don't put your own wiki in a public repository.** Notes accumulate business judgment and unverified guesses. If you track it in Git, use a private repo.
- **On Windows + PowerShell 5.1**, passing `-Encoding UTF8` explicitly is mandatory to avoid mangled non-ASCII text. The rule is recorded in `wiki-config/ai-agent-rules.md`.
- Note deletion and demotion to `deprecated` by the agent require confirmation (see "破壊的操作の制限" in `wiki-config/ai-agent-rules.md`).

## Questions and improvements

Bug reports and proposals for the rules or templates are welcome in [Issues](https://github.com/ReoT-donzuba/kikiwiki/issues) — impressions from actually using it too. See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

## Author

Created by [ReoT-donzuba](https://github.com/ReoT-donzuba), building on Karpathy's LLM wiki pattern.

## License

[MIT](LICENSE). Fork it and reshape it into your own organization's template freely.
