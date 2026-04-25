---
name: test
description: Run integration tests for the plugin to verify all functionality works correctly
---

This skill runs comprehensive tests to validate the plugin's functionality and ensure all components work as expected.

## Usage
Run `/test` to execute:
- Basic skill functionality tests
- Hook script execution verification
- Skill dependency checks
- Output test reports and recommendations

## Process
1. Test each skill (`/git-commit-check`, `/executor-dependency`, `/git-commit`)
2. Verify hook scripts execute without errors
3. Check skill interdependencies and cross-references
4. Generate test report with any issues found