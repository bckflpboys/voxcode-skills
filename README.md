# VoxCode Skills 🧠⚡

A curated, modular library of high-performance skills for AI coding agents and LLM models.

Every skill lives in its own standalone directory. Users, teams, and applications can **cherry-pick individual skills** or **install the entire repository** with zero friction.

---

## 🌟 Philosophy: Universal Access & Modularity

Different AI apps, agent frameworks, and developer environments consume instructions differently. To ensure seamless adoption everywhere, every skill in this repository provides **multi-format entry points**:

| Format | File | Target Environment |
| :--- | :--- | :--- |
| **Agentic Standard** | `SKILL.md` | Antigravity, Claude Code, Cline, Roo Code, OpenHands |
| **Universal Self-Contained** | `README.md` | Web apps, prompt libraries, documentation loaders, GitHub |
| **Direct System Prompt** | `prompt.md` | ChatGPT (Custom GPTs), Claude Projects, System Prompts |
| **Programmatic JSON** | `skill.json` | API registries, LangChain, LlamaIndex, automated installers |

> **Universal Ingestion**: If an app or service only supports importing a single markdown file or URL, simply import the skill's `README.md`. Each skill's `README.md` is 100% self-contained with full protocols, checklists, and examples.

---

## 📦 Available Skills

| Skill | Category | Description | Links |
| :--- | :--- | :--- | :--- |
| **Flow Simulation** (`flow-simulation`) | Reliability & Verification | Forces AI models to trace end-to-end execution routes, analyze blast radius, mentally simulate workflows, and verify regressions before and after code changes. | [Folder](./flow-simulation) • [Doc](./flow-simulation/README.md) • [Prompt](./flow-simulation/prompt.md) |

*(More skills coming soon...)*

---

## 🚀 Installation & Usage Guide

### Option 1: Install an Individual Skill

If you only want a specific skill (e.g., `flow-simulation`), copy that single directory into your project:

#### For Antigravity / Agentic IDEs:
```bash
# In your target project root
mkdir -p .agents/skills/
cp -r /path/to/voxcode-skills/flow-simulation .agents/skills/
```

#### For User-Global Availability (Antigravity):
```bash
cp -r /path/to/voxcode-skills/flow-simulation ~/.gemini/config/skills/
```

#### For Cursor / Windsurf:
Reference the skill in your `.cursorrules` or `.windsurfrules`:
```markdown
# Flow Simulation Rule
Read and strictly adhere to: ./skills/flow-simulation/README.md
```

#### For ChatGPT / Claude Projects:
1. Open the skill's folder (e.g., [`flow-simulation/`](./flow-simulation)).
2. Open [`prompt.md`](./flow-simulation/prompt.md).
3. Copy the text block and paste it directly into your Custom GPT or Claude Project instructions.

---

### Option 2: Install All Skills

Clone or copy the entire repository into your workspace or global skills directory:

```bash
# Clone directly into your project's .agents directory
git clone https://github.com/voxcode/skills.git .agents/skills/voxcode-skills

# Or copy into global agent configuration
cp -r voxcode-skills/* ~/.gemini/config/skills/
```

---

### Option 3: Programmatic Consumption (LangChain / Python / Node.js)

Each skill contains a `skill.json` manifest and clean markdown files ready for dynamic prompt injection:

```typescript
// TypeScript / Node.js
import fs from 'fs';
import path from 'path';

export function loadSkillPrompt(skillName: string): string {
  const promptPath = path.join(__dirname, skillName, 'prompt.md');
  return fs.readFileSync(promptPath, 'utf8');
}
```

```python
# Python
from pathlib import Path
import json

def load_skill_manifest(skill_dir: str) -> dict:
    manifest = Path(skill_dir) / "skill.json"
    return json.loads(manifest.read_text(encoding="utf-8"))
```

---

## 📁 Repository Structure

```text
voxcode-skills/
├── README.md                           # Repository overview & installation guide
├── skills.json                         # Global registry manifest of all skills
│
├── flow-simulation/                    # Skill: Flow Simulation
│   ├── SKILL.md                        # Agentic standard skill file with frontmatter
│   ├── README.md                       # Complete self-contained docs & prompt
│   ├── prompt.md                       # Direct copy-paste system prompt
│   ├── skill.json                      # Machine-readable JSON manifest
│   ├── references/
│   │   ├── flow-tracing-checklist.md   # Step-by-step route tracing checklist
│   │   └── simulation-techniques.md    # Mental simulation & state machine guide
│   ├── examples/
│   │   ├── signup-flow-walkthrough.md  # Detailed auth & signup walkthrough
│   │   └── payment-checkout-flow.md    # Checkout & webhook race condition example
│   └── resources/
│       └── flow-diagram-templates.md   # ASCII & Mermaid reporting templates
│
└── [future-skills]/                    # Future modular skills follow the same architecture
```

---

## 🛠️ Adding a New Skill

To contribute a new skill:
1. Create a new directory `<skill-name>/`.
2. Provide `SKILL.md` (with YAML frontmatter `name` and `description`).
3. Provide a self-contained `README.md`, `prompt.md`, and `skill.json`.
4. Include supporting references (`references/`), practical examples (`examples/`), and templates (`resources/`).
5. Register the skill in the root `skills.json` and `README.md`.

---

## 📄 License

This repository is licensed under the [MIT License](LICENSE).
