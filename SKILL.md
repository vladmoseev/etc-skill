---
name: etc
description: "The user fixed two or three things as examples and expects you to infer the rule and apply it everywhere else. Triggers on \"and so on\", \"etc\", \"same for the rest\", \"apply the same principle\", \"do the rest yourself\", \"fix 1 and 2, you get the idea\", \"and everywhere else too\", /etc. The items shown are samples, not a task list: fixing only those and reporting \"done\" counts as a failure here."
---

# etc — a rule, not a list of items

The user fixed a few things and said "carry on the same way."
That means: **the items named are examples you have to derive a rule from.**
Fixing exactly those and stopping is the most common and most maddening failure.

Why it happens: doing what was named is safe and verifiable, while deriving a
rule means taking on interpretation and making edits nobody asked for by name.
The pull toward safe is strong. Resist it — here, safe *is* wrong.

## 1. State the rule out loud, before touching anything

The first thing you do is **write the rule in words**. Not "got it, fixing,"
but something concrete:

> You fixed `X` and `Y`. The rule I derived: **\<one sentence\>**.
> It covers N more places.

Until the rule is stated, it doesn't exist, and there is nothing to sweep with.
Saying it out loud catches a mismatch at the start instead of after twenty edits.

**The rule has to be checkable.** "Make it better" is not a rule.
"All dates use `YYYY-MM-DD`", "no function catches an exception that cannot
happen", "the confirm button always sits to the right of cancel" — those are rules.

If it doesn't collapse into one rule, say so honestly: "I see two different
rules, here they are." Don't force them together.

## 2. Find everything the rule covers

Search the **whole stated scope**, not just where it's convenient. If you're
unsure where the boundary is, ask in one line: "this file or the whole project?"
That's cheaper than sweeping the wrong area.

Show the full list of what you found. Even if it's long — *especially* if it's
long: "I found 23 places" is often the very answer you were called in for.

## 3. Sort the findings into three piles

| Pile | What to do |
|---|---|
| **Clearly covered** | fix it |
| **Looks covered, but differs** | fix it and **name the difference** in one line |
| **Looks similar, but the rule isn't about this** | **leave it alone** and say why |

The third pile matters more than the first. It's what shows you understood the
rule instead of grepping for lookalikes. If the third pile is empty, check
again — you probably swept by shape rather than by meaning.

## 4. Report the rule, not the count

Bad: "fixed 23 files."
Good: "rule: \<sentence\>. It covered 23 places, all fixed. Three left alone:
\<which and why\>. One borderline: \<which\>, handled like this."

## Pitfalls

- **Don't wait for the rule to be confirmed when it's obvious.** State it, give
  the number of places, and go. The user asked for a result, not a sign-off.
  Ask only when you ended up with two rules that contradict each other.
- **Don't let the rule grow along the way.** You started with date formats —
  don't fix indentation while you're in there. That's what separate passes
  are for.
- **Check the items you were shown, too.** Sometimes the user fixed them only
  partway, as a sketch — the rule may be broader than their own example.
- **Same thing when you're writing, not fixing.** Some lists can't be finished:
  there are hundreds of tools, dozens of file formats, any number of error cases.
  Three examples in place of an explanation lie about the coverage — whoever has
  the fourth case concludes it was left out. Explain how to tell what fits from
  what doesn't, and call the examples examples.
- **Zero findings is a result, not a failure.** Say "besides the ones you
  showed, nothing else falls under the rule," and be done.
