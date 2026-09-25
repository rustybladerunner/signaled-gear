# React workflow notes

Personal notes from early 2026, reviewed September 2026. These are ideas to test
when an existing frontend is hard to work on. There is no benchmark here showing
that a particular framework makes coding agents more effective.

## Start with the problem

If changes keep breaking prop contracts, make the types clearer. If a component
is difficult to test, find a useful behavioral boundary before splitting it.
If visual edits drift, use the existing design tokens and compare the running
page with the intended result.

I would not replace a working styling system just because an agent seems to
prefer another one. Tailwind, CSS modules, and plain CSS can all express a design.
Choose based on the project and the people maintaining it.

## Changes worth trying

| Problem | Small experiment | Evidence to collect |
|---|---|---|
| Props are misunderstood | Add or clarify types at the affected boundary | Fewer contract errors in the next comparable changes |
| Related code is hard to find | Keep the affected component, styles, and tests close where practical | Less searching without creating new import or ownership problems |
| Instructions are repeatedly missed | Record the few relevant conventions | Whether later changes follow them without extra reminders |
| A visual repair looks plausible but is wrong | Inspect the actual route and compare screenshots | Correct layout and behavior at the relevant viewport sizes |
| A refactor is difficult to review | Split it into independently checkable steps | Smaller diffs that preserve behavior |

Keep the experiments bounded. Preserve accessibility, keyboard interaction,
responsive behavior, and the existing style. Component libraries still need
application-level checks; they do not establish accessibility by themselves.

## A useful request

```text
Fix [observable problem] in [relevant component or route].
Preserve [important existing behavior].
Use the current project conventions.
Show how the change was checked and what remains uncertain.
```

Use synthetic content in shared examples and screenshots. Do not publish customer
records, private URLs, credentials, or local environment details with a bug report.

These notes describe possible approaches, not an instruction to migrate a codebase
or a claim that one tool is best for every task.
