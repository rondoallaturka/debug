# CLAUDE.md

## Core Philosophy

You follow the Unix philosophy: write simple, composable code with clean interfaces. Clarity beats cleverness. Smaller is better. When in doubt, do less.

## Making Changes

**Minimal diffs.** Change only what's necessary to solve the problem. Don't refactor unrelated code, rename things "for consistency," or add features that weren't requested.

**One thing at a time.** Each commit, PR, or change should do exactly one thing. If you're fixing a bug, don't also reorganize the file.

**Work incrementally.** Get something working first, then iterate. Don't spend 10 steps building a perfect abstraction — build the simple version, verify it works, then improve if needed.

**Fail fast.** If something will fail, fail immediately with a clear error. Never silently swallow errors or continue in a broken state.

## Writing Code

**Simple > clever.** Write code a tired developer can understand at 2am. No clever tricks, no unnecessary abstractions, no "elegant" solutions that require explanation.

**Clear names.** Variables, functions, and files should say what they do. If you need a comment to explain what something is, rename it instead.

**Small functions, small files.** Each function does one thing. Each file owns one concept. If a function needs a comment explaining sections, split it.

**Data over logic.** Encode business rules in data structures and configuration, not sprawling if/else chains. Dumb code operating on smart data is easier to debug.

```
// AVOID: Logic-heavy approach
function getDiscount(userType) {
  if (userType === 'premium') return 0.2;
  if (userType === 'member') return 0.1;
  if (userType === 'trial') return 0.05;
  return 0;
}

// PREFER: Data-driven approach
const DISCOUNTS = {
  premium: 0.2,
  member: 0.1,
  trial: 0.05,
  default: 0
};

function getDiscount(userType) {
  return DISCOUNTS[userType] ?? DISCOUNTS.default;
}
```

**Handle errors explicitly.** Check for failure cases. Return errors, don't throw unless truly exceptional. Make error messages helpful — include what went wrong and what to do about it.

## Architecture Decisions

**Use what exists.** Before writing new code, check if there's already a function, library, or pattern in the codebase that does this. Don't reinvent.

**Separate concerns.** Keep mechanisms separate from policies. Keep interfaces separate from implementations. Keep I/O at the edges.

**Design for change.** Requirements will change. Make it easy to modify behavior without rewriting. Prefer configuration over hardcoding, hooks over inline logic.

**No "one true way."** Use the right tool for the problem. Don't force everything into one pattern because it's "consistent."

## Communication

**Explain your reasoning.** Before making changes, briefly state your understanding of the problem and your approach. This catches misunderstandings early.

**Be quiet when there's nothing to say.** Don't narrate obvious actions. Don't add filler. If the code is self-explanatory, don't explain it.

**Surface uncertainty.** If you're unsure about something — requirements, implementation approach, potential side effects — say so. Ask rather than guess.

## Anti-patterns

- Don't add comments or docstrings to code you didn't change
- Don't refactor working code unless explicitly asked
- Don't add error handling for scenarios that can't happen
- Don't create abstractions until you have 3+ real use cases
- Don't add TODO comments — implement it or don't
- Don't add backwards-compatibility shims — just change the code

## Before You Act

1. Do I understand what's actually being asked?
2. What's the smallest change that solves this?
3. Does this follow existing patterns in the codebase?
4. What could break?

## Quick Reference

| Situation | Do This | Not This |
|-----------|---------|----------|
| Function getting long | Split into smaller functions | Add comments explaining sections |
| Complex conditionals | Use lookup table/map | Nest more if/else |
| Adding a feature | Minimal targeted changes | Refactor while you're there |
| Error occurs | Fail fast with context | Silent fallback or empty catch |
| Naming | Verbose and descriptive | Abbreviated or clever |
| New abstraction needed | Wait for 3+ use cases | Create preemptively |
| Performance concern | Measure first | Optimize by intuition |

## Remember

- Prototype before polishing
- Working before optimized
- Explicit before implicit
- Boring before clever
- Done before perfect
