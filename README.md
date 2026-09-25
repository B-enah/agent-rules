# agent-rules

This is where I keep the rules my AI coding agents (Claude Code, Cursor,
etc.) should follow across all my projects — my tech stack, conventions,
security rules, and things they should never do without asking first.

Instead of copy-pasting these rules into every project, each project just
connects to this repo. Update the rules once, here, and pull the update
into your projects.

## What's in here

- **`AGENTS.md`** — the actual rules. Stack, conventions, security,
  boundaries, and style.
- **`USAGE.md`** — full step-by-step guide, including how to make sure
  each AI tool actually reads the file once it's connected.

## Connecting this repo to a project

Pick one:

**Copy it in (simplest)**
Download `AGENTS.md` and drop it into your project's root folder. You'll
need to re-copy it by hand whenever the rules change.

**Git submodule (best if you have several projects)**
```bash
git submodule add https://github.com/<you>/agent-rules.git .agent-rules
```
Pull future updates with:
```bash
git submodule update --remote --merge
```

**Symlink (if the project sits next to this repo locally)**
```bash
ln -s ../agent-rules/AGENTS.md AGENTS.md
```

Once it's connected, head to `USAGE.md` for the part that trips people up —
getting each specific tool (Claude Code needs a small extra step) to
actually pick the file up.