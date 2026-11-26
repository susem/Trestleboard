# Quick Start Guide

This guide will help you get started with Trestleboard and the AI-powered personas.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Choose Your AI CLI](#choose-your-ai-cli)
  - [Claude Code](#claude-code-anthropic)
  - [Gemini CLI](#gemini-cli-google)
  - [GitHub Copilot CLI](#github-copilot-cli)
- [Setting Up Trestleboard](#setting-up-trestleboard)
- [Your First Project](#your-first-project)
- [Using Each Persona](#using-each-persona)
- [Tips and Best Practices](#tips-and-best-practices)

---

## Prerequisites

- **Node.js** 18+ for Trestleboard's own tools (the AI CLIs below may need Node.js 22+ when installed via npm)
- **Git** (for version control with Paired Developer)
- **An account or API key** for your chosen AI provider
- **Pandoc and a PDF engine** (only for `tb-export-pdf`; see [Pandoc installation](https://pandoc.org/installing.html))

---

## Choose Your AI CLI

Trestleboard is **AI-agnostic**. Choose whichever CLI tool works best for you:

| CLI | Provider | Best For |
|-----|----------|----------|
| Claude Code | Anthropic | Deep reasoning, long context |
| Gemini CLI | Google | Google Cloud integration |
| GitHub Copilot CLI | GitHub/Microsoft | VS Code users, GitHub integration |

> **Important:** Trestleboard is designed for **AI CLI tools** that can load system prompts and access the filesystem. Run your CLI from the Trestleboard repository root so the personas can read `personas/`, `templates/` and `projects/`. Web interfaces can use the personas but require manual artifact handling.

---

## Claude Code (Anthropic)

### Installation

**Option 1: Native installer (macOS, Linux, WSL)**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Option 2: Homebrew (macOS)**
```bash
brew install --cask claude-code
```

**Option 3: npm (Node.js 22+)**
```bash
npm install -g @anthropic-ai/claude-code
```

### Verify Installation

```bash
claude --version
```

### Authentication

```bash
# Interactive login (opens browser)
claude auth login

# Or set environment variable (API billing)
export ANTHROPIC_API_KEY=your-api-key-here
```

### Usage with Trestleboard

```bash
# Start with a persona (appended to Claude Code's default system prompt)
claude --append-system-prompt-file personas/analyst.md

# Switch personas by starting a new session with a different file
claude --append-system-prompt-file personas/paired-developer.md
```

`--append-system-prompt-file` keeps Claude Code's built-in file, shell and git guidance, which
the personas rely on. `--system-prompt-file` replaces the default prompt entirely. Note that
`--system-prompt` (without `-file`) takes literal text, not a file path.

**Documentation:** [Claude Code CLI reference](https://code.claude.com/docs/en/cli-reference)

---

## Gemini CLI (Google)

### Installation

**Option 1: npm**
```bash
npm install -g @google/gemini-cli
```

**Option 2: Homebrew (macOS/Linux)**
```bash
brew install gemini-cli
```

### Verify Installation

```bash
gemini --version
```

### Authentication

```bash
# Run gemini and choose "Login with Google" on first launch
gemini

# Or set an API key
export GEMINI_API_KEY=your-api-key-here
```

### Usage with Trestleboard

Gemini CLI has no flag that appends a system prompt. Load the persona inside the session:

```
gemini
> Read personas/analyst.md and adopt it as your persona for this session.
```

Or use a persona as the full system prompt with the `GEMINI_SYSTEM_MD` environment variable:

```bash
GEMINI_SYSTEM_MD=personas/analyst.md gemini
```

`GEMINI_SYSTEM_MD` **replaces** Gemini CLI's built-in system prompt, including its tool-use
instructions, so the in-session approach is usually the safer choice.

**Documentation:** [Gemini CLI docs](https://geminicli.com/docs) ·
[System prompt override](https://geminicli.com/docs/cli/system-prompt/)

---

## GitHub Copilot CLI

### Installation

Requires an active GitHub Copilot subscription.

```bash
# npm (Node.js 22+)
npm install -g @github/copilot

# Homebrew (macOS/Linux)
brew install copilot-cli

# WinGet (Windows)
winget install GitHub.Copilot
```

### Verify Installation

```bash
copilot version
```

### Authentication

```bash
copilot login
# Or start `copilot` and use the /login slash command
```

### Usage with Trestleboard

Copilot CLI has no system-prompt flag. Load the persona inside the session:

```
copilot
> Read personas/analyst.md and adopt it as your persona for this session.
```

For repeated use, Copilot CLI supports
[custom agents](https://docs.github.com/en/copilot/how-tos/copilot-cli/use-copilot-cli/overview):
Markdown agent profiles in `.github/agents/` or `~/.copilot/agents/`, invoked with `/agent`
or `copilot --agent=<name>`.

The older `gh copilot` extension (`gh copilot explain` / `suggest`) has been replaced by the
standalone Copilot CLI and does not accept a system prompt. You can also paste persona content
into **GitHub Copilot Chat** in VS Code (Ctrl+Shift+I / Cmd+Shift+I).

**Documentation:** [GitHub Copilot CLI docs](https://docs.github.com/en/copilot/how-tos/copilot-cli)

---

## Quick Comparison

| Feature | Claude Code | Gemini CLI | Copilot CLI |
|---------|-------------|------------|-------------|
| Load a persona | `--append-system-prompt-file file.md` | Ask in session, or `GEMINI_SYSTEM_MD=file.md` | Ask in session, or a custom agent |
| Interactive mode | Yes | Yes | Yes |
| File and shell access | Yes | Yes | Yes |

---

## Setting Up Trestleboard

### 1. Clone the Repository

```bash
git clone <repository-url>
cd Trestleboard
```

### 2. Explore the Structure

```bash
ls -la
```

You should see:
```
personas/           # AI persona system prompts
templates/          # Mustache templates for artifacts
schemas/            # JSON schemas for validation
projects/           # Your project workspaces
README.md
QUICKSTART.md
```

### 3. Install Dependencies

```bash
npm install
```

This installs the template renderer and validation tools.

### 4. Create Your First Project

```bash
# Using the init script (recommended)
npm run init -- my-first-project --description "My first Trestleboard project"

# Or manually
mkdir -p projects/my-first-project/{input,output/{inception-deck,specification,development}}
```

---

## Your First Project

### Step 1: Start with the Analyst

The Analyst helps you build an Agile Inception Deck through discovery conversations.

**Choose your CLI:**
```bash
# Claude Code
claude --append-system-prompt-file personas/analyst.md

# Gemini CLI or GitHub Copilot CLI: start `gemini` or `copilot`, then say
#   "Read personas/analyst.md and adopt it as your persona for this session."
```

**What to expect:**
- The Analyst will greet you and ask about your project
- You'll be guided through 10 sections of the Inception Deck
- Answer questions about your problem, solution, stakeholders, risks, etc.

**Output:**
- `projects/<project>/output/inception-deck/inception-deck.md`
- `projects/<project>/output/inception-deck/inception-deck.json`
- Copies placed in `projects/<project>/input/` for next persona

### Step 2: Move to Solution Architect

Once your Inception Deck is complete, engage the Solution Architect.

**Choose your CLI:**
```bash
# Claude Code
claude --append-system-prompt-file personas/solution-architect.md

# Gemini CLI or GitHub Copilot CLI: start `gemini` or `copilot`, then say
#   "Read personas/solution-architect.md and adopt it as your persona for this session."
```

**What to expect:**
- The Solution Architect reads your Inception Deck from `input/`
- You'll create use cases, C4 diagrams, wireframes, and ADRs
- Any contradictions with the Inception Deck will be flagged

**Output:**
- `projects/<project>/output/specification/specification.md`
- `projects/<project>/output/specification/specification.json`
- Copies placed in `projects/<project>/input/` for next persona

### Step 3: Implement with Paired Developer

With specifications complete, start implementation.

**Choose your CLI:**
```bash
# Claude Code
claude --append-system-prompt-file personas/paired-developer.md

# Gemini CLI or GitHub Copilot CLI: start `gemini` or `copilot`, then say
#   "Read personas/paired-developer.md and adopt it as your persona for this session."
```

**What to expect:**
- The Paired Developer reads both Inception Deck and Specification
- Work is broken into the smallest possible increments
- Strict TDD: tests first, then implementation
- Each change goes through a PR for your approval
- Demos after each wave of value delivery

**Output:**
- Working, tested code
- `projects/<project>/output/development/task-breakdown.md`
- Code review checklists for each PR

---

## Using Each Persona

### Working with the Analyst

**Good prompts:**
- "I want to build a project management tool for small teams"
- "We're creating an API for integrating with payment providers"
- "Help me define a mobile app for tracking fitness goals"

**The Analyst will ask about:**
- The problem you're solving
- Who experiences the problem
- What success looks like
- Who the stakeholders are
- What's explicitly out of scope
- Risks and constraints

### Working with the Solution Architect

**Good prompts:**
- "Let's start with the use cases"
- "Show me the C4 context diagram"
- "What technology should we use for the database?"
- "I need to document why we chose REST over GraphQL"

**The Solution Architect will:**
- Reference your Inception Deck constantly
- Create Mermaid diagrams for visualization
- Document architectural decisions as ADRs
- Flag any contradictions with upstream artifacts

### Working with the Paired Developer

**Good prompts:**
- "Let's break down the user authentication feature"
- "Write a failing test for the login endpoint"
- "I'm ready to review the PR"
- "Show me a demo of what we've built"

**The Paired Developer will:**
- Split work into 1-3 point increments
- Always write tests before implementation
- Create a feature branch for each change
- Wait for your PR approval before merging
- Offer demos after completing value waves

---

## Rendering Artifacts

After collecting data through conversations with personas, render final artifacts:

### Render a Template

```bash
# Render inception deck
npm run render -- \
  --template templates/inception-deck/inception-deck.md \
  --data projects/my-project/output/inception-deck/inception-deck.json \
  --output projects/my-project/output/inception-deck/inception-deck.md

# Render specification
npm run render -- \
  --template templates/specification/specification.md \
  --data projects/my-project/output/specification/specification.json \
  --output projects/my-project/output/specification/specification.md
```

### Validate Data Before Rendering

```bash
# Validate your JSON data against the schema
npm run validate -- --data inception-deck.json --schema inception-deck

# With verbose error output
npm run validate -- --data specification.json --schema specification --verbose
```

### Template Helpers

The renderer includes Handlebars helpers for common patterns:

| Helper | Usage | Description |
|--------|-------|-------------|
| `{{formatDate date}}` | Format ISO date | `2024-01-15` |
| `{{#eq a b}}...{{/eq}}` | Equality check | Conditional block |
| `{{join array ", "}}` | Join array | `a, b, c` |
| `{{sum array "points"}}` | Sum property | Total story points |
| `{{checkbox checked}}` | Markdown checkbox | `[x]` or `[ ]` |
| `{{riskLevel score}}` | Risk level | High/Medium/Low |

---

## Tips and Best Practices

### General Tips

1. **Be specific** - The more context you provide, the better the output
2. **Iterate** - Don't expect perfection on the first pass
3. **Review artifacts** - Check the generated markdown and JSON files
4. **Use the templates** - They ensure consistency across projects

### For Discovery (Analyst)

- Bring any existing documentation to the conversation
- Don't skip the NOT List - explicit exclusions prevent scope creep
- Identify all stakeholders early, even skeptics

### For Design (Solution Architect)

- Start with C4 Context before diving into details
- Create ADRs for any significant technical decision
- Use Mermaid diagrams - they render directly in GitHub

### For Development (Paired Developer)

- **Embrace small increments** - If a task feels big, split it
- **Trust TDD** - The tests will guide your design
- **Review every PR** - This is your quality gate
- **Request demos** - See working software frequently

### Contradiction Handling

If a persona flags a contradiction:
1. Don't ignore it
2. Go back to the upstream persona
3. Resolve the conflict explicitly
4. Update the affected artifacts
5. Continue with the downstream work

---

## Web Interfaces & IDEs

The personas work with any AI interface. Here's how to use them:

### Web Chat Interfaces

| Interface | How to Load Persona |
|-----------|---------------------|
| **Claude.ai** | Paste: "Please adopt this persona: [contents]" |
| **ChatGPT** | Paste: "Use this as your system prompt: [contents]" |
| **Gemini Web** | Paste: "Follow these instructions: [contents]" |
| **Perplexity** | Paste persona as context in first message |

### IDE Integrations

| IDE | AI Tool | How to Use |
|-----|---------|------------|
| **VS Code** | GitHub Copilot Chat | Paste persona in chat, or set as custom instructions |
| **VS Code** | Continue.dev | Add persona to `.continue/config.json` |
| **Cursor** | Built-in AI | Set persona in AI settings or paste in chat |
| **JetBrains** | AI Assistant | Paste persona in chat window |
| **Zed** | Claude Integration | Configure in settings |

### Custom Instructions Setup

Some tools support persistent custom instructions:

**GitHub Copilot (VS Code):**
1. Open Settings → GitHub Copilot
2. Find "Custom Instructions"
3. Paste persona content

**Continue.dev:**
```json
// .continue/config.json
{
  "systemMessage": "contents of persona file here"
}
```

**Cursor:**
1. Open Settings → AI
2. Paste persona in "Custom Instructions"

---

## Troubleshooting

### Claude Code Issues

**CLI not found:**
```bash
# Run a health check (if the command is found)
claude doctor

# For npm installs, check that npm's global bin directory is on your PATH
export PATH="$PATH:$(npm prefix -g)/bin"
```

**Authentication issues:**
```bash
# Re-authenticate
claude auth logout
claude auth login
```

### Gemini CLI Issues

**CLI not found:**
```bash
# Reinstall
npm install -g @google/gemini-cli
```

**Authentication issues:**
- Inside `gemini`, run `/auth` to change the sign-in method, or set `GEMINI_API_KEY`

### GitHub Copilot CLI Issues

**CLI not found:**
```bash
# Reinstall or update
npm install -g @github/copilot@latest
```

**Authentication issues:**
```bash
copilot login
```

### General Issues

**Persona not loading correctly:**
- Ensure you're using the full path to the persona file
- Check that the file exists: `cat personas/analyst.md`
- Verify file is valid UTF-8 markdown

**System instruction too long:**
- Some CLIs have token limits for system prompts
- Try using a web interface for very long personas
- Consider splitting the persona into sections

---

## Next Steps

1. **Complete your first Inception Deck** with the Analyst
2. **Create a technical specification** with the Solution Architect
3. **Implement a small feature** with the Paired Developer
4. **Customize the templates** to match your team's needs
5. **Create project-specific personas** if needed

---

## Getting Help

- Check the [README.md](README.md) for an overview
- Review the persona files in `personas/` for detailed behavior
- Examine the templates in `templates/` for output format
- Validate against schemas in `schemas/` for data structure

Happy building! 🏗️
