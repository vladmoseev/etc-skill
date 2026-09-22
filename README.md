# etc — an agent skill

> "Fix items 1 and 2, then do the rest the same way."

You show a coding agent two or three corrections as examples. It fixes exactly
those two or three, reports "done," and hands back a file that still has twenty
instances of the thing you just complained about.

This skill fixes that. It turns "and so on" into a working instruction.

It ships as a `SKILL.md` because that's a format some agents load by
themselves, but nothing in it is tied to one of them: it's a page of plain
Markdown describing a behaviour, and it works in any agent you can give
standing instructions to. See [Install](#install).

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

On any phrase that hands over a pattern instead of a list — you name a couple
of cases and make the agent responsible for the rest. Some that do it:

`and so on` · `etc` · `same for the rest` · `apply the same principle`
`do the rest yourself` · `fix 1 and 2, you get the idea` · `and everywhere else too`

Those are examples, not a closed set, and an agent that only pattern-matches the
literal strings has already missed the point. Invoking it as `/etc` works too
where your agent supports that.

## Install

The skill is one page of Markdown with no code, no dependencies and no tool
calls. Installing it means putting that page where your agent reads standing
instructions from. Every agent has such a place; only the path and the loading
rule differ.

Two questions get you there.

**1. Does your agent load instruction files on demand, or always?**

- **On demand** — it scans a directory of instruction files and pulls one in
  when its description matches what you're doing. **Keep the YAML front matter:**
  the `description` field is what makes the loader fire on "and so on."
- **Always** — it reads one file (or a whole directory) into context at startup.
  **Drop the front matter.** When the text is always loaded, the trigger list is
  dead weight; the four steps are the part that works.

**2. Where does it read from?** Check your agent's own docs for the path. Three
shapes cover almost everything:

| Shape | Path looks like | Front matter |
|---|---|---|
| Skill / command directory, loaded on demand | `<config dir>/skills/<name>/SKILL.md` | keep |
| One rules file, always loaded | `AGENTS.md` in the repo or home config | drop |
| Rules directory, always or conditionally loaded | `<rules dir>/etc.md` | keep if the agent parses it, else drop |

If your agent takes no file at all — a web UI, an API system prompt, a "custom
instructions" box — paste the body in there. Same text, same effect.

`AGENTS.md` is the closest thing to a common denominator: a growing number of
agents read it, and an agent that documents no path of its own is worth trying
it on first.

### Two worked examples

A skill directory that loads on demand, Claude Code being one such agent:

```bash
git clone https://github.com/vladmoseev/etc-skill.git ~/.claude/skills/etc
```

A single always-loaded rules file, front matter stripped:

```bash
curl -fsSL https://raw.githubusercontent.com/vladmoseev/etc-skill/main/SKILL.md \
  | sed '1,/^---$/d' >> AGENTS.md
```

Swap the path for whatever your agent uses; nothing else changes. Then check the
install the only way that means anything: make two corrections by hand, say
"the rest the same way," and see whether you get a rule back or two fixed lines.

## Other languages

The skill is one Markdown file with no code, so a translation is a full port.

- **English** — [`SKILL.md`](SKILL.md) (default)
- **Russian** — [`translations/ru/SKILL.md`](translations/ru/SKILL.md)

Triggers are language-specific: an agent won't recognise `и так далее` from the
English file. Install the version matching the language you actually write in —
the procedure above is identical, just take that file instead of `SKILL.md`.

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
