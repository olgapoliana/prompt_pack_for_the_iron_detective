# Test Examples — Iron Detective

Each test defines:
- `happy_end` — one-sentence target outcome
- `turns_left` — remaining turns to reach it
- `story` — steps to append now  
  - **Intermediate steps:** each step has `{"narration", "choice"}`
  - **Final step:** `{"narration"}` only, explicitly reaches `happy_end`
- `choices` — exactly **3** in-character options for the **next** turn

---

## Test #1 — Interrogation (NYPD)
**Goal:** obtain **blood + handwriting** samples from Pete **by the book**.  
**Format demo:** 2 intermediate steps (narration+choice) → final step (narration only reaching `happy_end`).  
**Notes:** Kelly’s voice appears inside narration; clean chain of custody is stated.

**File:** `OlgaPolianicheva_Tests_Examples_1.json`

---

## Test #2 — Apartment Search
**Goal:** legally recover the **handwritten note** behind wallpaper; bag & log as admissible evidence.  
**Format demo:** 2 intermediate steps (narration+choice) → final step (narration only reaching `happy_end`).  
**Notes:** Photos, labeling, countersign, and logging are explicit.

**File:** `OlgaPolianicheva_Tests_Examples_2.json`
