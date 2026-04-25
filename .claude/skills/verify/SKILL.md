---
name: verify
description: Verify plugin configuration and skill definitions for syntax correctness
---

This skill validates the plugin configuration and all skill definitions to ensure they are properly formatted and syntactically correct.

## Usage
Run `/verify` to check:
- plugin.json configuration syntax
- All skill YAML frontmatter format
- Hook script syntax and permissions
- Cross-references between skills

## Process
1. Check `.claude-plugin/plugin.json` for valid JSON structure
2. Validate each skill's YAML frontmatter in `skills/*/SKILL.md`
3. Verify hook scripts are executable and syntactically correct
4. Report any issues found with suggested fixes