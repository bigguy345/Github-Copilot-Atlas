# Copilot Agent Orchestra

A multi-agent orchestration system for VS Code Copilot that enables complex software development workflows through intelligent agent delegation and parallel execution.

## Overview

This repository contains custom agent prompts that work together to handle the complete software development lifecycle: **Planning → Implementation → Review → Commit**. The system uses a conductor-delegate pattern where a main orchestrator (Atlas) coordinates specialized subagents to efficiently tackle complex development tasks.

## Architecture

### Primary Agents

- **Atlas** (`Atlas.agent.md`) - The CONDUCTOR
  - Orchestrates the full development lifecycle
  - Delegates to specialized subagents for research, implementation, and review
  - Manages context conservation and parallel execution
  - Handles phase tracking and user approval gates

- **Prometheus** (`Prometheus.agent.md`) - The AUTONOMOUS PLANNER
  - Researches requirements and analyzes codebases
  - Writes comprehensive TDD-driven implementation plans
  - Automatically hands off to Atlas for execution
  - Supports parallel research across multiple subsystems

### Specialized Subagents

- **Oracle-subagent** (`Oracle-subagent.agent.md`) - THE RESEARCHER
  - Gathers comprehensive context about tasks
  - Can delegate to Explorer for large-scope research
  - Returns structured findings to parent agents
  - Supports parallel research across independent subsystems

- **Sisyphus-subagent** (`Sisyphus-subagent.agent.md`) - THE IMPLEMENTER
  - Executes implementation following strict TDD principles
  - Writes tests first, then minimal code to pass
  - Handles linting and formatting
  - Can be invoked in parallel for disjoint features

- **Explorer-subagent** (`Explorer-subagent.agent.md`) - THE SCOUT
  - Rapid file/usage discovery across codebases
  - Read-only exploration (no edits/commands)
  - Returns structured results with file lists and analysis
  - MANDATORY parallel search strategy (3-10 simultaneous searches)

- **code-review-subagent** (`code-review-subagent.agent.md`) - THE REVIEWER
  - Reviews code for correctness, quality, and test coverage
  - Returns structured feedback (APPROVED/NEEDS_REVISION/FAILED)
  - Can be invoked in parallel for independent phases
  - Focus on blocking issues vs nice-to-haves

- **frontend-engineer-subagent** (`frontend-engineer-subagent.agent.md`) - THE UI/UX SPECIALIST
  - Implements user interfaces, styling, and responsive layouts
  - Expert in modern frontend frameworks and tooling
  - Follows TDD principles for frontend (component tests first)
  - Focuses on accessibility and responsive design

## Key Features

### 🔄 Parallel Agent Execution
- Launch multiple subagents simultaneously for independent tasks
- Explorer: 3-10 parallel searches in first batch
- Oracle: Parallel research across multiple subsystems
- Sisyphus: Parallel implementation for disjoint features
- Maximum 10 parallel agents per phase

### 💾 Context Conservation
- Intelligent delegation to preserve context window
- Subagents handle heavy lifting (exploration, research, implementation)
- Conductor synthesizes findings without reading everything
- Clear guidelines on when to delegate vs handle directly

### 🧪 Test-Driven Development
- Every phase follows red-green-refactor cycle
- Tests written first, run to fail, then minimal code
- Explicit test → code → test steps in all plans
- No manual testing unless explicitly requested

### 🤝 Proper Agent Handoffs
- VS Code Custom Agent handoff configuration
- Each agent declares available delegations
- Prometheus → Atlas automatic handoff option
- Clear handoff workflow with user approval gates

### 📋 Structured Planning
- Atlas-compatible plan format
- 3-10 incremental, self-contained phases
- Open questions with options/recommendations
- Risk assessment and mitigation strategies

## Directory Structure

```
prompts/
├── README.md                          # This file
├── Atlas.agent.md                     # Main conductor agent
├── Prometheus.agent.md                # Autonomous planner agent
├── Oracle-subagent.agent.md           # Research subagent
├── Sisyphus-subagent.agent.md         # Implementation subagent
├── Explorer-subagent.agent.md         # Exploration subagent
├── code-review-subagent.agent.md      # Review subagent
├── frontend-engineer-subagent.agent.md # Frontend specialist subagent
└── subagents/                         # Alternative subagent versions
    ├── Oracle-subagent.agent.md
    ├── Sisyphus-subagent.agent.md
    ├── Explorer-subagent.agent.md
    ├── code-review-subagent.agent.md
    └── frontend-engineer-subagent.agent.md
```

## Installation

1. **Clone this repository:**
   ```bash
   git clone https://github.com/YOUR-USERNAME/copilot-agent-orchestra.git
   ```

2. **Copy agent files to VS Code User prompts directory:**
   - **Windows:** `%APPDATA%\Code - Insiders\User\prompts\`
   - **macOS:** `~/Library/Application Support/Code - Insiders/User/prompts/`
   - **Linux:** `~/.config/Code - Insiders/User/prompts/`

3. **Reload VS Code** to recognize the new agents

## Usage

### Planning a Feature with Prometheus

```
@Prometheus Plan a comprehensive implementation for adding user authentication to the app
```

Prometheus will:
1. Research the codebase (delegating to Explorer/Oracle as needed)
2. Write a detailed TDD plan with 3-10 phases
3. Offer to invoke Atlas automatically or let you review first

### Executing a Plan with Atlas

```
@Atlas Implement the plan in .sisyphus/plans/user-authentication-plan.md
```

Atlas will:
1. Review the plan
2. Delegate Phase 1 implementation to Sisyphus
3. Delegate review to code-review-subagent
4. Present results and wait for commit approval
5. Continue through all phases

### Direct Research with Oracle

```
@Oracle-subagent Research how the networking system works in this mod
```

Oracle will:
1. Delegate to Explorer for file discovery (if >10 files)
2. Analyze key files and patterns
3. Return structured findings

### Quick Exploration with Explorer

```
@Explorer-subagent Find all files related to GuiScriptInterface
```

Explorer will:
1. Launch 3-10 parallel searches immediately
2. Read necessary files to confirm relationships
3. Return structured results with file list and analysis

## Workflow Example

```
User: @Prometheus Plan adding autocomplete feature to script editor

Prometheus:
  ├─ @Explorer-subagent (find script editor files)
  ├─ @Oracle-subagent (research syntax highlighter)
  ├─ @Oracle-subagent (research autocomplete providers)
  └─ Writes plan → Offers to invoke Atlas

User: Yes, invoke Atlas

Prometheus:
  └─ @Atlas Implement the plan...

Atlas: Phase 1/4 - Test Infrastructure
  └─ @Sisyphus-subagent Implement Phase 1
      ├─ Writes tests (fail)
      ├─ Writes minimal code
      └─ Tests pass ✓

Atlas: Reviewing Phase 1
  └─ @code-review-subagent Review Phase 1
      └─ Status: APPROVED ✓

Atlas: Phase 1 complete! [commit message provided]
```

## Configuration

### Plan Directory
Agents check for plan directory configuration:
1. Look for `AGENTS.md` file in workspace
2. Find plan directory specification (e.g., `.sisyphus/plans`)
3. Default to `plans/` if not specified

### Tool Requirements
All agents declare their required tools in YAML frontmatter:
- `agent` - For delegating to other agents
- `edit` - File editing capabilities
- `search` - Semantic/grep search
- `runCommands/runTasks` - Terminal execution
- etc.

### Handoff Declarations
Agents declare available delegations:
```yaml
handoff:
  - agent: Oracle-subagent
    description: Delegate research
  - agent: Sisyphus-subagent
    description: Delegate implementation
```

## Requirements

- VS Code Insiders (with GitHub Copilot)
- GitHub Copilot subscription
- VS Code Custom Agents feature enabled
- Multi-agent orchestration support

## Best Practices

1. **Use Prometheus for complex features** - Let it research and plan before implementation
2. **Leverage parallel execution** - Invoke multiple Explorers/Oracles for large tasks
3. **Trust the TDD workflow** - Each phase is self-contained with tests
4. **Review before proceeding** - Check completed phases before moving forward
5. **Commit frequently** - After each approved phase
6. **Delegate appropriately** - Let subagents handle heavy lifting

## Contributing

Contributions are welcome! Please:
1. Test agent changes thoroughly
2. Maintain the orchestration architecture
3. Follow TDD principles in implementation agents
4. Keep plan format compatible with Atlas
5. Document any new agents or workflows

## License

MIT License - See LICENSE file for details

## Acknowledgments

Built for complex software development tasks that benefit from:
- Structured planning and execution
- Parallel research and implementation
- Domain-specific agent expertise
- Context-aware delegation strategies

---

**Note:** These agents are designed to work together. While individual agents can be used standalone, the full power comes from Atlas orchestrating the complete workflow with intelligent delegation and parallel execution.
