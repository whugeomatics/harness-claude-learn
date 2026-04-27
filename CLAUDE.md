# CLAUDE.md

This file provides guidance for this Claude Code-first plugin repository. Codex can reuse the shared skills through `.codex-plugin/plugin.json`, but Codex hooks and custom agents require separate Codex configuration.

## Project Overview

This is a Claude Code-first AI coding plugin for Spring Boot development workflows. It provides automated git checks, conventional commit generation, Java executor dependency guidance, Java code quality enforcement, security scanning, session summaries, and a Java code review subagent.

Codex support is intentionally narrower: `.codex-plugin/plugin.json` exposes the shared `skills/` directory so Codex can reuse the core skill instructions. Claude Code hooks and Claude-style subagents are not automatically installed by the Codex plugin manifest.

## Plugin Architecture

- **Primary Plugin Type**: Claude Code extension plugin
- **Claude Code Entry Point**: `.claude-plugin/plugin.json`
- **Codex Entry Point**: `.codex-plugin/plugin.json` for skill reuse
- **Codex Marketplace**: `.agents/plugins/marketplace.json`
- **Skills Directory**: `skills/` - Contains custom skills for enhanced development workflows
- **Agents Directory**: `agents/` - Contains Claude-style subagent definitions
- **Hooks Directory**: `hooks/` and `scripts/` - Contains Claude hook metadata and reusable shell scripts

Keep shared skill behavior in `skills/`. Keep Claude-specific hooks/subagents in the existing Claude-oriented structure. If Codex needs equivalent hooks or agents, add `.codex/hooks.json`, `.codex/config.toml`, or `.codex/agents/*.toml` according to Codex's own configuration model instead of adding unsupported fields to `.codex-plugin/plugin.json`.

## Development Workflow

### Skills System
The plugin provides the following skills:
- `/git-commit-check`: Validates git repository state and enforces conventional commits
- `/executor-dependency`: Manages thread pool dependencies for Java projects
- `/git-commit`: Internal skill for generating conventional commit messages
- `/verify`: Validates plugin configuration and skill definitions
- `/test`: Runs integration tests for plugin functionality

### Hook System
Automated hooks run on specific tool events:
- **PreToolUse (Bash)**: `scripts/command-guard.sh` - Blocks dangerous commands
- **PostToolUse (Write/Edit)**: 
  - `scripts/java-lint.sh` - Runs quality checks on Java files
  - `scripts/secret-scan.sh` - Scans for hardcoded credentials
- **Stop**: `scripts/session-summary.sh` - Logs session summaries

Current `hooks/hooks.json` uses Claude Code hook naming and `${CLAUDE_PLUGIN_ROOT}`. Codex plugin manifests do not configure hooks. To reuse these scripts in Codex, create separate Codex hook configuration under `.codex/` or `~/.codex/` and adapt the event payload as needed.

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
