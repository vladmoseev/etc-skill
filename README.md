# etc — an agent skill

> "Fix items 1 and 2, then do the rest the same way."

You show a coding agent two or three corrections as examples. It fixes exactly
those two or three, reports "done," and hands back a file that still has twenty
instances of the thing you just complained about.

This skill fixes that. It turns "and so on" into a working instruction.

It ships as a `SKILL.md` for Claude Code, but nothing in it is Claude-specific —
it's a page of plain Markdown describing a behaviour, and it works in any agent
that reads instructions from a file. See [Install](#install).

## What it does

When the skill fires, the agent stops treating your examples as a task list and
starts treating them as evidence of a rule. Four steps:

1. **State the rule in words before editing anything.** "You fixed `X` and `Y`.
   The rule I derived: *dates use `YYYY-MM-DD`*. It covers 23 more places."
   A rule that isn't stated doesn't exist — and a stated rule lets you catch a
   misreading in one line instead of after twenty wrong edits.
2. **Search the whole stated scope**, not just the convenient part, and show the
   full list of hits.
3. **Sort the hits into three piles** — clearly covered, covered-but-different
   (fix and name the difference), and looks-similar-but-isn't (leave alone and
   say why). The third pile is the important one: it's the proof the agent
   understood the rule instead of grepping for lookalikes.
4. **Report the rule, not the count.** "Fixed 23 files" tells you nothing.
   "Rule: X. 23 places, all fixed. Three left alone because Y" tells you whether
   to trust the pass.

It also covers the failure modes around the edges: don't stall waiting for the
obvious rule to be confirmed, don't let the rule grow into unrelated cleanup,
re-check the examples you were shown (people often fix them only partway), and
treat zero findings as a legitimate answer.

## When it fires

On phrases that hand over a pattern instead of a list:

`and so on` · `etc` · `same for the rest` · `apply the same principle`
`do the rest yourself` · `fix 1 and 2, you get the idea` · `and everywhere else too`

Or explicitly, as `/etc`.

## Install

### Claude Code

Skills live in `~/.claude/skills/<name>/SKILL.md`:

```bash
git clone https://github.com/vladmoseev/etc-skill.git ~/.claude/skills/etc
```

Or without git — grab the one file:

```bash
mkdir -p ~/.claude/skills/etc && curl -fsSL \
  https://raw.githubusercontent.com/vladmoseev/etc-skill/main/SKILL.md \
  -o ~/.claude/skills/etc/SKILL.md
```

For one project only, use `.claude/skills/etc/` inside the project instead.
Start a new session and check it's there:

```bash
test -f ~/.claude/skills/etc/SKILL.md && echo installed
```

### Codex, Gemini CLI, and other agents that read a rules file

These don't have a skill loader — they read one instructions file at startup
(`AGENTS.md`, `GEMINI.md`, or whatever the agent calls it). Append the body of
`SKILL.md`, without the YAML front matter, to that file:

```bash
curl -fsSL https://raw.githubusercontent.com/vladmoseev/etc-skill/main/SKILL.md \
  | sed '1,/^---$/d' >> ~/.codex/AGENTS.md
```

The front matter only exists to tell a skill loader when to activate. In a
rules file the text is always loaded, so the trigger list is redundant — the
four steps are the part that does the work.

### Cursor, Windsurf, and similar

Drop `SKILL.md` into the project's rules directory (`.cursor/rules/etc.md` and
its equivalents). Keep or drop the front matter depending on whether that editor
uses it.

## Other languages

The skill is one Markdown file with no code, so a translation is a full port.

- **English** — [`SKILL.md`](SKILL.md) (default)
- **Russian** — [`translations/ru/SKILL.md`](translations/ru/SKILL.md)

Triggers are language-specific: an agent won't recognise `и так далее` from the
English file. Install the version matching the language you actually write in —
same path, just copy that file in place of `SKILL.md`.

Pull requests with more translations are welcome. Keep the four-step structure
and translate the trigger phrases into ones people really say in that language,
rather than word-for-word.

## Why it's a skill and not a line in a config

Because the pull toward the wrong behaviour is strong. Doing what was named is
safe and verifiable; deriving a rule means taking on interpretation and making
edits nobody asked for by name. An agent optimising for "did I do what I was
told" will pick the narrow reading every time. Countering that takes a few
paragraphs of argument and a concrete procedure — not a reminder.

## License

MIT — see [LICENSE](LICENSE).
