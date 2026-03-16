# AEM 6.5 LTS Workflow Skills

This package contains workflow-focused skills for **AEM 6.5 LTS**.

"Workflow" here means the **Granite Workflow Engine** — models, process steps, participant steps, launchers, and the `WorkflowSession` API. It does not mean CI/CD pipelines.

## Scope

Use these skills for:

- designing and deploying workflow models (XML, Workflow Model Editor, `/etc/workflow/models/`)
- implementing custom `WorkflowProcess` and `ParticipantStepChooser` Java components
- configuring workflow launchers that automatically start workflows on JCR events
- starting workflows manually, via Manage Publication, or programmatically via the API
- understanding the Granite engine, `WorkflowSession`, payload types, and metadata maps
- 6.5 LTS deployment patterns: Package Manager, Maven, Felix SCR annotations, legacy `/etc` paths
- workflow packages (`cq:WorkflowContentPackage`) for multi-page payloads

## Skill Map

- `workflow-orchestrator/` — end-to-end workflow development spanning design, implementation, deployment, and validation
- `workflow-model-design/` — designing workflow models: step types, model XML, OR/AND splits, variables, legacy `/etc` vs `/conf` storage
- `workflow-development/` — implementing custom Java steps: `WorkflowProcess`, `ParticipantStepChooser`, Felix SCR and DS R6 registration
- `workflow-triggering/` — all mechanisms to start a workflow: manual, Manage Publication, programmatic API, HTTP API, replication-linked triggers
- `workflow-launchers/` — configuring `cq:WorkflowLauncher` nodes: event types, glob patterns, conditions, override chain including legacy `/etc`

## Shared Foundation

Cross-cutting references are packaged inside each skill under `references/workflow-foundation/`.

That directory contains architecture overviews, JCR path references, API contracts, and 6.5 LTS deployment guardrails used across multiple skills.

## How To Start

For broad or first-time requests, start with:

- `workflow-orchestrator/SKILL.md`
- `workflow-orchestrator/references/workflow-foundation/quick-start-guide.md`

For targeted work, start with the specialist skill that matches the request.
