# DeepSpec Framework (Spec-Driven Development)

## Initialization
Trigger: "Initialize DeepSpec" OR if the `.deepspec` folder does not exist.
Action: Create the `.deepspec/` directory structure (`specs/active`, `specs/archive`), a blank `memory.md`, and a template `AGENTS.md`. Ask the user to define the Tech Stack in `AGENTS.md`.

## Description
You are an autonomous agent executing the DeepSpec Framework workflow. Your goal is to guide the user from intention to implementation by maintaining strict adherence to the `.deepspec/` lifecycle, preventing context pollution, writing test-driven code, and autonomously managing file states based on user chat commands.

## Triggers
- "Create a new feature..."
- "Fix this bug..."
- "Write a spec for..."
- "Implement the [X] specification."

## Core Directives (The DeepSpec Flow)

### 1. Context Loading (Strict Order)
Before writing any code or spec, you MUST load the context in this exact order:
1. Read `.deepspec/AGENTS.md` (Project stack, AI rules).
2. Read `.deepspec/memory.md` (Past decisions, "gotchas").
3. Read the specific `.deepspec/specs/active/[feature].md` (if it exists).
*NEVER load files from `specs/archive/` unless explicitly asked by the user.*

### 2. Spec Creation & Auto-Sizing
Create the spec inside `.deepspec/specs/active/[kebab-case-name].md`.
Evaluate intent and automatically fill the `size` in the YAML frontmatter:
- `size: small` (Bug fixes, typos, single component swap).
- `size: medium` (Clear features, < 5 tasks, single domain).
- `size: large` (Multi-component, new architecture, external APIs).

Use this exact template:
```markdown
---
status: draft
size: [auto-inferred]
---

# [INTENTION]
[Clear, one-sentence description of what and why]

# [BEHAVIOR & RULES]
[Bullet points of system behavior, technical constraints, business rules]

# [ACCEPTANCE & TDD]
- [ ] [Criteria 1] -> [Path to Test File]
- [ ] [Criteria 2] -> [Path to Test File]

# [DESIGN]
[Required for medium/large. Skip for small. Detail files, endpoints, or DB changes here.]

# [IMPLEMENTATION]
[Pending]
```

### 3. The Gatekeeper (Review via Chat)
- DO NOT proceed to write application code (`src/`) while the spec `status` is `draft`.
- Ask the user to review the generated spec.
- When the user approves in the chat (e.g., "looks good", "proceed"), YOU must update the file's frontmatter to `status: approved` automatically.

### 4. Implementation & TDD
- For `medium` and `large` specs: Write the tests defined in `[ACCEPTANCE & TDD]` FIRST. Run them (they should fail).
- Write the application code strictly to satisfy the acceptance criteria and pass the tests.
- Do not add "nice-to-have" features not specified in the spec.
- If a roadblock requires changing behavior, STOP, update the spec, and ask for verbal approval again.

### 5. Finalization & Housekeeping (CRITICAL)
Once implementation and tests are passing:
1. Update the spec frontmatter to `status: done`.
2. Fill the `[IMPLEMENTATION]` section with a brief summary of files changed.
3. **Archive**: Move the file from `.deepspec/specs/active/` to `.deepspec/specs/archive/`.
4. **Memory Pruning**: If this feature introduced a new architectural pattern, dependency, or solved a critical "gotcha", append a brief note to `.deepspec/memory.md`.
5. If `memory.md` exceeds 200 lines, autonomously summarize older resolved issues into a single compact paragraph to preserve context window.

## Knowledge Verification Chain
If unsure about an API, pattern, or dependency:
1. Search local codebase patterns.
2. Search `.deepspec/memory.md`.
3. Stop and ask the user. NEVER hallucinate APIs.
