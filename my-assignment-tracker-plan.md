# My Assignment Tracker — Plan

## Overview

Build a single-file (`assignment-tracker.html`) page titled **My Assignment Tracker**.
The page is built with plain HTML, CSS, and vanilla JavaScript — no frameworks,
no build tools, no external dependencies beyond a Google Font import.

The page displays:
- Three summary cards: **Total Students**, **Submitted**, **Not Submitted**
- One **Student Submission Checklist** (5 students: Aina, Ben, Chandra, Devi, Farid)
  — each student has a checkbox; ticking it increments **Submitted** by 1 and
  decrements **Not Submitted** by 1 in real time
- All five students start **unchecked** (nothing submitted by default)
- All data is hard-coded sample data embedded in a JavaScript `DATA` object

Visual style: strictly **electric blue** (`#0057FF`) and **orange** (`#FF6B00`) —
no green anywhere; Poppins typography; dark header bar.

---

## Color Palette

| Role | Value |
|---|---|
| Primary accent | `#0057FF` (electric blue) |
| Secondary accent | `#FF6B00` (orange) |
| Card 1 background | `#0039B3` (deep blue — total students) |
| Card 2 background | `#0057FF` (electric blue — submitted) |
| Card 3 background | `#FF6B00` (orange — not submitted) |
| Page background | `#F0F4FF` (light blue-tinted grey) |
| Header background | `#001A66` (near-black blue) |

---

## Interactive Logic

The core behaviour of the page is driven by the student checklist:

1. Each student row has a checkbox bound to that student's `submitted` flag in `DATA`.
2. On every `change` event, the handler toggles `submitted` in the data, then calls
   `renderCards()` to recompute and redisplay all three card values.
3. `renderCards()` derives counts by iterating `DATA.students`:
   - **Total Students** = `DATA.students.length` (always 5, never changes)
   - **Submitted** = students where `submitted === true`
   - **Not Submitted** = students where `submitted === false`
4. No page reload is required — all updates are in-memory DOM mutations.

---

## Sub-Tasks

---

### Sub-Task 1 — Page Shell & Typography

**Intent**
Create the base HTML document with the correct `<head>` metadata, Google Fonts
import, and a top-level layout container. Every other sub-task builds on this shell.

**Expected Outcomes**
- `index.html` exists with valid HTML5 boilerplate
- Page title reads "My Assignment Tracker"
- A dark header bar shows the page title in bold white Poppins type
- Poppins (weights 400, 600, 800) is loaded and applied globally
- A CSS grid area (`<main>`) exists ready to receive cards and checklist

**Todo List**
1. Create `index.html` with HTML5 doctype, `<head>`, `<body>`
2. Add `<title>My Assignment Tracker</title>` and viewport meta tag
3. Import Poppins from Google Fonts (weights 400, 600, 800)
4. Write base CSS reset and font defaults inside a `<style>` block
5. Add a `<header>` with bold title "My Assignment Tracker" and subtitle
   "Student Submission Tracker"
6. Add a `<main>` wrapper that will hold all sections

**Relevant Context**
- Header background: `#001A66`, text: white
- Page background: `#F0F4FF`
- Mirrors Sub-Task 1 of `my-tvet-dashboard-plan.md`

**Status:** `[x] done`

---

### Sub-Task 2 — Total Students Card

**Intent**
Build the first summary card that always shows the fixed count of students.
This card never changes because the total is a constant.

**Expected Outcomes**
- Deep blue card (`#0039B3`) with white text
- Displays: emoji icon 🎓, label "Total Students", large headline number `5`
- Rounded corners, generous padding, bold number
- Value sourced from `DATA.students.length`

**Sample Data**
```
Total Students: 5
```

**Todo List**
1. Add a `.cards-grid` wrapper `<div>` inside `<main>`
2. Add `<section class="card card--deepblue">` as the first card inside `.cards-grid`
3. Include emoji icon, label, and large `<span class="card__number">` showing `5`
4. Define shared `.card` CSS and the `.card--deepblue` color override
5. Populate value from `DATA.students.length` via `renderCards()` function stub

**Relevant Context**
- Card background: `#0039B3` (deep blue, same as TVET dashboard Card 3)
- Shared `.card` styles (padding, border-radius, shadow) apply to all three cards

**Status:** `[x] done`

---

### Sub-Task 3 — Submitted Card

**Intent**
Build the second summary card showing how many students have been marked as
submitted. Its displayed number updates live whenever the checklist is toggled.

**Expected Outcomes**
- Electric blue card (`#0057FF`) with white text
- Displays: emoji icon ✅, label "Submitted", large headline number (starts at `0`)
- Number reflects a live count of students with `submitted === true`
- Recalculates immediately on every checklist toggle

**Sample Data**
```
Submitted: 0  (default — all students start unchecked)
```

**Todo List**
1. Add `<section class="card card--blue">` as the second card in `.cards-grid`
2. Include emoji icon, label, and `<span id="count-submitted" class="card__number">`
3. Add `.card--blue` color override CSS (`#0057FF`)
4. `renderCards()` writes the submitted count to `#count-submitted`

**Relevant Context**
- Card background: `#0057FF` (electric blue — no green used anywhere)
- The count is derived by filtering `DATA.students` where `submitted === true`
- This card and the Not Submitted card always sum to Total Students (5)

**Status:** `[x] done`

---

### Sub-Task 4 — Not Submitted Card

**Intent**
Build the third summary card showing how many students have NOT yet submitted.
It is the mirror of the Submitted card and updates live on every checklist toggle.

**Expected Outcomes**
- Orange card (`#FF6B00`) with white text
- Displays: emoji icon ⏳, label "Not Submitted", large headline number (starts at `5`)
- Number reflects a live count of students with `submitted === false`
- Decreases by 1 every time a student checkbox is ticked

**Sample Data**
```
Not Submitted: 5  (default — all students start unticked)
```

**Todo List**
1. Add `<section class="card card--orange">` as the third card in `.cards-grid`
2. Include emoji icon, label, and `<span id="count-not-submitted" class="card__number">`
3. Add `.card--orange` color override CSS (reuse from tvet dashboard pattern)
4. `renderCards()` writes the not-submitted count to `#count-not-submitted`

**Relevant Context**
- The count is derived by filtering `DATA.students` where `submitted === false`
- Card background: `#FF6B00`

**Status:** `[x] done`

---

### Sub-Task 5 — Student Submission Checklist

**Intent**
Build the interactive student checklist with five named students. Each row has a
checkbox; ticking it marks the student as submitted and triggers a live card update.

**Expected Outcomes**
- Section titled "📋 Student Submission Checklist" below the cards grid
- Five rows rendered from `DATA.students`: Aina, Ben, Chandra, Devi, Farid
- **All five students start unchecked** — Submitted = 0, Not Submitted = 5 on load
- Checked rows show the student name with strikethrough and muted styling
- Each toggle immediately calls `renderCards()` to update both live count cards

**Sample Data**
```javascript
DATA.students = [
  { name: "Aina",    submitted: false },
  { name: "Ben",     submitted: false },
  { name: "Chandra", submitted: false },
  { name: "Devi",    submitted: false },
  { name: "Farid",   submitted: false }
]
```

**Todo List**
1. Add `<section class="checklist">` below `.cards-grid` in `<main>`
2. Add a heading "📋 Student Submission Checklist" inside the section
3. Render a `<label class="checklist__item">` with `<input type="checkbox">` and
   student name for each entry in `DATA.students`
4. Add CSS for `.checklist__item` layout and `.checklist__item--done`
   (strikethrough text + muted color) applied when `submitted === true`
5. On `change` event: find the student by index, toggle `submitted`, toggle the
   `.checklist__item--done` class on the row, then call `renderCards()`

**Relevant Context**
- Mirrors the Preparation Checklist pattern from Sub-Task 5 of
  `my-tvet-dashboard-plan.md`
- The event handler is the *only* place that mutates `DATA.students[i].submitted`
- `renderCards()` must be called after every mutation to keep cards in sync

**Status:** `[x] done`

---

## Data Shape Reference

```
DATA
└── students: [ { name, submitted }, ... ]   (5 rows, all submitted: false by default)
```

Derived values (computed in `renderCards()`, never stored):

| Value | Derivation |
|---|---|
| Total Students | `DATA.students.length` |
| Submitted | `DATA.students.filter(s => s.submitted).length` |
| Not Submitted | `DATA.students.filter(s => !s.submitted).length` |

---

## Page Layout (top → bottom)

```
┌─────────────────────────────────────────────┐
│  HEADER — My Assignment Tracker             │
├────────────────┬──────────────┬─────────────┤
│   Card 1       │   Card 2     │   Card 3    │
│ Total Students │  Submitted   │ Not Submit. │
│  (deep blue)   │  (elec. blue)│  (orange)   │
├─────────────────────────────────────────────┤
│  📋 Student Submission Checklist            │
│  [ ] Aina                                   │
│  [ ] Ben                                    │
│  [ ] Chandra                                │
│  [ ] Devi                                   │
│  [ ] Farid                                  │
└─────────────────────────────────────────────┘
```

---

## File Structure

```
assignment-tracker.html   ← single file: HTML + <style> + <script>
```
