# Every Marketplace - Claude Code Plugin Marketplace

This repository is a Claude Code plugin marketplace that distributes the `compounding-engineering` plugin to developers building with AI-powered tools.

**Note:** This is the enhanced fork (tommy-ca/every-marketplace) that serves as the authoritative source for the compounding-engineering plugin with comprehensive standardization improvements. The upstream repository (EveryInc/every-marketplace) is tracked for reference but all development occurs in this fork.

## Repository Structure

```
every-marketplace/
├── .claude-plugin/
│   └── marketplace.json          # Marketplace catalog (lists available plugins)
└── plugins/
    └── compounding-engineering/   # The actual plugin
        ├── .claude-plugin/
        │   └── plugin.json        # Plugin metadata
        ├── agents/                # 17 specialized AI agents
        ├── commands/              # 6 slash commands
        ├── skills/                # 3 git worktree skills
        └── README.md              # Plugin documentation
```

## Philosophy: Compounding Engineering

**Each unit of engineering work should make subsequent units of work easier—not harder.**

When working on this repository, follow the compounding engineering process:

1. **Plan** → Understand the change needed and its impact
2. **Delegate** → Use AI tools to help with implementation
3. **Assess** → Verify changes work as expected
4. **Codify** → Update this CLAUDE.md with learnings

## Working with This Repository

### Adding a New Plugin

1. Create plugin directory: `plugins/new-plugin-name/`
2. Add plugin structure:
   ```
   plugins/new-plugin-name/
   ├── .claude-plugin/plugin.json
   ├── agents/
   ├── commands/
   └── README.md
   ```
3. Update `.claude-plugin/marketplace.json` to include the new plugin
4. Test locally before committing

### Updating the Compounding Engineering Plugin

When agents or commands are added/removed:

1. **Scan for actual files:**

   ```bash
   # Count agents
   ls plugins/compounding-engineering/agents/*.md | wc -l

   # Count commands
   ls plugins/compounding-engineering/commands/*.md | wc -l
   ```

2. **Update plugin.json** at `plugins/compounding-engineering/.claude-plugin/plugin.json`:

   - Update `components.agents` count
   - Update `components.commands` count
   - Update `agents` object to reflect which agents exist
   - Update `commands` object to reflect which commands exist

3. **Update plugin README** at `plugins/compounding-engineering/README.md`:

   - Update agent/command counts in the intro
   - Update the agent/command lists to match what exists

4. **Update marketplace.json** at `.claude-plugin/marketplace.json`:
   - Usually doesn't need changes unless changing plugin description/tags

### Marketplace.json Structure

The marketplace.json follows the official Claude Code spec:

```json
{
  "name": "marketplace-identifier",
  "owner": {
    "name": "Owner Name",
    "url": "https://github.com/owner"
  },
  "metadata": {
    "description": "Marketplace description",
    "version": "1.0.0"
  },
  "plugins": [
    {
      "name": "plugin-name",
      "description": "Plugin description",
      "version": "1.0.0",
      "author": { ... },
      "homepage": "https://...",
      "tags": ["tag1", "tag2"],
      "source": "./plugins/plugin-name"
    }
  ]
}
```

**Only include fields that are in the official spec.** Do not add custom fields like:

- `downloads`, `stars`, `rating` (display-only)
- `categories`, `featured_plugins`, `trending` (not in spec)
- `type`, `verified`, `featured` (not in spec)

### Plugin.json Structure

Each plugin has its own plugin.json with detailed metadata:

```json
{
  "name": "plugin-name",
  "version": "1.0.0",
  "description": "Plugin description",
  "author": { ... },
  "keywords": ["keyword1", "keyword2"],
  "components": {
    "agents": 15,
    "commands": 6,
    "hooks": 2
  },
  "agents": {
    "category": [
      {
        "name": "agent-name",
        "description": "Agent description",
        "use_cases": ["use-case-1", "use-case-2"]
      }
    ]
  },
  "commands": {
    "category": ["command1", "command2"]
  }
}
```

## Testing Changes

### Test Locally

1. Install the marketplace locally:

   ```bash
   claude /plugin marketplace add /Users/yourusername/every-marketplace
   ```

2. Install the plugin:

   ```bash
   claude /plugin install compounding-engineering
   ```

3. Test agents and commands:
   ```bash
   claude /review
   claude agent kieran-rails-reviewer "test message"
   ```

### Validate JSON

Before committing, ensure JSON files are valid:

```bash
cat .claude-plugin/marketplace.json | jq .
cat plugins/compounding-engineering/.claude-plugin/plugin.json | jq .
```

## Common Tasks

### Adding a New Agent

1. Create `plugins/compounding-engineering/agents/new-agent.md`
2. Update plugin.json agent count and agent list
3. Update README.md agent list
4. Test with `claude agent new-agent "test"`

### Adding a New Command

1. Create `plugins/compounding-engineering/commands/new-command.md`
2. Update plugin.json command count and command list
3. Update README.md command list
4. Test with `claude /new-command`

### Updating Tags/Keywords

Tags should reflect the compounding engineering philosophy:

- Use: `ai-powered`, `compounding-engineering`, `workflow-automation`, `knowledge-management`
- Avoid: Framework-specific tags unless the plugin is framework-specific

## Commit Conventions

Follow these patterns for commit messages:

- `Add [agent/command name]` - Adding new functionality
- `Remove [agent/command name]` - Removing functionality
- `Update [file] to [what changed]` - Updating existing files
- `Fix [issue]` - Bug fixes
- `Simplify [component] to [improvement]` - Refactoring

Include the Claude Code footer:

```
🤖 Generated with [Claude Code](https://claude.com/claude-code)

Co-Authored-By: Claude <noreply@anthropic.com>
```

## Resources to search for when needing more information

- [Claude Code Plugin Documentation](https://docs.claude.com/en/docs/claude-code/plugins)
- [Plugin Marketplace Documentation](https://docs.claude.com/en/docs/claude-code/plugin-marketplaces)
- [Plugin Reference](https://docs.claude.com/en/docs/claude-code/plugins-reference)

## Key Learnings

_This section captures important learnings as we work on this repository._

### 2025-10-09: Simplified marketplace.json to match official spec

The initial marketplace.json included many custom fields (downloads, stars, rating, categories, trending) that aren't part of the Claude Code specification. We simplified to only include:

- Required: `name`, `owner`, `plugins`
- Optional: `metadata` (with description and version)
- Plugin entries: `name`, `description`, `version`, `author`, `homepage`, `tags`, `source`

**Learning:** Stick to the official spec. Custom fields may confuse users or break compatibility with future versions.

### 2025-11-10: Comprehensive standardization using Codex-driven review

Executed a multi-phase standardization using Codex skill (gpt-5-codex with high reasoning effort) to review and improve all 17 agents, 6 commands, and documentation. Results:

**Phase 1: Agent Standardization** (17 agents)
- Standardized YAML frontmatter across all agents using block scalars (|) instead of inline escapes
- Normalized examples to structured array format with context/user/assistant/commentary keys
- Created TEMPLATE.md as canonical agent structure reference
- All agents validated with 100% YAML compliance

**Phase 2: Command Standardization** (6 commands)
- Applied consistent Goal → Prerequisites → Workflow → Success Criteria → Troubleshooting structure
- Fixed typos (Runn→Run, paralel→parallel in plan.md)
- Enhanced namespace consistency (all use /compounding-engineering: prefix)
- Added concrete usage examples where missing
- Preserved all functionality while improving clarity

**Phase 3: README Documentation**
- Documented all 10 previously undocumented agents with use cases and examples
- Added GitHub CLI prerequisite section (critical for /review and /triage)
- Added GitHub CLI Setup troubleshooting (installation, authentication, errors)
- Enhanced command reference table with time estimates and detailed context

**Phase 4: Compounding Philosophy**
- Added "Compounding Engine" section with concrete Week 1→4→12 timeline showing 80% velocity improvement
- Demonstrated how standards catalog, todos, patterns, and architecture knowledge compound
- Made implicit compounding promise explicit and measurable
- Updated Table of Contents to reflect new section

**Key Improvements:**
- All 17 agents now have detailed README documentation with examples
- Consistent structure makes maintenance easier
- Explicit GitHub CLI requirements prevent setup issues
- Concrete velocity metrics make compounding promise tangible
- Knowledge assets (standards catalog, todos, patterns) clearly identified

**Learning:** A comprehensive review cycle using high-reasoning Codex analysis reveals opportunities for standardization, documentation gaps, and philosophical clarity that improve user experience and maintenance burden. Timeline progressions (Week 1/4/12) are more compelling than abstract promises of compounding value.
