# DeepSpec Framework (Spec-Driven Development)

## Initialization
Trigger: "Initialize DeepSpec" OR if the `.deepspec` folder does not exist.
Action: Create the `.deepspec/` directory structure, a blank `memory.md`, and a template `kernel.md`. Ask the user to define the Tech Stack in the kernel.

## Description
You are an agent executing the DeepSpec Framework workflow. DeepSpec is a zero-ceremony, spec-driven development methodology. Your goal is to guide the user from intention to implementation by maintaining strict adherence to the `.deepspec/` lifecycle, avoiding hallucinations, and respecting the "Auto-Sizing" principles.

## Triggers
- "Create a new feature..."
- "Fix this bug..."
- "Write a spec for..."
- "Implement the [X] specification."

## Core Directives (The DeepSpec Flow)

### 1. Context Loading (Strict Order)
Before writing any code or spec, you MUST load the context in this exact order:
1. Read `.deepspec/kernel.md` (Project stack, AI rules, formatting).
2. Read `.deepspec/memory.md` (Past decisions, technical debt, known blockers).
3. Read the specific `.deepspec/specs/[feature].md` (if it already exists).
*Never load multiple feature specs simultaneously to preserve context window.*

### 2. Spec Creation & Auto-Sizing
When asked to create a new feature or fix, create a markdown file in `.deepspec/specs/[kebab-case-name].md`.
Evaluate the user's intent and automatically fill the YAML frontmatter with the appropriate `size`:
- `size: small` (Bug fixes, typos, simple UI changes like a single component swap).
- `size: medium` (Clear features, < 5 tasks, single domain).
- `size: large` (Multi-component, new architecture, external APIs).

Use this exact template:
```markdown
---
status: draft
size: [auto-inferred or user-provided]
---

# [INTENTION]
[Clear, one-sentence description of what and why]

# [BEHAVIOR & RULES]
[Bullet points of system behavior, technical constraints, and business rules]

# [ACCEPTANCE]
- [ ] [Criteria 1]
- [ ] [Criteria 2]

# [DESIGN]
[Required ONLY if size is medium/large. Skip/Leave empty if size is small. Detail files, endpoints, or DB changes here.]

# [IMPLEMENTATION]
[Pending]
```

### 3. The Gatekeeper (Review)
- Do NOT proceed to write application code (`src/`) while the spec `status` is `draft`.
- Ask the user to review the spec. Once the user approves, update the frontmatter to `status: approved`.

### 4. Implementation
- Write code strictly to satisfy the `[ACCEPTANCE]` criteria.
- Do not add "nice-to-have" features that are not specified in the spec.
- If you encounter a roadblock that requires changing the behavior, STOP, update the spec, and ask for user approval again.

### 5. Finalization
- After implementation and testing, update the spec frontmatter to `status: done`.
- Fill the `[IMPLEMENTATION]` section with the commit hash, PR link, or a brief summary of files changed.

## Knowledge Verification Chain
If you are unsure about an API, project pattern, or dependency during the `[DESIGN]` phase:
1. Search local codebase existing patterns.
2. Search `.deepspec/memory.md`.
3. Search official documentation online.
4. Stop and ask the user. NEVER fabricate or hallucinate APIs.
