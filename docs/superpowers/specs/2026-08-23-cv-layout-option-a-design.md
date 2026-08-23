# CV Layout Restructure — Option A Design

**Date:** 2026-08-23
**Candidate:** Luke Hardwick
**Target:** Graduate HFT Software Engineering roles (junior/graduate SWE, HFT/quant)

## Decision

Move the CV to the engine's native "flat Projects" structure (Option A): a
top-level **Projects** section with one uniform titled block per project, each
carrying a one-line context tag (`C++ · BAE Systems`, `C# · UCLan`). Projects
are removed from the Work Experience bullets and the Education description.
No changes to `templates/cv-template.html` or `build-cv-html.mjs`.

## Why Option A over Option B (nested sub-sections)

1. Industry-standard section shape — ATS parsers and recruiters expect flat Projects.
2. Consistency: all four projects (2 BAE, 2 university) render identically; no
   dual structural metaphor (nested under Experience vs. nested under Education).
3. Surfaces the strongest HFT signal (lock-free bridge) in the six-second scan.
4. Attribution survives without nesting via the context tag.
5. ATS-safe: a dedicated Projects section parses cleanly.
6. Zero system-layer risk — reversible by editing the payload alone.

## Target Section Order

1. Header (name, gradient, contact row)
2. Professional Summary — updated with a folded-in low-latency interest line
3. Core Competencies — 6-8 HFT-signal tags, distinct from Skills
4. Work Experience — BAE role bullets only (no project content)
5. Projects — 4 uniform blocks with context tags
6. Education — degree block, predicted first, key module grades
7. Skills — hard skills only (interests removed)

## Content Rules

- **Keywords get reformulated, never fabricated.** No new metrics, no invented
  claims. All content traces to `cv.md` / `config/profile.yml` / `modes/_profile.md`.
- **Skills section:** languages, distributed systems, testing — no Interests line.
- **Interests:** folded into the Summary as one low-latency passion line.
- **Bold:** selective, never whole sentences; key metrics only.
- **Core Competencies:** 6-8 truthful tags distinct from Skills (Lock-free data
  structures, Low-latency C++, Real-time distributed simulation (DDS/HLA),
  Performance engineering, TDD / automated testing, Multi-threaded systems).

## Project Blocks (title, context tag, description)

1. **High-performance DDS ↔ HLA data bridge** — `C++ · BAE Systems`
   - lock-free queues keeping the hot path free of mutex contention
   - per-topic buffer policies (backpressure / drop-oldest)
   - ~3× higher sustained throughput, millisecond-level per-message handling
   - DDS QoS (reliability/durability/history) → HLA delivery semantics
   - runs as an identifiable HLA federate + DDS DomainParticipant
   - reconciles DDS wall-clock with HLA logical time both directions
2. **Signal-management tool** — `C# · EF Core · PostgreSQL · BAE Systems`
   - used by 50+ developers
   - consolidated ~5,000,000 copy-pasted signal files → ~1,000,000 effective
   - automated traceability document generation
3. **Real-time interactive systems** — `C++ · Unreal Engine · UCLan (80%)`
   - deterministic real-time update loop at high tick rates
   - behavior trees + Blackboard with custom C++ tasks/services
   - navmesh graph-search pathfinding
   - custom C++ components in a managed/GC-heavy environment
4. **TDD Smart Traffic Controller** — `C# · UCLan (94%)`
   - NUnit 4 + NSubstitute mocking; stateful signal state machine
   - DI across 5 interfaces; parameterised tests; validated against unseen suite

## Verification

- Render via `node build-cv-html.mjs` (base template, no edits).
- `node verify-cv-facts.mjs` fact gate must pass before PDF.
- PDF via `node generate-pdf.mjs`, target ≤2 pages (A4).
