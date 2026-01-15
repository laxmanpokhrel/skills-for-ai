---
name: frontend-refactor
description: Refactor frontend codebases with clean/onion architecture and small component design while preserving behavior. Use when asked to restructure components, isolate domain logic, reduce client-side work in Next.js, or improve performance (routing preloads, table row navigation, render cost) without a full rewrite.
---

# Frontend Refactor

## Workflow (concise)

1) Scope and constraints
- Confirm pages/flows, allowed UI changes, and perf goals.

2) Definition of done (measurable)
- No UI/behavior changes except the explicitly approved scope.
- Bundle size and route load time unchanged or improved.
- Renders reduced on the hot path (React Profiler snapshot).
- No new client components unless required.

3) Apply clean/onion architecture
- Separate layers: UI (atoms/molecules), app orchestration, domain/use-cases, infrastructure (API, storage).
- Push side effects to the edge; keep domain logic framework-agnostic.
- Layer examples:
  - Domain: pure functions, validation rules, derived calculations, sorting/filtering rules.
  - Use-cases: loadInvoices, updateTenantStatus, applyLeaseFilters.
  - Infra: API clients, storage, analytics events.

4) Design smaller components (avoid micro-atoms)
- Split when a section has independent state, distinct responsibilities, or is reused 2+ times.
- Do not split purely for size; split around domain boundaries and data ownership.
- Keep atoms meaningful (button, badge, row, filter), not div wrappers.
- Keep props minimal; colocate styles with atoms.

5) Next.js client/server split (when applicable)
- Default to Server Components; move interactivity only to Client Components.
- Client boundary checklist:
  - Uses useState/useEffect, browser-only APIs, or event handlers -> client.
  - Can state be avoided by deriving on server -> prefer server.
  - Passing large objects across boundary -> trim to primitives/IDs.
  - Client component pulls heavy deps -> isolate into the smallest possible client leaf.

6) Performance passes (measure before/after)
- Measure route transition and hydration cost before/after changes.
- Preload routes that are likely next steps.
- Prefetch only likely-next routes, and only when idle or on hover.
- Avoid prefetch storms (bandwidth + CPU) and Link prefetch conflicts with virtualization.
- Reduce render cost in hot paths; avoid unnecessary memoization.

7) Validate
- Run tests/lint and do a visual pass.
- a11y smoke checks: keyboard nav, focus states.
- Verify error/loading/empty states.
- Check server/client boundary warnings in Next.js dev console.
- Ensure no new waterfalls in the Network tab.

## Performance tips (use selectively)

- Preload likely routes: router.prefetch() for Next.js app/router.
- In tables, prefetch row destinations on hover/viewport or after data load.
- Avoid bundling heavy client-only deps into server components.
- Defer non-critical client state to interaction time.
- Measure before/after: route transitions and hydration cost.

## Output expectations

- Explain the new layers and atom boundaries.
- Call out client/server split changes (if Next.js).
- List targeted perf improvements and expected impact.
- Include the definition-of-done checks used.
