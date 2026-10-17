# Project templates

These are the files that `opencadd init` writes in a new project, from section 3 of spec/ai/mcp/10-skills-plugins-distribution.md. `opencadd init` does not exist yet, so copy them by hand.

- AGENTS.md has the rules for any agent. It stays under 8 KiB, because a host loads it on each turn. Put the values of your project in place of `<process>`, `<material>`, `<toolsets>` and `<name>`. The line about `opencadd verify` names a command of a later release.
- CLAUDE.md has one line, `@AGENTS.md`, because Claude Code reads CLAUDE.md and not AGENTS.md.

examples/bracket/AGENTS.md is this template with the values of the bracket, and with the lines of the later release taken out.
