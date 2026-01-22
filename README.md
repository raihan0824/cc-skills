# Claude Code & OpenCode Skills

This repository contains reusable skills for Claude Code and OpenCode that extend the capabilities of your AI coding assistant with specialized workflows and domain knowledge.

## What Are Skills?

Skills are extensions that add custom capabilities to Claude Code and OpenCode. They allow you to:

- Create custom slash commands (e.g., `/slides`)
- Provide domain-specific knowledge and conventions
- Automate workflows and tasks
- Share reusable functionality across projects

Skills follow the [Agent Skills](https://agentskills.io) open standard and work across multiple AI tools.

## Available Skills

### Slides

Create professional HTML slide presentations from reference materials.

**Usage:**
```
/slides [input-files]
```

**Features:**
- Converts docs, PDFs, images, and markdown into clean HTML slides
- Dark modern aesthetic with CSS animations
- Keyboard navigation (←/→, Space, Enter)
- Animated charts and counters
- Fully self-contained HTML (no external dependencies)

## Installation

### For Claude Code

Skills can be installed at different scopes:

#### Option 1: Personal (Available in All Projects)

Install skills globally for your user account:

```bash
# Create the skills directory
mkdir -p ~/.claude/skills

# Clone or copy the skill
cp -r /path/to/cc-skills/slides ~/.claude/skills/
```

#### Option 2: Project-Specific

Install skills only for a specific project:

```bash
# Navigate to your project
cd /path/to/your/project

# Create the skills directory
mkdir -p .claude/skills

# Clone or copy the skill
cp -r /path/to/cc-skills/slides .claude/skills/
```

#### Option 3: Direct Clone

Clone this repository directly into your skills directory:

```bash
# For personal use
cd ~/.claude/skills
git clone https://github.com/raihan0824/cc-skills.git

# For project use
cd /path/to/your/project/.claude/skills
git clone https://github.com/raihan0824/cc-skills.git
```

### For OpenCode

OpenCode searches multiple locations for skills:

#### Option 1: Global Installation

```bash
# Create the skills directory
mkdir -p ~/.config/opencode/skills

# Clone or copy the skill
cp -r /path/to/cc-skills/slides ~/.config/opencode/skills/
```

#### Option 2: Project-Specific

```bash
# Navigate to your project
cd /path/to/your/project

# Create the skills directory
mkdir -p .opencode/skills

# Clone or copy the skill
cp -r /path/to/cc-skills/slides .opencode/skills/
```

#### Option 3: Claude-Compatible Paths

OpenCode also supports Claude Code paths:

```bash
# Global (Claude-compatible)
mkdir -p ~/.claude/skills
cp -r /path/to/cc-skills/slides ~/.claude/skills/

# Project (Claude-compatible)
mkdir -p .claude/skills
cp -r /path/to/cc-skills/slides .claude/skills/
```

## Using Skills

### Manual Invocation

Call a skill directly using the slash command:

```
/slides presentation-notes.md company-logo.png
```

### Automatic Invocation

Claude Code can automatically invoke skills based on your request:

```
Create a presentation about our Q4 results
```

Claude will recognize the task matches the slides skill and invoke it automatically.

### With Arguments

Pass arguments to skills:

```
/slides --theme dark project-summary.md
```

## Skill Priority

When the same skill exists in multiple locations, the following priority order applies:

1. **Project skills** (`.claude/skills/` or `.opencode/skills/`)
2. **Personal skills** (`~/.claude/skills/` or `~/.config/opencode/skills/`)
3. **Enterprise skills** (managed by organization)

Project-level skills override personal skills with the same name.

## Permission Control

### Claude Code

Restrict skill access in `/permissions`:

```bash
# Deny all skills
Skill

# Allow specific skills
Skill(slides)

# Deny specific skills
Skill(deploy:*)
```

### OpenCode

Control permissions in `opencode.json`:

```json
{
  "permissions": {
    "skills": {
      "slides": "allow",
      "deploy": "deny",
      "internal-*": "ask"
    }
  }
}
```

Options:
- `allow`: Loads immediately
- `deny`: Hidden from agents
- `ask`: Prompts user for approval

## Creating Your Own Skills

### Basic Structure

Every skill requires a `SKILL.md` file:

```
my-skill/
├── SKILL.md           # Main instructions (required)
├── assets/            # Optional: templates, images
├── examples/          # Optional: example output
└── scripts/           # Optional: utility scripts
```

### Minimal SKILL.md

```yaml
---
name: my-skill
description: What this skill does and when to use it
---

# My Skill

Instructions for Claude to follow when this skill is invoked.

## Workflow

1. Step one
2. Step two
3. Step three
```

### Required Frontmatter

- **name**: Lowercase, hyphens only, 1-64 characters
- **description**: When to use this skill, 1-1024 characters

### Optional Frontmatter

```yaml
---
name: my-skill
description: What this skill does
argument-hint: [file-path]           # Autocomplete hint
disable-model-invocation: true       # Only manual invocation
user-invocable: false                # Only Claude can invoke
allowed-tools: Read, Grep            # Restrict tool access
model: claude-opus                   # Custom model
context: fork                        # Run in isolated context
agent: Explore                       # Subagent type
---
```

## Troubleshooting

### Skill Not Found

**Claude Code:**
- Verify the skill is in `~/.claude/skills/<skill-name>/SKILL.md` or `.claude/skills/<skill-name>/SKILL.md`
- Check that `SKILL.md` filename is uppercase
- Run `/context` to see loaded skills

**OpenCode:**
- Check `~/.config/opencode/skills/` or `.opencode/skills/`
- Verify you're in a git repository for project-local skills
- Ensure frontmatter has required `name` and `description` fields

### Skill Not Triggering Automatically

- Make the `description` field more specific with clear keywords
- Try manual invocation with `/skill-name`
- Check if `disable-model-invocation: true` is set

### Skill Triggers Too Often

- Make the description more specific
- Add `disable-model-invocation: true` to require manual invocation

## Resources

- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills)
- [OpenCode Skills Documentation](https://opencode.ai/docs/skills/)
- [Agent Skills Standard](https://agentskills.io)

## Contributing

To add new skills to this repository:

1. Create a new directory with your skill name
2. Add a `SKILL.md` file with proper frontmatter
3. Include examples and documentation
4. Test with both Claude Code and OpenCode
5. Submit a pull request

## License

[Add your license here]
