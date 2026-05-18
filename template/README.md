# kiro-workspace-template

A multi-agent workspace architecture for [kiro-cli](https://github.com/kirocli/kiro).

Organize multiple AI agents, shared context, learned memories, skills, and task tracking in a single workspace.

## Directory Structure

```
.kiro/
├── steering/         # AI-DLC workflow rules (loaded automatically)
│   └── aws-aidlc-rules/
│       └── core-workflow.md
├── aws-aidlc-rule-details/  # Detailed rules referenced by core-workflow
│   ├── common/
│   ├── inception/
│   ├── construction/
│   ├── extensions/
│   └── operations/
├── agents/           # Agent definitions (JSON)
│   └── example.json
├── learned/          # Auto-accumulated experience
│   └── LEARNED.md
├── prompts/          # Agent system prompts
│   └── example.md
├── settings/         # CLI/LSP settings
│   ├── cli.json
│   └── lsp.json
├── shared/           # Cross-agent shared context
│   └── SHARED-CONTEXT.md
└── skills/           # Reusable skill documents
    ├── auto-learn.md
    ├── delegate-to-local-llm.md
    └── output-templates.md

tasks/
├── <task-name>/
│   ├── RESUME.md     # Current state (session recovery)
│   ├── WORKFLOW.md   # Process definition
│   ├── task.yaml     # Task metadata
│   └── docs/         # Documentation
└── ...

scripts/              # Utility scripts
dashboard/            # Token usage dashboard (optional)
```

## Key Concepts

### Agents (`.kiro/agents/`)

Each agent is a JSON file defining:
- `name` / `description`
- `prompt` — system prompt (inline or `file://` reference)
- `tools` — available tools
- `resources` — files/skills loaded into context
- `hooks.agentSpawn` — commands run on session start (e.g., load RESUME.md)

### Shared Context (`.kiro/shared/SHARED-CONTEXT.md`)

Information shared across all agents: environment info, credentials references, team members, common URLs.

### Learned Memories (`.kiro/learned/LEARNED.md`)

Auto-appended experience entries. Format:
```markdown
### [YYYY-MM-DD] AgentName — Title
Brief description (1-3 lines)
```

### Skills (`.kiro/skills/`)

Reusable instruction documents that agents reference. Examples:
- `auto-learn.md` — when/how to record learnings
- `delegate-to-local-llm.md` — rules for delegating to local models
- `output-templates.md` — commit message, MR description formats

### Tasks (`tasks/`)

Each task has:
- `RESUME.md` — current state, read on session start for continuity
- `WORKFLOW.md` — process steps, variables, stage definitions
- `task.yaml` — metadata (optional)

## Usage

1. Copy this template to your workspace
2. Edit `.kiro/shared/SHARED-CONTEXT.md` with your environment info
3. Create agent definitions in `.kiro/agents/`
4. Define tasks in `tasks/<task-name>/`
5. Run `kiro-cli chat --agent <name>` to start working

## Session Recovery

Agents read `RESUME.md` on spawn via hooks. This ensures continuity across sessions without relying on conversation history.

## AI-DLC Integration

This template includes [AI-DLC (AI-Driven Development Life Cycle)](https://github.com/awslabs/aidlc-workflows) v0.1.8 rules.

AI-DLC provides a structured three-phase workflow:
- **Inception** — Requirements analysis, user stories, architecture design
- **Construction** — Detailed design, code generation, testing
- **Operations** — Deployment and monitoring

Rules are in English but all interaction is in Chinese (configured via `steering/locale-override.md`).

### Quick Start Prompt

```
AI-DLC を使って、<your project description here>
```

The `locale-override.md` steering file ensures all documents, questions, and responses are in Chinese with JST timestamps.

### Generated Artifacts

AI-DLC generates working documents in `aidlc-docs/` (gitignored):
- `aidlc-state.md` — Current workflow state and progress
- `audit.md` — Timestamped work log
- Requirements, user stories, design docs, etc.

### Verify Setup

Run `kiro-cli`, then `/context show`, and confirm entries for `.kiro/steering/aws-aidlc-rules` and `.kiro/steering/locale-override.md`.

## License

MIT
