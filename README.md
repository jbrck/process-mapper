# process-mapper

A structured-interview skill that documents how a current business process actually works today at a resolution good enough to hand off to whoever will convert it into an agentic AI workflow later.

The skill runs a structured interview (scope, stages, step-level detail, systems & data inventory, automation readiness), then writes a matched pair of output files (`procmap_<slug>.md` + `procmap_<slug>.json`) that a builder uses to decide what's safe to automate, what needs AI-assisted review, and what should stay manual. It uses the following frameworks: Business Process Mapping (BPM), Standard Operating Procedure (SOP), and Responsible/Accountable/Consulted/Informed (RACI).  

---

## Quick start

1. **Drop the skill into your agent.** Copy the folder into the agent's skills directory, or point the agent at this repo:

   ```bash
   mkdir -p ~/.hermes/skills    # or ~/.grok/skills, ~/.cursor/skills, ...
   cp -r process-mapper ~/.hermes/skills/process-mapper
   ```

2. **Ask the agent** to "interview me about how ___ process runs" — it handles the rest: the setup pass, the interview, the review, and filing the output pair.

## Configuration

All destinations are optional. Unset values disable that step cleanly — the skill still produces the two output files and hands them to the person. Set these as environment variables in the environment where the skill runs, or edit the defaults in `SKILL.md` before installing.

| Setting | Env var | Behavior when unset |
|---|---|---|
| Drive output folder (absolute path to the folder on the person's local Drive sync mount) | `PROCMAP_DRIVE_FOLDER` | Drive filing skipped; files handed directly to the person |
| Slack channel for completion notices | `PROCMAP_SLACK_CHANNEL` | Slack notice skipped; person told to post it themselves |
| Default output directory for the files | `PROCMAP_OUTPUT_DIR` | Files saved to the current working directory |
| Maintainer / feedback contact (name, handle, or email) | `PROCMAP_MAINTAINER` | Defaults to the author in the frontmatter |
| Team / org name to use in copy | `PROCMAP_TEAM_NAME` | "the team" |

## What the interview produces

- **Markdown** (`procmap_<slug>.md`) — the readable version: scope, stages, per-step detail (with RACI, decision rules, judgment calls, common failures, manual-toil level), and evidence references.
- **JSON** (`procmap_<slug>.json`) — the structured version with a fixed shape, so a builder or another skill can parse it programmatically.

The frontmatter `description` lists the intended triggers. The interview covers scope, stage-mapping, step-level detail (with a push for object/field-level specificity, e.g. a CRM field name, encouraged to collect screenshots/copied labels as evidence), system & data inventory, and automation-readiness.

## Layout

```
process-mapper/
├── SKILL.md            # the skill itself — agent-readable, drop-in for Hermes / Grok / Cursor / Claude co-work
├── README.md           # this file
├── NOTICE.md           # attribution + provenance
└── LICENSE             # MIT
```
