# How to Use AGENTS.md

This file (`AGENTS.md`) holds all your coding rules — your stack, conventions,
and things your AI agent should never do. Instead of copying it into every
project, you keep one copy here and connect each project to it.

## Step 1: Get the file into your project

Choose whichever feels easiest:

**Option A — Copy it in (simplest, no extra tools)**
Just download `AGENTS.md` and drop it in your project's root folder.
Downside: if you update the rules later, you have to copy it into every
project again by hand.

**Option B — Git submodule (best if you have 3+ projects)**
This links your project to this repo, so you can pull updates instead of
re-copying.
```bash
git submodule add https://github.com/<you>/agent-rules.git .agent-rules
```
When rules change later, run this in each project to get the update:
```bash
git submodule update --remote --merge
```

**Option C — Symlink (if projects live in the same folder as this repo)**
```bash
ln -s ../agent-rules/AGENTS.md AGENTS.md
```

## Step 2: Make each AI tool actually read it

This is the part that trips people up — not every tool reads `AGENTS.md`
by the same name.

**If you use Cursor:**
Nothing to do. Cursor reads `AGENTS.md` automatically.

**If you use Claude Code:**
Claude Code looks for a file called `CLAUDE.md`, not `AGENTS.md`. So you
need to create a `CLAUDE.md` that points to it. Easiest way:
```bash
ln -s AGENTS.md CLAUDE.md
```
On Windows (symlinks need admin rights there), just create `CLAUDE.md` and
put this single line inside it instead: