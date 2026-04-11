# Copilot Atlas

Copilot Atlas is a suite of specialized agents for planning, implementation, and review. The canonical agent definitions live under `agents/` and are supported by two installation targets:

- **Claude Code plugin** — installed via `/plugin` using the bundled marketplace metadata in `.claude-plugin/`
- **VS Code GitHub Copilot plugin** — installed as an agent plugin via the Command Palette or Extensions view

## What's included

| Agent | Role |
|---|---|
| **Atlas** | Orchestration and phase coordination |
| **Prometheus** | Research and implementation planning |
| **Oracle** | Deep codebase research |
| **Explorer** | Fast file and dependency discovery |
| **Sisyphus** | Implementation with tests-first discipline |
| **Code Review** | Review of completed changes |
| **Frontend Engineer** | UI, styling, and responsive work |

---

## Installing in Claude Code

### From the marketplace (recommended)

```text
/plugin marketplace add https://github.com/bigguy345/Github-Copilot-Atlas
/plugin install copilot-atlas@copilot-atlas-marketplace
/reload-plugins
```

### From a local clone

```text
git clone https://github.com/bigguy345/Github-Copilot-Atlas
claude --plugin-dir ./Github-Copilot-Atlas
```

### Usage in Claude Code

After installation, open `/agents` to pick a specialized agent, or let Claude invoke them automatically when the task matches their specialty.

---

## Installing in VS Code (GitHub Copilot)

VS Code supports [agent plugins](https://code.visualstudio.com/docs/copilot/customization/agent-plugins) — the easiest way to install Copilot Atlas globally across all workspaces.

> **Note:** Agent plugins are currently in preview. Make sure `chat.plugins.enabled` is set to `true` in your VS Code settings.

### Install from source (recommended)

1. Open the Command Palette (`Ctrl+Shift+P` / `⇧⌘P`) and run **Chat: Install Plugin From Source**.
2. Enter the repository URL:
   ```
   https://github.com/bigguy345/Github-Copilot-Atlas
   ```
3. VS Code clones and installs the plugin automatically — no manual file copying needed.

### Install from the marketplace

1. Open the Extensions view (`Ctrl+Shift+X` / `⇧⌘X`) and search for `@agentPlugins`.
2. Find **Copilot Atlas** and select **Install**.

### Use a local clone

If you've already cloned the repo, register it via `settings.json`:

```json
"chat.pluginLocations": {
    "/path/to/Github-Copilot-Atlas": true
}
```

### Usage in VS Code

Open the Copilot Chat panel and switch to **Agent mode**. The Atlas agents appear in the agent selector. Select the one that matches your task, or type `@Atlas` to start with the orchestrator.

> **Tip:** VS Code checks for plugin updates automatically every 24 hours, or run **Extensions: Check for Extension Updates** to update manually.

---

## Repository layout

```
agents/                  # Canonical agent definitions (shared by both targets)
.claude-plugin/          # Claude Code marketplace and plugin metadata
```

## License

MIT

## Acknowledgments

This project builds on ideas from copilot-orchestra and oh-my-opencode.
