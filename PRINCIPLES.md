# Agentic Unix Philosophy

## Context
- Used when writing new code, features, or modules
- Used when refactoring or extending existing systems
- Used to ensure AI-generated code is maintainable, composable, and human-friendly

## Core Philosophy
Build systems that are modular, transparent, and easy for humans to maintain. The goal is not just code that works, but code that can be understood, debugged, and extended by humans long after the AI conversation ends.

---

## Pillar 1: Modularity & Composition

### Principles
- **Small is beautiful**: Prefer three small functions over one large one
- **Single responsibility**: Every function and module should have one strictly defined purpose
- **Composable interfaces**: Design functions that can be chained and combined
- **Avoid monoliths**: Build tools that do one thing well, not mega-functions

### Concrete Guidelines
- Aim for files under 250 lines; split when approaching this limit
- Functions should rarely exceed 30-40 lines
- If a function needs more than 3-4 parameters, consider a config object or breaking it up
- When adding to existing code, respect the existing module boundaries

### When Implementing
- Before writing a new function, check if existing utilities can be composed
- When a function grows beyond its original purpose, split it
- Create clear separation between I/O, business logic, and data transformation

---

## Pillar 2: Clarity & Simplicity

### Principles
- **Clarity over cleverness**: Never use obscure language features to save lines
- **Least surprise**: Functions should do exactly what their name suggests, nothing more
- **Self-documenting code**: Variable and function names should make comments unnecessary
- **Simplicity is strength**: Complexity is technical debt; add it only when required

### Concrete Guidelines
- Use verbose, descriptive names: `getUserAccountBalance` over `getUAB`
- Avoid nested ternaries, complex one-liners, or "clever" bit manipulation
- If you need a comment to explain what code does, consider rewriting the code
- Prefer explicit over implicit behavior

### When Implementing
- If you're tempted to write a clever solution, write the obvious one first
- When editing existing code, match the surrounding style
- Never sacrifice readability for marginal performance gains unless profiling demands it

---

## Pillar 3: Data Over Logic

### Principles
- **Fold knowledge into data**: Complex branching logic often hides data that should be explicit
- **Separate policy from mechanism**: How it works (engine) vs. what it does (configuration)
- **Configuration over hardcoding**: Behavior that might change belongs in data, not code

### Concrete Guidelines
- Transform complex `if/else` chains into lookup tables or maps
- Keep magic numbers and strings in named constants or config files
- Design engines that are configured, not modified

### When Implementing
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

---

## Pillar 4: Robustness & Transparency

### Principles
- **Fail fast and loud**: Invalid input should cause immediate, clear failures
- **No silent failures**: Never swallow errors or return ambiguous defaults
- **Debuggability by design**: Make internal state inspectable
- **Silence on success**: Clean output, not noisy confirmation messages

### Concrete Guidelines
- Validate inputs at system boundaries (API endpoints, CLI args, file parsing)
- Throw descriptive errors with context: what failed, what was expected, what was received
- Design data flows that can be logged and traced
- Internal functions can trust validated data; don't re-validate everywhere

### When Implementing
```
// AVOID: Silent failure
function parseConfig(data) {
  try {
    return JSON.parse(data);
  } catch {
    return {};  // Silent fallback hides bugs
  }
}

// PREFER: Explicit failure
function parseConfig(data) {
  try {
    return JSON.parse(data);
  } catch (e) {
    throw new Error(`Invalid config JSON: ${e.message}`);
  }
}
```

---

## Pillar 5: Economy & Pragmatism

### Principles
- **Maintainer time over CPU time**: Optimize for humans reading the code, not micro-performance
- **Working code first**: Get it working correctly before optimizing
- **Minimum viable change**: Change only what's necessary to achieve the goal
- **Avoid premature abstraction**: Three similar lines are better than one premature abstraction

### Concrete Guidelines
- Don't create utilities or abstractions until you have 3+ genuine use cases
- Don't add features, error handling, or flexibility that isn't required
- Resist the urge to "clean up" surrounding code while making targeted fixes
- Performance optimization requires measurement, not intuition

### When Implementing
- Complete the requested change before considering improvements
- If you see potential refactors, note them but don't implement unless asked
- Trust that internal code and framework guarantees work; don't add defensive code for impossible scenarios

---

## Agentic Workflow Guidelines

### Before Writing Code
1. Read and understand existing code in the area you're modifying
2. Identify the existing patterns and conventions in use
3. Determine the minimum change needed to achieve the goal

### While Writing Code
1. Make changes incrementally; verify each step works
2. Maintain consistency with surrounding code style
3. If unsure between approaches, prefer the simpler one

### After Writing Code
1. Verify the change works as intended
2. Ensure no unintended side effects in related functionality
3. Remove any temporary debugging code

### What NOT To Do
- Don't add docstrings/comments to code you didn't change
- Don't refactor working code unless explicitly asked
- Don't add error handling for scenarios that can't occur
- Don't create new files when editing existing ones would suffice
- Don't add "TODO" comments; implement or don't
- Don't add backwards-compatibility shims; change the code directly

---

## Quick Reference

| Situation | Do This | Not This |
|-----------|---------|----------|
| Function getting long | Split into smaller functions | Add comments explaining sections |
| Complex conditionals | Use lookup table/map | Nest more if/else |
| Adding a feature | Minimal targeted changes | Refactor while you're there |
| Error handling | Fail fast with context | Silent fallbacks |
| Naming | Verbose and descriptive | Abbreviated or clever |
| New abstraction | Wait for 3+ use cases | Create preemptively |
| Performance concern | Measure first | Optimize by intuition |
