---
name: doku
description: Technical documentation for projects, systems, automations, and infrastructure — architecture, setup, configuration, workflow, extension, troubleshooting. Trigger when /doku is invoked or when the user asks to document a technical implementation. Output goes to projects/<project>/TECHNICAL.md.
---

# Doku — Technical Project Documentation

## Purpose

`/doku` creates **technical project documentation** — for anything code-, infrastructure-, automation-, or integration-related.

Goal: in 6 weeks (or for someone other than you, like a developer or operations colleague), it must be clear how a system is built, how to start it, how to change it, and how to maintain it.

## Boundaries

- **README.md** = meta-information (goal, status, next step) — maintained separately
- **TECHNICAL.md (via /doku)** = how-it-works (architecture, setup, maintenance, extension)
- Not for marketing copy or whitepapers (use a content skill)
- Not for training materials (use a training-design skill)

## Input

`/doku <project-name>` or `/doku <path-to-project>`

Examples:
- `/doku transfer-loop` — document an n8n workflow + API
- `/doku web-frontend` — document a Next.js route + signup flow
- `/doku server-setup` — document complete server infrastructure
- `/doku news-scanner` — document a managed agent + LaunchAgent

If no project given: ask which system to document and wait.

## Procedure

### Phase 1 — Identify and inspect

1. Find the project directory:
   - In repo: `projects/<name>/`
   - External: `/path/to/<name>/`
   - Remote: SSH read for server-side projects
2. Read existing files: README, package.json, docker-compose.yml, Dockerfile, key code files
3. For external infrastructure: read config, directory structure, running containers via SSH

### Phase 2 — Build the document

Write to `projects/<project>/TECHNICAL.md`. Structure:

```markdown
# <Project Name> — Technical Documentation

> Created: YYYY-MM-DD
> Last updated: YYYY-MM-DD
> See also: [[README]], [[related-projects]]

## Overview

[1-3 sentences: what the system does, for whom, what purpose]

## Architecture

[Text diagram or component list: which building blocks, how connected]

```
[ASCII diagram or flow description]
```

## Components

### <Component A>
- **Purpose:** ...
- **Path/Host:** /path/to/code/ or server:/opt/...
- **Technology:** Node.js 22 / Python / n8n / Docker / Next.js
- **Dependencies:** [list with versions from package.json]
- **Ports:** 3000 internal, 443 external via reverse proxy
- **Status:** active / in development / archived

[Repeat per component]

## Setup

### Prerequisites
- [Host requirements, accounts, API keys]

### Installation
```bash
[Actual commands, copy-able]
```

### Start / Deploy
```bash
[Start command locally and on server]
```

## Configuration

### Environment Variables
| Variable | Purpose | Source |
|---|---|---|
| API_KEY | external service access | /path/to/.env (server) |
| ... | ... | ... |

### Credentials / Secrets
- **Where:** [path or service, e.g. password manager, .env, secret manager]
- **Never:** commit to repo, log, or display in messages

### Important Files
- `docker-compose.yml`: container orchestration
- `Caddyfile`: reverse proxy + SSL
- ...

## Usage / Workflow

### Standard flow
1. [Step 1: what triggers the system]
2. [Step 2: what happens internally]
3. [Step 3: what is the result]

### Alternative flows / edge cases
[Examples]

## Maintenance

### View logs
```bash
[Command]
```

### Restart
```bash
[Command]
```

### Backup / Restore
[Strategy + commands]

## Extension

### Add new <component>
[Step by step]

### Integration with other systems
[How to connect X to Y]

## Troubleshooting

### Known issue A
- **Symptom:** ...
- **Cause:** ...
- **Solution:** ...

[Repeat for known issues]

## References

- [[infrastructure-docs]] — server foundation
- [[agent-registry]] — if this is an agent
- External docs: [link]

## History

| Date | What happened |
|---|---|
| YYYY-MM-DD | Initial built + deployed |
| YYYY-MM-DD | [next significant change] |
```

### Phase 3 — Cross-link

1. In `projects/<project>/README.md` add: "Technical doc: [[TECHNICAL]]"
2. In any agent registry, add reference to TECHNICAL.md
3. For infrastructure-relevant projects, link from infrastructure docs

### Phase 4 — Confirm

Short report:
- File path of the new/updated TECHNICAL.md
- Number of documented components
- Open items (e.g. "Credential paths not verified, please confirm")

## Output

- File: `projects/<project>/TECHNICAL.md`
- Format: Markdown, all sections from the template
- Standard filename: `TECHNICAL.md`. For multiple docs per project: `docs/<topic>.md`

## Rules

- **Concrete before abstract** — real commands, real paths, real ports
- **No secrets in the doc** — only paths to where they live, never values
- **Commands copy-able** — as code blocks with full paths
- **Architecture visual** — ASCII diagram where it helps (container topology, request flow)
- **Reality, not plan** — what actually runs (verify via SSH if remote)
- **Updateable** — write so new components can be added in 6 weeks without rebuilding the doc

## When to use

- After building new infrastructure (server, container)
- After building an automation (workflow, integration)
- After deploying an app
- When delegating to another person
- After major refactoring or technology changes
