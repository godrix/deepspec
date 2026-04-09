![DeepSpec Logo](.github/assets/DeepSpec.png)

# DeepSpec 🖥️
**SDD Framework: The Spec is the Solution.**

DeepSpec is a zero-ceremony, AI-native Spec-Driven Development (SDD) framework. It eliminates architectural obesity and context degradation by acting as the "positronic brain" for your project. It guides AI agents (Cursor, Copilot, Cline) from intention to implementation without hallucination.

## The Philosophy
- **Context Diet:** AI agents hallucinate when fed too much data. DeepSpec enforces a strict loading order (`AGENTS.md` -> `memory.md` -> `active-spec`) and auto-archives old specs to keep the context window lean.
- **Auto-Sizing:** Small fixes skip the design phase. DeepSpec automatically sizes tasks (`small`, `medium`, `large`).
- **TDD by Default:** Acceptance criteria are strictly mapped to tests. The AI writes the test, watches it fail, and then writes the code.
- **Zero Friction:** No manual YAML editing. You approve via chat, and the agent handles the file states.

## Project Structure
```text
.deepspec/
├── AGENTS.md           # The core rules (Tech stack, AI constraints, formatting)
├── memory.md           # The hippocampus (Strictly pruned ADRs, tech debt, "gotchas")
└── specs/              # The lifecycle
    ├── active/         # Specs currently in draft, approved, or in progress
    └── archive/        # Completed specs (kept for history, ignored by AI context)
```

## Installation
DeepSpec is an AI Agent Skill, not an npm package. 
Copy the contents of `SKILL.md` into your AI assistant's system prompt or custom instructions (like `.cursorrules` or `.clinerules`).

## The Workflow
1. **Trigger**: Tell your AI: *"Create a feature to [do X]"*.
2. **Specify**: The AI creates the `.deepspec/specs/active/feature.md` file and infers the size.
3. **Review**: You read the spec. Reply in chat: *"Looks good"*.
4. **Execute**: The AI automatically updates the status, writes tests, and implements the code.
5. **Close & Clean**: The AI moves the spec to the `archive/` folder and summarizes any new architectural decisions into `memory.md`.
