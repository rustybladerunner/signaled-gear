# AI-Friendly React Front-End Suggestions

**Note:** These are **suggestions** and observations based on patterns that tend to work reasonably well with current AI coding assistants (Cursor, Claude Code, Windsurf, etc.) in early 2026.  
They are **not** a required plan, roadmap, migration guide, or todo list.  
Feel free to ignore everything, cherry-pick one or two ideas, or use this purely as background reading when prompting the AI.

## 1. Quick Wins That Sometimes Make AI Assistants More Effective

- **TypeScript expansion / stronger typing**  
  More typed props, state, hooks, context, etc. → models hallucinate less about shapes and APIs.  
  Even partial typing on frequently changed components can help noticeably.

- **Smaller, more focused components**  
  Breaking large "god" components into smaller ones with clear, explicit props often makes generation & refactoring easier.

- **Descriptive prop names + interfaces/types**  
  Reduces ambiguity when the AI is reading or writing usage.

- **Loose living documentation**  
  A casual file like `AI_GUIDE.md`, `PROJECT_NOTES.md` or even comments at the top of key files containing:  
  - Current naming / folder conventions  
  - State management preference (Redux / Zustand / Context / etc.)  
  - Important "do not touch" areas or gotchas  
  - Any strong stylistic rules you care about  
  You can paste or @-reference relevant parts into prompts.

## 2. Styling Approaches That Often Feel Easier for AI (Biggest Observed Difference)

Utility-first + design-token patterns tend to produce more coherent, editable output than deeply nested CSS, heavy CSS-in-JS, or selector-heavy modules — mostly because of locality and predictability.

Some approaches that show up frequently in successful AI-assisted React work:

- **Tailwind CSS** (or similar atomic/utility systems)  
  - Styles live right in the JSX → full visual intent visible in one place  
  - Classes are short, composable, token-like strings → strong pattern recall  
  - Incremental addition tends to be low-conflict

- **shadcn/ui, Radix UI, Ark UI + utility classes**  
  Headless primitives keep behavior/a11y separate from visuals  
  Lets the AI focus mostly on layout & tokens

- **Semantic design tokens (CSS variables)**  
  `--primary-600`, `--radius-md`, `--space-5`, etc.  
  Higher-level names are easier to reason about than raw pixels or rem values

**Very optional incremental thoughts (only if/when it feels right):**
- Keep whatever styling system you have today fully working  
- Try Tailwind just in new components or high-interaction areas first  
- Convert cards/forms/tables/dashboards gradually if you want  
- Let the AI help rewrite pieces when you're experimenting

## 3. Code & Folder Patterns That Sometimes Help

- Colocation (component + styles/tests/types close together — or even in same file)  
- File-based routing (especially Next.js App Router) — very natural mental model for AI  
- Clear component boundaries with typed props  
- Preference for hooks + functional components (class components are usually harder now)  
- Flatter CSS when you must write custom styles (avoid deep nesting / crazy specificity)  
- Modern helpers like container queries, `:has()`, subgrid, etc. when they fit naturally

## 4. Prompting / Workflow Observations When Using Cursor or Similar

- Start bigger changes with "Plan first" / Plan Mode  
- Ask to show diffs or explain reasoning before accepting  
- Explicitly reference files: `@file src/components/Card.tsx`  
- Add guardrail phrases like:  
  - "Respect existing patterns / naming / state strategy"  
  - "Avoid new dependencies unless really necessary"  
  - "Preserve dark mode / a11y / existing behavior"  
- For visual/layout bugs: run dev server → screenshot → attach to prompt  
- Break large refactors into tiny, sequential asks

## 5. Realistic Caveats (What Still Tends to Be Hard)

- Deep legacy integrations, subtle CSS order/overflow/z-index bugs, heavy third-party widgets  
- Very old patterns (old class components + mixin hell + global CSS wars)  
- Business/domain rules that aren't obvious from code alone  
- Always human-review: perf, edge cases, taste, accessibility nuances

The more code feels **local**, **predictable**, and **self-describing in plain text**, the smoother AI interactions usually go — but there's no one right way.

Use, adapt, ignore, or laugh at any part of this doc — it's just a vibe check / memory jog from early 2026 reports.

Last refreshed feel: February 2026
