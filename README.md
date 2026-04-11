# Copilot Atlas

Copilot Atlas is packaged as a Claude Code plugin with a companion marketplace so it can be installed through `/plugin`. The canonical agent definitions now live once under `agents/`; the Claude Code marketplace metadata is in `.claude-plugin/`, and the same agent catalog can also be reused for VS Code Copilot agent packaging.

## What’s included

- **Atlas** - orchestration and phase coordination
- **Prometheus** - research and implementation planning
- **Oracle** - deep codebase research
- **Explorer** - fast file and dependency discovery
- **Sisyphus** - implementation with tests-first discipline
- **Code Review** - review of completed changes
- **Frontend Engineer** - UI, styling, and responsive work

## Install from the marketplace

Add the marketplace, then install the plugin:

```text
/plugin marketplace add <path-or-github-repo>
/plugin install copilot-atlas@copilot-atlas-marketplace
/reload-plugins
```

For local development, you can also load the repo directly:

```text
claude --plugin-dir .
```

## Repository layout

- `.claude-plugin/` - marketplace and plugin metadata
- `agents/` - canonical agent definitions used by the Claude Code plugin and reusable for VS Code Copilot packaging

## Usage

After installation, open `/agents` in Claude Code to pick the specialized agent you want, or let Claude invoke them automatically when the task matches their specialty.

## License

MIT

## Acknowledgments

This project builds on ideas from copilot-orchestra and oh-my-opencode.
