# CV Layout Restructure (Option A) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure Luke Hardwick's CV to the engine's native flat-Projects layout (Option A) — projects as a top-level section with context tags — and re-render HTML/PDF with zero template changes.

**Architecture:** No changes to `templates/cv-template.html` or `build-cv-html.mjs`. The restructure lives entirely in `cv.md` (source of truth) and the JSON payload passed to `build-cv-html.mjs`. Verify with the existing fact-gate and PDF scripts.

**Tech Stack:** Markdown (cv.md), Node.js scripts (`build-cv-html.mjs`, `verify-cv-facts.mjs`, `generate-pdf.mjs`), JSON payload.

## Global Constraints

- **No fabrication.** Every bullet traces to current `cv.md` / `config/profile.yml` / `modes/_profile.md`. Reformulate, never invent.
- **No template or build-script edits.** The engine's native structure is sufficient.
- **Interests removed from Skills**; folded into Summary as one low-latency line.
- **Bold only on key metrics** (`~3×`, `50+`, `120Hz`), never whole sentences.
- **Core Competencies:** 6-8 truthful tags distinct from Skills.
- **Project context tags:** one line each (`C++ · BAE Systems`, `C# · UCLan`).
- **Output:** A4 PDF, ≤ 2 pages.

---

### Task 1: Restructure `cv.md` source of truth

**Files:**
- Modify: `cv.md` (full rewrite of Experience / Education / Projects / Skills sections)

**Interfaces:**
- Produces: the canonical content all later render steps read from.

- [ ] **Step 1: Rewrite `cv.md` sections**

Apply Option A structure per the design doc:
- Experience: BAE scheme bullets only (no project content).
- Projects: top-level section, 4 uniform blocks, each with context tag.
- Education: degree block + predicted first + key module grades (no projects).
- Skills: languages, distributed systems, testing — no Interests line.
- Summary: fold in the low-latency interest line.

- [ ] **Step 2: Review the diff against the no-fabrication rule**

Confirm no new metrics or claims were introduced.

- [ ] **Step 3: Commit**

```bash
git add cv.md
git commit -m "cv: restructure to flat Projects layout (Option A)"
```

---

### Task 2: Build JSON payload and render HTML

**Files:**
- Create: `/tmp/cv-luke-hardwick.json` (render payload)
- Create: `output/cv-luke-hardwick-option-a.html`

**Interfaces:**
- Consumes: restructured `cv.md` (Task 1).
- Produces: HTML that the fact gate (Task 3) and PDF (Task 4) consume.

- [ ] **Step 1: Build the JSON payload**

Shape per the `pdf.md` JSON Input Schema:
- `experience[]`: BAE with scheme bullets only.
- `projects[]`: 4 entries `{name, badge?, tech, description}` where `tech` carries the context tag (`C++ · BAE Systems`, etc.).
- `education[]`: degree + predicted first + module grades.
- `skills[]`: languages / distributed systems / testing only.
- `competencies[]`: 6-8 HFT-signal tags.

- [ ] **Step 2: Render**

Run:
```bash
node build-cv-html.mjs /tmp/cv-luke-hardwick.json output/cv-luke-hardwick-option-a.html
```
Expected: exit 0; HTML has 4 `.project` blocks, no project content in `.job ul`.

- [ ] **Step 3: Commit (only if payload worth keeping; otherwise note)**

```bash
git add output/cv-luke-hardwick-option-a.html
git commit -m "cv: render Option A layout HTML"
```

---

### Task 3: Fact gate

**Files:**
- Test: `output/cv-luke-hardwick-option-a.html`

- [ ] **Step 1: Run the fact gate**

Run:
```bash
node verify-cv-facts.mjs output/cv-luke-hardwick-option-a.html
```
Expected: PASS (no invented metrics). If FAIL, fix by removing invented claims and re-render (Task 2).

---

### Task 4: Generate and verify PDF

**Files:**
- Create: `output/cv-luke-hardwick-option-a.pdf`

- [ ] **Step 1: Render PDF**

Run:
```bash
node generate-pdf.mjs output/cv-luke-hardwick-option-a.html output/cv-luke-hardwick-option-a.pdf --format=a4
```
Expected: exit 0, page count ≤ 2.

- [ ] **Step 2: Verify page count and layout**

Confirm ≤ 2 pages; Projects section shows 4 titled blocks with context tags.

- [ ] **Step 3: Commit**

```bash
git add output/cv-luke-hardwick-option-a.pdf
git commit -m "cv: generate Option A layout PDF"
```

---

### Task 5: Report and close out

- [ ] **Step 1: Summarize to user**

Report: PDF path, page count, keyword coverage, any gaps; flag that the previous per-company tailored CVs are untouched and Option A is the new base.

- [ ] **Step 2: Update todo list** — mark all completed.
