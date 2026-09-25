# Signaled Gear

Personal notes on working with coding agents, collected from guides and public
talks in January 2026. This is a place to keep ideas worth
testing. It is not a framework, a course, or a maintained set of integrations.

**Reviewed September 25, 2026.** I have cut the "ultimate guide" framing,
unsupported savings figures, and model rankings. The earlier draft made stronger
claims than the evidence in this repo could support.

## What I would keep

Give the agent a clear outcome, the files it may change, and a way to check the
result. Let the task determine how much process it needs. A small repair does not
need a team of agents or a new orchestration layer.

| Idea | Where I would try it | What I would check |
|---|---|---|
| Explicit repository instructions | Repeated work with conventions that are easy to miss | Whether the instructions prevent actual mistakes; remove stale or conflicting rules |
| Search before implementation | A codebase with existing utilities and prior decisions | Whether the retrieved material is relevant and still describes the code |
| Small implementation steps | Changes with a clear behavioral boundary | A check that fails on the defect and passes after the fix |
| A short checkpoint | Work that continues across sessions | Whether another session can resume from the current files and recorded next step |
| Reusable skills | A procedure that has been useful more than once | Whether it saves review effort without adding unnecessary work |
| Parallel agents | Independent tasks with separate outputs or files | Coordination cost, conflicting edits, and review quality |

These are working preferences and hypotheses. This repository contains no
benchmark proving that they improve every task.

## A small workflow

1. Read the relevant code and current instructions. Search for an existing solution.
2. State the intended behavior and the evidence that would show it works.
3. Make a bounded change. Preserve work owned by another person or session.
4. Run the affected checks. For a visible change, inspect the running page.
5. Review the diff and explain the outcome, remaining limits, and next step.

Use a rule or script when the decision is mechanical. Use a model when the work
needs interpretation. More prompts, context, or agents can increase cost without
improving the result. Compare the full cost per accepted result, including review
and retries, before calling a workflow more efficient.

## Context and memory

Keep the source files and test results authoritative. A summary can omit a
constraint; a search can miss a useful result. Neither guarantees lossless recall.

A useful checkpoint can be small:

```text
Outcome wanted:
Current state and relevant files:
Checks run and their results:
Unresolved issue:
Next action:
```

Keep private checkpoints private. Do not capture every transcript by default or
send a personal corpus to a cloud service as a convenience. Choose what to retain,
where it may go, who may read it, and when it should be removed. Public examples
should use synthetic data. Scan and review the exact files, metadata, and commit
history before publishing; a pattern scan alone cannot establish privacy.

## Tools mentioned in the original notes

These links identify projects, not evidence for the earlier draft's claims:

- [QMD](https://github.com/tobi/qmd) is a local document-search project. Use its
  current documentation for commands and setup; this repo does not maintain a QMD integration.
- [The historical PAI repository](https://github.com/danielmiessler/Personal_AI_Infrastructure)
  now redirects to Daniel Miessler's LifeOS project. The PAI labels in the old
  notes describe the material I was studying then, not a verified current API.
- [Claude Code documentation](https://code.claude.com/docs/en/overview) is the
  place to check its supported configuration and behavior. Old shortcuts and
  hook examples here should not be treated as current product documentation.

The earlier draft also summarized talks under the labels "Tactical Agentic
Coding" and "Claude Code Deep Mastery." The repository does not contain a
complete list of source URLs and timestamps. I cannot establish quotation-level
attribution from those notes alone, so this revision does not repeat the quotes
or treat the summaries as verified findings.

The [original draft](https://github.com/rustybladerunner/signaled-gear/blob/deb1f0229db86f27d9e0f0ffb915be61521f1ece/README.md)
remains in Git history for context. It contains untested sketches, old model
preferences, and unsupported claims. It is not an implementation guide.

## Claims I would not make

- No fixed token-saving percentage follows from using templates, skills, or retrieval.
- A longer-running agent is not necessarily more capable or more reliable.
- A prompt cannot guarantee safety, privacy, or permission enforcement.
- A security hook needs tested code and enforcement; a diagram is not evidence.
- Model rankings need a dated task set, baseline, and evaluation method.

For an experiment, record the task set, baseline, environment, acceptance rule,
failures, review time, and total cost. Keep unsuccessful results. If those records
are missing, describe the idea as untested.

## Related notes

[React workflow notes](other_suggestions.md) contains a few ideas to test in an
existing frontend. It is not a migration recommendation.
