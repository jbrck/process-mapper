# NOTICE

## Provenance

**process-mapper** is an original skill by **Justin Brock** (https://github.com/jbrck), MIT-licensed (see `LICENSE`).

This skill was written for teams to document real business processes at a resolution an agentic orchestration builder can convert into agentic AI workflows. It is configurable via `PROCMAP_*` environment variables and is named `process-mapper` so it works in any environment (Hermes, Claude co-work, Grok, Cursor, or any terminal agent).

## What's here

| Path | Origin |
|---|---|
| `SKILL.md` | The skill itself — the portable interview + output templates + env-var config contract. |
| `README.md` | Project documentation: quick start, config table, layout. |
| `LICENSE` | MIT, Justin Brock. |
| `NOTICE.md` | This file. |

## Capabilities

`SKILL.md` runs a structured process-mapping interview (scope, stages, step-level detail, systems & data inventory, automation readiness) and writes a matched `procmap_<slug>.md` + `.json` pair. It conditionally handles a post-interview flow: a setup pass to confirm a destination, filing to a configured Drive folder or working directory, and a Slack (or equivalent) completion notice. Environment-specific destinations are all behind `PROCMAP_*` env vars (see the config table in `SKILL.md`), so the skill ships with zero hard-coded paths, folders, identities, or team names.