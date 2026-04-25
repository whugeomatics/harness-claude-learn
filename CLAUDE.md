# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Claude Code plugin project that enhances Spring Boot development workflows. The plugin provides automated git workflows, Java code quality enforcement, and security scanning capabilities.

## Plugin Architecture

- **Plugin Type**: Claude Code extension plugin
- **Entry Point**: `.claude-plugin/plugin.json`
- **Skills Directory**: `skills/` - Contains custom skills for enhanced development workflows
- **Hooks Directory**: `scripts/` - Contains shell scripts for automated tool events

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

## Plugin Development

### Building and Testing
- No traditional build system - this is a plugin project
- Test by activating the plugin in Claude Code
- Installation: Deploy to Claude Code plugin directory
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
- Plugin configuration is in `.claude-plugin/` directory
- Skills and scripts can be modified independently
- Always test skills in development environment before deployment
- Run `/verify` after making changes to configuration files
- Use `/test` to validate all functionality after updates