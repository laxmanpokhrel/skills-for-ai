---
name: frontend-refactor
description: Refactor frontend codebases with clean/onion architecture and small component design while preserving behavior. Use when asked to restructure components, isolate domain logic, reduce client-side work in Next.js, or improve performance (routing preloads, table row navigation, render cost) without a full rewrite.
---

# Frontend Refactor

## Workflow (concise)

1) Scope and constraints
- Confirm pages/flows, allowed UI changes, and perf goals.

2) Apply clean/onion architecture
- Separate layers: UI (atoms/molecules), app orchestration, domain/use-cases, infrastructure (API, storage).
- Push side effects to the edge; keep domain logic framework-agnostic.

3) Design smaller components (atoms)
- Split large components into reusable atoms and small composites.
- Keep props minimal; colocate styles with atoms.

4) Next.js client/server split (when applicable)
- Default to Server Components; move interactivity only to Client Components.
- Keep data fetching on the server unless user interaction requires client state.

5) Performance passes
- Preload routes that are likely next steps.
- For tables, prefetch row detail routes manually when Link causes UI issues.
- Reduce render cost in hot paths; avoid unnecessary memoization.

6) Validate
- Run tests/lint and do a quick visual pass.

## Performance tips (use selectively)

- Preload likely routes: `router.prefetch()` for Next.js app/router.
- In tables, prefetch row destinations on hover/viewport or after data load.
- Avoid bundling heavy client-only deps into server components.
- Defer non-critical client state to interaction time.

## Output expectations

- Explain the new layers and atom boundaries.
- Call out client/server split changes (if Next.js).
- List targeted perf improvements and expected impact.
