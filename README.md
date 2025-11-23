# Claude Skills Library

A curated library of Claude Code skills for enhanced development workflows.

## What are Claude Skills?

Claude Skills are **model-invoked capability extensions** that augment Claude's abilities by packaging specialized instructions, expertise, and tools into discoverable, reusable components. Unlike slash commands that require explicit invocation, skills are autonomous—Claude automatically discovers and uses them when relevant to your request.

## Skills in This Library

### Spec-Driven Development
**Location:** `.claude/skills/spec-driven-development/`

Expert skill for implementing a comprehensive spec-driven development workflow. This skill helps you:

1. **Generate Specifications** - Create clear, concise `spec.md` files that fully define features
2. **Create Technical Plans** - Build detailed `plan.md` files with implementation strategies
3. **Break Down Complex Projects** - Organize work into logical phases with `phase-task.md` files

**When to use:**
- Starting new features or projects
- Designing system architecture
- Planning complex implementations
- Creating development roadmaps

**Example usage:**
```
"I need to build a user authentication system. Help me create a spec for it."
"The spec is approved. Create a technical plan for implementation."
"Break down Phase 1 into detailed tasks."
```

See [skill documentation](.claude/skills/spec-driven-development/SKILL.md) and [examples](.claude/skills/spec-driven-development/templates/) for more details.

## How to Use These Skills

### Option 1: Use in This Repository
Clone this repository and work directly within it. All skills will be automatically available to Claude Code when you open this project.

```bash
git clone <your-repo-url>
cd claude-skills
# Open Claude Code in this directory
```

### Option 2: Copy to Your Projects
Copy the entire `.claude/skills/` directory (or individual skills) to your own projects:

```bash
# Copy all skills to your project
cp -r /path/to/claude-skills/.claude/skills /path/to/your-project/.claude/

# Or copy individual skills
cp -r /path/to/claude-skills/.claude/skills/spec-driven-development /path/to/your-project/.claude/skills/
```

### Option 3: Symlink for Reuse
Create a symlink to use skills across multiple projects without duplication:

```bash
# In your project directory
mkdir -p .claude/skills
ln -s /path/to/claude-skills/.claude/skills/spec-driven-development .claude/skills/spec-driven-development
```

## Skill Organization

```
.claude/skills/
├── spec-driven-development/
│   ├── SKILL.md                       # Main skill definition
│   └── templates/
│       ├── example-spec.md            # Example specification
│       ├── example-plan.md            # Example technical plan
│       └── example-phase-1-task.md    # Example phase tasks
│
└── [future-skill-name]/
    ├── SKILL.md
    └── ...
```

Each skill follows the standard structure:
- **SKILL.md** - Main skill file with YAML frontmatter + markdown instructions
- **templates/** - Example files and reference materials (optional)
- **scripts/** - Executable scripts for the skill (optional)
- **REFERENCE.md** - Additional documentation (optional)

## Creating Your Own Skills

### Basic Structure

Every skill needs a `SKILL.md` file with two parts:

1. **YAML Frontmatter** (required)
```yaml
---
name: "skill-name"
description: "Clear description of what this skill does and when to use it"
allowed-tools: "Read,Write,Bash(git:*)"  # Optional
version: "1.0.0"  # Optional
---
```

2. **Markdown Instructions**
```markdown
# Skill Name

Detailed instructions for Claude on how to execute this skill.

## When to Use
...

## Process
...
```

### Key Principles

1. **Good Descriptions** - The description is crucial for Claude to discover when to use your skill:
   - Include WHAT the skill does AND WHEN to use it
   - Write in third person
   - Be specific about use cases
   - Keep under 1024 characters

2. **Clear Instructions** - Write step-by-step guidance for Claude:
   - Be specific and unambiguous
   - Include examples
   - Define expected outputs
   - Handle edge cases

3. **Progressive Disclosure** - Show just enough info:
   - Frontmatter enables discovery
   - Main content provides core instructions
   - Reference files add deeper details

4. **Minimal Permissions** - Only request tools you need:
   ```yaml
   allowed-tools: "Read,Write"  # File operations only
   allowed-tools: "Read,Write,Bash(git:*)"  # Add scoped git commands
   ```

### Example: Simple Git Helper Skill

```yaml
---
name: "git-commit-helper"
description: "Generate descriptive commit messages by analyzing git diffs. Use when the user asks for help writing commit messages or reviewing staged changes."
allowed-tools: "Read,Bash(git:*)"
---

# Git Commit Message Helper

This skill analyzes git diffs and generates clear commit messages.

## Process

1. Run `git diff --cached` to examine staged changes
2. Analyze the changes to understand modifications
3. Generate a commit message following conventional commit format
4. Suggest the message to the user

## Format

`<type>(<scope>): <description>`

Types: feat, fix, docs, refactor, test
```

## Best Practices

### For Skill Authors

- **Test Thoroughly** - Verify skills work with real use cases
- **Document Well** - Include examples and clear instructions
- **Keep Focused** - Each skill should have a single, clear purpose
- **Version Your Skills** - Use semantic versioning in frontmatter
- **Provide Examples** - Add template files showing expected outputs

### For Skill Users

- **Trust Claude** - Let Claude discover and use skills autonomously
- **Provide Context** - Mention relevant keywords to trigger skill usage
- **Iterate** - Work with Claude to refine outputs from skills
- **Combine Skills** - Multiple skills can work together on complex tasks

## Contributing New Skills

To add a new skill to this library:

1. Create a new directory under `.claude/skills/[skill-name]/`
2. Add a `SKILL.md` file with proper frontmatter and instructions
3. Include templates or examples if helpful
4. Test the skill thoroughly
5. Update this README with skill documentation
6. Commit and push your changes

## Resources

- [Claude Skills Documentation](https://docs.claude.com/en/docs/agents-and-tools/agent-skills)
- [Skill Authoring Best Practices](https://docs.claude.com/en/docs/agents-and-tools/agent-skills/best-practices)
- [Anthropic Skills Repository](https://github.com/anthropics/skills) - Official examples
- [Claude Code Documentation](https://code.claude.com/docs)

## License

[Add your license here]

## Feedback and Issues

Have feedback or found an issue? [Create an issue](../../issues) in this repository.
