# AGENTS.md

This repository is a Claude Code-first plugin. Codex can reuse the shared skills, but it does not automatically receive the Claude hooks or Claude-style subagents.

Read `CLAUDE.md` for the full project guidance. Codex-related entry points:

- `.codex-plugin/plugin.json` is the Codex plugin manifest for skill reuse.
- `.agents/plugins/marketplace.json` is the Codex marketplace entry.
- Shared skills live in `skills/`.
- Claude-style subagents live in `agents/`; Codex custom agents require `.codex/agents/*.toml` or `~/.codex/agents/*.toml`.
- Claude hook metadata lives in `hooks/hooks.json`; Codex hooks require `.codex/hooks.json`, `.codex/config.toml`, `~/.codex/hooks.json`, or `~/.codex/config.toml`.
- Reusable hook scripts live in `scripts/`, but their input payload may need adaptation before use in Codex.

When changing docs or plugin metadata, keep the Claude-first positioning clear. Do not describe Codex as having full plugin parity unless the required Codex hooks/agents configuration is also added.

## Development Workflow

### Skills System
The plugin provides the following skills:
- `/git-commit-check`: Validates git repository state and enforces conventional commits
- `/executor-dependency`: Manages thread pool dependencies for Java projects
- `/git-commit`: Internal skill for generating conventional commit messages
- `/verify`: Validates plugin configuration and skill definitions
- `/test`: Runs integration tests for plugin functionality

## Plugin Development

### Building and Testing
- No traditional build system - this is a plugin project
- Test complete behavior by activating the plugin in Claude Code
- Test Codex skill reuse when changing `skills/` or `.codex-plugin/plugin.json`
- Claude installation: deploy through `.claude-plugin/plugin.json` / Claude marketplace
- Codex installation: deploy through `.codex-plugin/plugin.json` / `.agents/plugins/marketplace.json`; hooks and custom agents need manual Codex configuration
- Use `/verify` to check configuration syntax
- Use `/test` to run integration tests

### Code Style
- Bash scripts: Use strict mode (`set -euo pipefail`)
- JSON files: Standard formatting
- Skills: YAML frontmatter + Markdown body format

## GitHub Integration
The project uses GitHub for version control. Consider installing GitHub CLI (`gh`) for enhanced GitHub workflow integration.

## Important Notes

- Skills are invoked with `/skill-name` syntax
- Hooks run automatically on tool events
- Claude plugin configuration is in `.claude-plugin/`
- Codex plugin configuration is in `.codex-plugin/` and currently exposes shared skills
- Codex marketplace configuration is in `.agents/plugins/marketplace.json`
- Skills and scripts can be modified independently
- Always test shared skills in both Claude Code and Codex when they are changed
- Do not add unsupported `hooks` or `agents` fields to `.codex-plugin/plugin.json`
- Run `/verify` after making changes to configuration files
- Use `/test` to validate all functionality after updates
