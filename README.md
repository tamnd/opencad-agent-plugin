# OpenCAD agent plugin

This plugin connects Claude Code and Codex to [OpenCAD](https://github.com/tamnd/opencad). It gives the MCP server `opencadd mcp`, the skills `opencad-design` and `opencad-ocl`, and three optional hooks. The agent can build, check, render and measure OCL models with it.

Do not change this repository. Its source is in `plugin/` and `skills/` of the OpenCAD repository, and each release of OpenCAD makes it again.

## Install in Claude Code

```text
/plugin marketplace add tamnd/opencad-agent-plugin
/plugin install opencad@opencad
```

## Install in Codex

```sh
codex plugin marketplace add tamnd/opencad-agent-plugin
codex plugin add opencad@opencad
```

## The server

The plugin starts the server through `bin/opencad-launch`. The launcher runs the program in `OPENCAD_BIN` when it is set. If not, it runs `opencadd` on the `PATH` when its version is the version of the plugin. If not, it downloads the release binary for your system into `~/.cache/opencad/bin` and checks its SHA-256 against `bin/opencadd.sha256` before it runs it.

## Hooks

The hooks ask you before an edit that changes `requirements.toml` or makes a `requires` line weaker, and before a call that sends data out of the machine. They also tell the agent to build after it changes an OCL file. In Claude Code they act only when the variable `OPENCAD_HOOKS` is `1`. In Codex they act when you trust them.

## Not yet

- The launcher does not run on Windows. Install `opencadd` and add the server by hand.
- The release binaries have no geometry kernel yet.

See the [plugin docs](https://github.com/tamnd/opencad/blob/main/docs/tools/plugin.md) for more.
