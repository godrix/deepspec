![DeepSpec Logo](.github/assets/DeepSpec.png)

# DeepSpec 🖥️
**SDD Framework: The Spec is the Solution.**

DeepSpec is a zero-ceremony, AI-native Spec-Driven Development (SDD) framework. It eliminates architectural obesity, context degradation, and AI hallucinations by acting as the "positronic brain" for your project. It safely guides AI agents (like Cursor, Copilot, or Cline) from intention to implementation using a strict, state-machine workflow.

## The Philosophy
- **Context Diet:** AI agents hallucinate when fed too much data. DeepSpec enforces a strict loading order (`AGENTS.md` -> `memory.md` -> `active-spec`) and autonomously archives old specs to keep the context window lean.
- **Auto-Discovery:** Just say "Initialize". The agent reads your `package.json`, testing setup, linters, and commit history to automatically generate your project's rules (`AGENTS.md`).
- **3-Stage Pipeline:** Specs move through `drafts` (Research/No-Code) ➔ `active` (Execution/TDD) ➔ `archive` (History). Agents are strictly forbidden from writing application code while a spec is in the draft stage.
- **TDD & Step-by-Step:** Medium and large tasks require an Execution Plan. The AI breaks the work into PR-sized steps, writes the test, watches it fail, and then writes the code for each step.
- **Zero Friction:** No manual YAML editing or folder dragging. You approve plans via natural language chat, and the agent handles the file states and migrations.

## Project Structure
```text
.deepspec/
├── AGENTS.md           # The core rules (Auto-discovered Tech stack, AI constraints, Personas)
├── memory.md           # The hippocampus (Strictly pruned ADRs, tech debt, "gotchas")
└── specs/              # The lifecycle pipeline
    ├── drafts/         # Stage 1: Planning, architecture, and research (NO CODE ALLOWED)
    ├── active/         # Stage 2: Specs currently being executed via TDD
    └── archive/        # Stage 3: Completed specs (Ignored by AI context to prevent pollution)
```

## Installation (AI Agent Skill)
DeepSpec doesn't require npm packages. It runs directly in your IDE's AI agent. 

To install, copy the contents of `SKILL.md` and paste it into your AI assistant's system prompt or custom rules file (e.g., `.cursorrules`, `.clinerules`, or GitHub Copilot custom instructions).

## The Workflow
1. **Initialize**: Say *"Initialize DeepSpec"*. The AI scans your repo and sets up the folders and `AGENTS.md` based on your existing stack and conventions.
2. **Trigger**: Tell your AI: *"Create a feature to [do X]"*.
3. **Draft & Plan**: The AI creates a `.deepspec/specs/drafts/feature.md`, infers the size, writes the acceptance criteria mapped to tests, and creates a step-by-step execution plan.
4. **The Gatekeeper (Review)**: You read the draft. Reply in chat: *"Looks good"*.
5. **Execute (Active)**: The AI automatically moves the spec to the `active/` folder, updates the status, and executes the plan strictly **one step at a time**, applying TDD.
6. **Archive & Learn**: Once done, the AI moves the spec to `archive/` and autonomously injects any newly discovered architectural patterns or "gotchas" into `memory.md`.
