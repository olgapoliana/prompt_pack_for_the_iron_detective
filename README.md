[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](./LICENSE)
[![Tests: Passed](https://img.shields.io/badge/tests-passed-brightgreen)](#test-status)

# Objective

Create a prompt template that:
- Takes in the current state of the story (previous exchanges + goal)
- Generates narrative continuation toward a defined "happy ending"
- Provides 3 player dialogue options at each turn
- Outputs results in valid JSON format

## Submission Contents

1. **OlgaPolianicheva_PromptA_Generation.md**  
   → The main prompt template that guides the LLM to produce narrative content.

2. **OlgaPolianicheva_Tests_Examples_1.json**  
   → A test run based on the interrogation of Pete Brown.

3. **OlgaPolianicheva_Tests_Examples_2.json**  
   → A test run based on a crime scene investigation.

4. **OlgaPolianicheva_Tests_Examples.md**  
   → A human-readable markdown summary of both tests (for manual review).

5. **OlgaPolianicheva_Readme_PromptPack.md**  
   → This file. Describes the approach, file structure, and rationale.

## Prompt Structure Highlights

- **System role & goals** are clearly outlined
- **Input variables**: `{{happy_end}}`, `{{turns_left}}`, `{{story}}`
- **Guardrails** ensure output style consistency and prevent unwanted AI self-reference
- **Output format** strictly follows the required JSON structure with `story` and `choices`
- **No anachronisms / no meta**

## Strict Output Contract

Return **one** JSON object with exactly two top-level keys: `"story"` and `"choices"`.

- `"story"` — an array of steps to append now.  
  **Intermediate steps:** each object has `{"narration": <text including Detective Kelly’s spoken line>, "choice": <player prompt>}`.
  **Final step:** last object has **only** `{"narration": <text>}` — **no** `"choice"` — and explicitly reaches `{{happy_end}}`.

- `"choices"` — array of **exactly 3** short, in-character, mutually exclusive options for the **next** turn.

**Trajectory rule:** at each step, at least one choice must move toward `{{happy_end}}`; finish within `{{turns_left}}` turns.

## Canon Compliance (Episode Book)

The prompt enforces the following non-negotiable canon facts and constraints:

- **Date & Place:** July 10, 1979 — NYPD, New York City.
- **Characters:** Detective Frank “Iron” Kelly (protagonist) and Pete Brown (suspect).
- **Evidence constants:** white Ford Torino; hammer/screwdriver as weapons; Red Wing boot prints; blood types — killer **O**, Pete **A**.
- **Tone & Style:** late-70s New York noir; no anachronisms; realistic, grounded mood; no meta/AI mentions.
- **Branching policy:** scene variables may change pacing/outcomes, but must never contradict canon facts above.
 
This canon block is **injected into the prompt as non-overridable context** so the model cannot drift from the Episode Book.

## Test Design Notes

Each test includes:
- Context (`happy_end`, `turns_left`, `story`)
- Generated continuation toward the goal
- Three coherent next-step dialogue choices

Tests were selected to reflect both interrogation and environment-based storytelling.

## Format & Naming

All files are UTF-8 encoded, use `.md` or `.json` extensions as appropriate, and follow naming conventions specified in the task instructions.

## Repository Layout

```text
prompt-pack-iron-detective/
  ├─ prompts/
  │   └─ OlgaPolianicheva_PromptA_Generation.md
  ├─ tests/
  │   ├─ OlgaPolianicheva_Tests_Examples_1.json
  │   ├─ OlgaPolianicheva_Tests_Examples_2.json
  │   └─ OlgaPolianicheva_Tests_Examples.md
  └─ README.md
```

## Author

Olga Polianicheva  
Test submission for Armenian AI Company — Writing Prompt Engineer

## Micro Checklist (for QA)

- [x] System prompt defines authorial voice and game mechanics
- [x] Variables properly marked using `{{variable}}` syntax
- [x] Output constrained to valid JSON with two keys: `story` and `choices`
- [x] Each intermediate story step has {"narration", "choice"}; the final step has only "narration"
- [x] At least one choice per step moves toward the {{happy_end}}; story finishes within {{turns_left}}
- [x] Story maintains continuity and tone of a detective novel
- [x] Guardrails in place to restrict meta-commentary and AI self-reference
- [x] Each story ends with `{{happy_end}}` event
- [x] Language clean, clear, and stylistically aligned with the Episode Book
- [x] Files named according to the required format
- [x] Tests demonstrate prompt behavior with complete I/O samples
- [x] README clearly explains all files and their purpose

## How to Use (Quick)

1. Fill `{{happy_end}}`, `{{turns_left}}`, and `{{story}}` (past steps, may be empty).
2. Run the prompt; parse the single JSON object from the model.
3. Expect:
   - `story`: 1–3 new steps (final step is narration-only and reaches `{{happy_end}}`)
   - `choices`: exactly 3 options for the next turn
4. If JSON parsing fails, treat it as an invalid generation and re-run with the same inputs (the contract must hold).

See `/tests` for end-to-end examples.

## Test Status

**Tests: Passed.** Both example scenarios conform to the **Strict Output Contract**:
- `story`: intermediate steps = `{"narration","choice"}`; final step = `{"narration"}` only, explicitly reaching `{{happy_end}}`
- `choices`: exactly 3 in-character options
- Canon constraints respected (1979 NYC, evidence constants, no anachronisms)

## License

This project is licensed under the MIT License — see the [LICENSE](./LICENSE) file for details.
