> [!NOTE]
> **This skill now lives in the main collection: [milad-ahmd/agent-skills](https://github.com/milad-ahmd/agent-skills/tree/main/skills/ai-slop-detector)**. Please ⭐ star it there; that's where updates land first.
> Docs: **https://milad-ahmd.github.io/agent-skills/skills/ai-slop-detector/** · Claude Code: `/plugin install all-skills --marketplace milad-ahmd/agent-skills`

<div align="center">

# 🧹 ai-slop-detector

**Score text for AI-sounding writing and rewrite it to sound human, keeping the author's voice. An Agent Skill for Claude Code, Cline, Cursor, Codex and other AI coding agents.**

[![License: MIT](https://img.shields.io/github/license/milad-ahmd/ai-slop-detector)](LICENSE)
[![Stars](https://img.shields.io/github/stars/milad-ahmd/ai-slop-detector?style=social)](https://github.com/milad-ahmd/ai-slop-detector)
![Category](https://img.shields.io/badge/category-Writing-blue)
![Works with](https://img.shields.io/badge/works%20with-Claude%20Code%20%7C%20Cline%20%7C%20Cursor%20%7C%20Codex-8A2BE2)

[Install](#-install) · [Usage](#-usage) · [Use in CI](#-use-it-in-ci) · [How it works](skills/ai-slop-detector/SKILL.md) · [All skills](https://github.com/milad-ahmd/agent-skills)

<img src="assets/demo.gif" alt="ai-slop-detector demo (real output)" width="760"/>

</div>

> Part of **[agent-skills](https://github.com/milad-ahmd/agent-skills)**, a growing collection of practical skills for AI coding agents.

## 📦 Install

**One command (installs the whole collection):**

```bash
curl -fsSL https://raw.githubusercontent.com/milad-ahmd/agent-skills/main/install.sh | bash -s -- ai-slop-detector
```

**Manually (Claude Code):**

```bash
git clone https://github.com/milad-ahmd/ai-slop-detector /tmp/ai-slop-detector
mkdir -p ~/.claude/skills && cp -r /tmp/ai-slop-detector/skills/ai-slop-detector ~/.claude/skills/
```

**Per project:** copy `skills/ai-slop-detector` into `<project>/.claude/skills/`.
**Any other agent:** paste [`SKILL.md`](skills/ai-slop-detector/SKILL.md) into your rules or system prompt.

## 💬 Usage

Just ask your agent:

```text
De-slop this blog post
Does this sound like AI? Make it sound human
```

The full workflow, rules and output format are in [`SKILL.md`](skills/ai-slop-detector/SKILL.md).

Or run the scanner yourself (Python 3.10+, no dependencies):

```bash
python3 skills/ai-slop-detector/scripts/slop_scan.py draft.md
python3 skills/ai-slop-detector/scripts/slop_scan.py 'docs/**/*.md' --json
pbpaste | python3 skills/ai-slop-detector/scripts/slop_scan.py -
```

## ✅ Use it in CI

This repo is also a GitHub Action. It annotates pull requests with AI-sounding patterns (line, pattern, and a plainer alternative) and writes a score table to the job summary.

```yaml
# .github/workflows/slop.yml
name: Slop check
on:
  pull_request:
permissions:
  contents: read
jobs:
  slop:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: milad-ahmd/ai-slop-detector@v1
        with:
          files: "README.md docs/**/*.md"
          exclude: "CHANGELOG.md docs/archive/**"
          pr-body: true
```

| input | default | what it does |
|---|---|---|
| `files` | `**/*.md` | Files, directories or globs, separated by spaces or newlines. The scanner expands globs; `**` is recursive. |
| `exclude` | | Globs to skip. `node_modules`, `.git`, `vendor` and `.venv` are always skipped. |
| `pr-body` | `false` | Also scan the pull request description. It is passed to the scanner through an environment variable and stdin, never interpolated into the shell. |
| `fail-above` | empty | Fail the step when any text scores above this number (0-100). Empty means annotate only. |
| `min-words` | `50` | Skip texts shorter than this. |
| `max-annotations` | `10` | Inline warnings to emit (GitHub shows at most 10 per step). |

The score is 0-100: below 20 is low, 20-29 moderate, 30 and above high. Code blocks, inline code and URLs are ignored. It needs `python3` on the runner, which GitHub-hosted Ubuntu and macOS runners have.

Start with annotate-only. If you turn on `fail-above`, treat it as a nudge for docs, not a gate on people: the scanner measures **style signals, not authorship**, and formal human writing can score high while edited AI text can score low. Benchmark numbers and their caveats are in [`bench/README.md`](skills/ai-slop-detector/bench/README.md).

## 🧩 More skills

See the full collection: **[milad-ahmd/agent-skills](https://github.com/milad-ahmd/agent-skills)**.

## 🤝 Contributing

Issues and PRs are welcome, especially real-world before/after examples.

## 📄 License

[MIT](LICENSE) © Milad Ahmadi

<div align="center">

**If this skill saved you time, a ⭐ helps others find it.**

</div>
