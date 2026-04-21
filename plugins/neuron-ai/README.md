# neuron-fe-ai

> Neuron FE AI — skills, slash commands, and subagents for the Neuron component library, distributed in VS Code's Copilot agent-plugin format.

A GitHub Copilot CLI / VS Code Copilot agent plugin that ships everything needed to build frontend features on top of the Neuron Framework and the Neuron UI library used at AIS-Servis / Vienna Insurance Group.

- **Version:** 1.0.0
- **Author:** Neuron FE
- **License:** UNLICENSED (internal)

## What's inside

| Component   | Count | Location      |
| ----------- | ----: | ------------- |
| Slash commands | 80 | `commands/`   |
| Skills      |    45 | `skills/`     |
| Subagents   |    10 | `agents/`     |
| MCP servers |     3 | `.mcp.json`   |

### Slash commands (`/neuron-ui-*`)

Per-component integration guides for every Neuron UI primitive — buttons, inputs, layouts, overlays, tables, charts, navigation, and more. Invoke with `/neuron-ui-<component>` (e.g. `/neuron-ui-button`, `/neuron-ui-table`, `/neuron-ui-datepicker`).

### Subagents

Specialist agents orchestrated by `neuron-frontend-dev`:

- `neuron-frontend-dev` — top-level orchestrator for Neuron frontend work
- `neuron-style-architect` — styling, theming, design tokens
- `neuron-css-writer` — CSS/SCSS implementation
- `neuron-layout-builder` — layouts and responsive structure
- `neuron-form-builder` — forms with validation
- `neuron-token-mapper` — Figma → token mapping
- `neuron-i18n` — internationalization
- `neuron-router` — routing and navigation
- `neuron-performance` — React performance optimization
- `neuron-validator` — form/data validation rules

### Skills

Loaded on-demand for focused tasks:

- **Neuron framework**: `neuron-api`, `neuron-auth`, `neuron-redux`, `neuron-hooks`, `neuron-i18n`, `neuron-pages-routing`, `neuron-project-folder-structure`, `neuron-package-creation`, `neuron-unit-testing`, `neuron-code-review`, `neuron-code-tables`, `neuron-commit-message`, `neuron-create-mr`, `neuron-form-composition-and-validation`, `neuron-content-helmet`, `neuron-content-layout-system`, `neuron-content-styling`
- **Neuron UI**: `neuron-ui-buttons`, `neuron-ui-composites`, `neuron-ui-data`, `neuron-ui-error-handling`, `neuron-ui-file`, `neuron-ui-form`, `neuron-ui-helpers`, `neuron-ui-icons`, `neuron-ui-layout`, `neuron-ui-menu`, `neuron-ui-messages`, `neuron-ui-misc`, `neuron-ui-overlays`, `neuron-ui-panels`, `neuron-ui-popups`
- **Figma**: `figma-use`, `figma-code-connect`, `figma-create-design-system-rules`, `figma-create-new-file`, `figma-generate-design`, `figma-generate-library`, `figma-implement-design`, `neuron-figma-mcp`
- **Integrations & meta**: `confluence-integration`, `jira-integration`, `make-blog-post`, `find-skills`, `skill-creator`

### MCP servers

Configured in `.mcp.json` and started automatically when the plugin is enabled:

- `context7` — up-to-date library documentation
- `chrome-devtools` — browser DevTools automation
- `figma` — Figma file access

## Installation

### From the marketplace repo

```bash
copilot plugin marketplace add luko248/neuron-ai
copilot plugin install neuron-fe-ai@neuron-fe-ai
```

### Direct install from the repo

```bash
copilot plugin install luko248/neuron-ai:plugins/neuron-ai
```

### From a local clone

```bash
copilot plugin install ./plugins/neuron-ai
```

## Usage

After installation, run `copilot` in your project and:

- Type `/` to browse the slash commands (filter by `neuron-ui-`).
- Run `/agent` to pick a subagent (e.g. `neuron-frontend-dev`).
- Run `/skills` to enable/disable skills for the session.
- Run `/mcp` to inspect or toggle the bundled MCP servers.

## Repository layout

```
plugins/neuron-ai/
├── plugin.json     # Plugin manifest
├── .mcp.json       # MCP server definitions
├── agents/         # Subagent definitions
├── commands/       # Slash command prompts
└── skills/         # Skill packages (each with SKILL.md)
```

The marketplace manifest lives at `.github/plugin/marketplace.json` in the repo root.
