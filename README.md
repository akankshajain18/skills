# Adobe Skills for AI Coding Agents

Repository of Adobe skills for AI coding agents.

## Installation

### Claude Code Plugins

```bash
# Add the Adobe Skills marketplace
/plugin marketplace add adobe/skills

# Install AEM Edge Delivery Services plugin (all 17 skills)
/plugin install aem-edge-delivery-services@adobe-skills

# Install AEM Project Management plugin (6 skills)
/plugin install aem-project-management@adobe-skills

# Install App Builder plugin (6 skills)
/plugin install app-builder@adobe-skills

# Install all AEM as a Cloud Service skills (create-component + workflow + dispatcher) in one command
/plugin install aem-cloud-service@adobe-skills

# Install all AEM 6.5 LTS skills (workflow + dispatcher) in one command
/plugin install aem-6-5-lts@adobe-skills
```

### Vercel Skills (npx skills)

```bash
# Install all AEM Edge Delivery Services skills
npx skills add https://github.com/adobe/skills/tree/main/skills/aem/edge-delivery-services --all

# Install all App Builder skills
npx skills add https://github.com/adobe/skills/tree/main/skills/app-builder --all

# Install all AEM as a Cloud Service skills (create-component + workflow + dispatcher) in one command
npx skills add https://github.com/adobe/skills/tree/beta/skills/aem/cloud-service --all

# Install all AEM 6.5 LTS skills (workflow + dispatcher) in one command
npx skills add https://github.com/adobe/skills/tree/beta/skills/aem/6.5-lts --all

# Install for a single agent (pick ONE flavor only)
npx skills add https://github.com/adobe/skills/tree/beta/skills/aem/cloud-service -a cursor -y
npx skills add https://github.com/adobe/skills/tree/beta/skills/aem/6.5-lts -a cursor -y

# Install specific skill(s)
npx skills add adobe/skills -s content-driven-development
npx skills add adobe/skills -s content-driven-development building-blocks testing-blocks

# List available skills
npx skills add adobe/skills --list
npx skills add https://github.com/adobe/skills/tree/beta/skills/aem/cloud-service --list
npx skills add https://github.com/adobe/skills/tree/beta/skills/aem/6.5-lts --list
```

### upskill (GitHub CLI Extension)

```bash
gh extension install trieloff/gh-upskill

# Install all AEM Edge Delivery Services skills
gh upskill adobe/skills --path skills/aem/edge-delivery-services --all

# Install all AEM as a Cloud Service skills (create-component + workflow + dispatcher)
gh upskill adobe/skills --path skills/aem/cloud-service --all

# Install all AEM 6.5 LTS skills (workflow + dispatcher)
gh upskill adobe/skills --path skills/aem/6.5-lts --all

# Install a specific skill
gh upskill adobe/skills --path skills/aem/edge-delivery-services --skill content-driven-development

# List available skills
gh upskill adobe/skills --path skills/aem/edge-delivery-services --list
gh upskill adobe/skills --path skills/aem/cloud-service --list
gh upskill adobe/skills --path skills/aem/6.5-lts --list
```

## Available Skills

### AEM Edge Delivery Services

This package provides three capability areas:
- Core development workflow skills
- Discovery and documentation lookup skills
- Migration and import workflow skills

See `skills/aem/edge-delivery-services/skills/` for the current concrete skill set.

### AEM as a Cloud Service — Create Component

The `create-component` skill creates complete AEM components following Adobe best practices for AEM Cloud Service and AEM 6.5. It covers:

- Component definition, dialog XML, and HTL template
- Sling Model and optional child item model (multifield)
- Unit tests for models and servlets
- Clientlibs (component and dialog)
- Optional Sling Servlet for dynamic content

See `skills/aem/cloud-service/skills/create-component/` for the skill and its reference files.

### AEM as a Cloud Service — Ensure AGENTS.md (bootstrap)

The `ensure-agents-md` skill is a **bootstrap skill** that runs first, before any other work. When a
customer opens their AEM Cloud Service project and asks the agent anything, this skill checks whether
`AGENTS.md` exists at the repo root. If missing, it:

- Reads root `pom.xml` to resolve the project name and discover actual modules
- Detects add-ons (CIF, Forms, SPA type, precompiled scripts)
- Generates a tailored `AGENTS.md` with only the modules that exist, correct frontend variant, conditional
  Dispatcher MCP section, and the right resource links
- Creates `CLAUDE.md` (`@AGENTS.md`) so Claude-based tools also discover the guidance

If `AGENTS.md` already exists it is never overwritten.

See `skills/aem/cloud-service/skills/ensure-agents-md/` for the skill, template, and module catalog.

### AEM Workflow

Workflow skills cover the full AEM Granite Workflow Engine lifecycle — from designing and implementing workflows to production debugging and incident triaging. Like Dispatcher, they are split by runtime flavor:

- `skills/aem/cloud-service/skills/aem-workflow` — Cloud Service variant (no JMX, Cloud Manager logs, pipeline deploy)
- `skills/aem/6.5-lts/skills/aem-workflow` — 6.5 LTS / AMS variant (JMX, Felix Console, direct log access)

Each flavor contains the same specialist sub-skills:

| Sub-Skill | Purpose |
|---|---|
| `workflow-model-design` | Design workflow models, step types, OR/AND splits, variables |
| `workflow-development` | Implement WorkflowProcess steps, ParticipantStepChooser, OSGi services |
| `workflow-triggering` | Start workflows from UI, code, HTTP API, or Manage Publication |
| `workflow-launchers` | Configure automatic workflow launchers on JCR events |
| `workflow-debugging` | Debug stuck, failed, or stale workflows in production |
| `workflow-triaging` | Classify incidents, determine log patterns, Splunk queries |
| `workflow-orchestrator` | Full lifecycle orchestration across all sub-skills |

### AEM Dispatcher

Dispatcher skills are split by runtime flavor to avoid mode auto-detection and keep installation explicit.
Install only one dispatcher flavor in a workspace (`cloud-service` or `6.5-lts`).

Current dispatcher flavors:
- `skills/aem/cloud-service/skills/dispatcher`
- `skills/aem/6.5-lts/skills/dispatcher`

Each flavor contains parallel capability groups (workflow orchestration, config authoring, technical advisory, incident response, performance tuning, and security hardening).
Shared advisory logic is centralized under each flavor's `dispatcher/shared/references/` to reduce duplication and drift.

### AEM as a Cloud Service — Best Practices & Migration

Under `skills/aem/cloud-service/skills/`, **`best-practices/`** is the **general-purpose** Cloud Service skill: pattern modules, Java baseline references (SCR→OSGi DS, resolver/logging, and related refs), and day-to-day Cloud Service alignment. Use it **without** loading **migration** for greenfield or maintainability work. **`migration/`** (BPA/CAM orchestration) is **scoped to legacy AEM → AEM as a Cloud Service** (not Edge Delivery or 6.5 LTS); it **delegates** concrete refactors to **`best-practices`** (`references/`). **Installing the AEM as a Cloud Service plugin** (`aem-cloud-service`, or the `skills/aem/cloud-service` path with `npx skills` / `gh upskill`) **includes both**; the agent should load the appropriate `SKILL.md` for the task. Use **`gh upskill` / `npx skills` with `--skill`** when you need a specific bundled skill (see **Installation** above).

**Key features:**
- **Best practices:** one skill for patterns, SCR→OSGi DS, and resolver/logging — applicable to Cloud Service projects generally, not only migration
- **Migration:** orchestration-only; pattern and transformation content lives in **`best-practices`**

### AEM Project Management

Project lifecycle management for AEM Edge Delivery Services including handover documentation, PDF generation, and authentication.

> **Requirement:** This plugin is exclusively for AEM Edge Delivery Services projects. It validates projects by checking for `scripts/aem.js`. For non-Edge Delivery projects, the plugin exits early — use standard documentation approaches instead.

**Quick Start:**
```bash
cd your-edge-delivery-project   # or any subdirectory within it
# Say: "create handover documentation for this project"
```

**Setup:** You will be prompted for your Config Service organization name (the `{org}` in `https://main--site--{org}.aem.page`). A browser window will open for authentication — sign in and **close the browser window** to continue.

**Permissions:** Admin access to the project organization is required. The plugin queries the Config Service API to gather project configuration, site settings, and access controls for comprehensive documentation.

**Output:** Professional PDFs generated in `project-guides/` folder:
- `project-guides/AUTHOR-GUIDE.pdf` - For content authors
- `project-guides/DEVELOPER-GUIDE.pdf` - For developers
- `project-guides/ADMIN-GUIDE.pdf` - For administrators

| Skill | Description |
|-------|-------------|
| `handover` | Orchestrates project documentation generation |
| `authoring` | Generate comprehensive authoring guide for content authors |
| `development` | Generate technical documentation for developers |
| `admin` | Generate admin guide for site administrators |
| `whitepaper` | Create professional PDF whitepapers from Markdown |
| `auth` | Authenticate with AEM Config Service API |

### App Builder

Development, customization, testing, and deployment skills for Adobe App Builder projects.

**Skill chaining:**
- **Actions path:** `appbuilder-project-init` → `appbuilder-action-scaffolder` → `appbuilder-testing` → `appbuilder-cicd-pipeline`
- **UI path:** `appbuilder-project-init` → `appbuilder-ui-scaffolder` → `appbuilder-testing` → `appbuilder-cicd-pipeline`
- **E2E path:** `appbuilder-ui-scaffolder` or `appbuilder-testing` → `appbuilder-e2e-testing` → `appbuilder-cicd-pipeline`

| Skill | Description |
|-------|-------------|
| `appbuilder-project-init` | Initialize new Adobe App Builder projects and choose the right bootstrap path |
| `appbuilder-action-scaffolder` | Scaffold, implement, deploy, and debug Adobe Runtime actions |
| `appbuilder-ui-scaffolder` | Generate React Spectrum UI components for ExC Shell SPAs and AEM UI Extensions |
| `appbuilder-testing` | Generate and run Jest unit, integration, and contract tests for actions and UI components |
| `appbuilder-e2e-testing` | Playwright browser E2E tests for ExC Shell SPAs and AEM extensions |
| `appbuilder-cicd-pipeline` | Set up CI/CD pipelines for GitHub Actions, Azure DevOps, and GitLab CI |

## Repository Structure

```
skills/
├── aem/
│   ├── edge-delivery-services/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   └── skills/
│   │       ├── content-driven-development/
│   │       ├── building-blocks/
│   │       └── ...
│   ├── project-management/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   ├── fonts/
│   │   ├── hooks/
│   │   │   └── pdf-lifecycle.js
│   │   ├── templates/
│   │   │   └── whitepaper.typ
│   │   └── skills/
│   │       ├── handover/
│   │       ├── authoring/
│   │       ├── development/
│   │       ├── admin/
│   │       ├── whitepaper/
│   │       └── auth/
│   ├── cloud-service/
│   │   ├── .claude-plugin/
│   │   │   └── plugin.json
│   │   └── skills/
│   │       ├── best-practices/
│   │       │   ├── README.md
│   │       │   ├── SKILL.md
│   │       │   └── references/
│   │       ├── migration/
│   │       │   ├── README.md
│   │       │   ├── SKILL.md
│   │       │   ├── references/
│   │       │   └── scripts/
│   │       ├── ensure-agents-md/
│   │       │   ├── SKILL.md
│   │       │   └── references/
│   │       │       ├── AGENTS.md.template
│   │       │       └── module-catalog.md
│   │       ├── create-component/
│   │       │   ├── SKILL.md
│   │       │   ├── assets/
│   │       │   └── references/
│   │       ├── aem-workflow/
│   │       │   ├── SKILL.md
│   │       │   ├── workflow-model-design/
│   │       │   ├── workflow-development/
│   │       │   ├── workflow-triggering/
│   │       │   ├── workflow-launchers/
│   │       │   ├── workflow-debugging/
│   │       │   ├── workflow-triaging/
│   │       │   └── workflow-orchestrator/
│   │       └── dispatcher/
│   │           ├── SKILL.md
│   │           ├── config-authoring/
│   │           ├── technical-advisory/
│   │           ├── incident-response/
│   │           ├── performance-tuning/
│   │           ├── security-hardening/
│   │           └── workflow-orchestrator/
│   └── 6.5-lts/
│       ├── .claude-plugin/
│       │   └── plugin.json
│       └── skills/
│           ├── aem-workflow/
│           │   ├── SKILL.md
│           │   ├── workflow-model-design/
│           │   ├── workflow-development/
│           │   ├── workflow-triggering/
│           │   ├── workflow-launchers/
│           │   ├── workflow-debugging/
│           │   ├── workflow-triaging/
│           │   └── workflow-orchestrator/
│           ├── ensure-agents-md/
│           └── dispatcher/
│               ├── SKILL.md
│               ├── config-authoring/
│               ├── technical-advisory/
│               ├── incident-response/
│               ├── performance-tuning/
│               ├── security-hardening/
│               └── workflow-orchestrator/
└── app-builder/
    ├── .claude-plugin/
    │   └── plugin.json
    └── skills/
        ├── _shared/
        ├── appbuilder-project-init/
        ├── appbuilder-action-scaffolder/
        ├── appbuilder-ui-scaffolder/
        ├── appbuilder-testing/
        ├── appbuilder-e2e-testing/
        └── appbuilder-cicd-pipeline/
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on adding or updating skills. Join [#agentskills](https://adobe.enterprise.slack.com/archives/C0APTKDNPEY) on Adobe Slack for questions and discussion.

## Resources

- [agentskills.io Specification](https://agentskills.io)
- [Claude Code Plugins](https://code.claude.com/docs/en/discover-plugins)
- [Vercel Skills](https://github.com/vercel-labs/skills)
- [upskill GitHub Extension](https://github.com/trieloff/gh-upskill)
- [#agentskills Slack Channel](https://adobe.enterprise.slack.com/archives/C0APTKDNPEY)

## License

Apache 2.0 - see [LICENSE](LICENSE) for details.
