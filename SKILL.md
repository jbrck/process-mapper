---
name: "process-mapper"
author: "Justin Brock (https://github.com/jbrck)"
description: "Interviews someone on a team to document how a current process actually works today at a resolution good enough to hand off to whomever will convert it into an agentic Artificial Intelligence (AI) workflow later. Runs a structured interview covering scope, stages, step detail, systems/data inventory, and automation readiness, then writes a matched pair of output files (\"procmap_\" prefixed markdown + JSON). Opens with a short setup pass to confirm where the files should go and whether a completion notice is wanted. Use whenever someone wants to document, map, or \"interview me about\" how a process currently runs — especially when prepping for automation, an AI-agent skill, or a builder handoff — even without saying \"agentic\" or \"automation.\""
---

## Configuration (edit when installing)

All destinations are optional. Unset values disable that step cleanly — the skill still produces the two output files and hands them to the person. Set these as environment variables in the environment where the skill runs, or edit the defaults here before installing.

| Setting | Env var | Behavior when unset |
|---|---|---|
| Drive output folder (absolute path to the folder on the person's local Drive sync mount) | `PROCMAP_DRIVE_FOLDER` | Drive filing skipped; files handed directly to the person |
| Slack channel for completion notices | `PROCMAP_SLACK_CHANNEL` | Slack notice skipped; person told to post it themselves |
| Default output directory for the files | `PROCMAP_OUTPUT_DIR` | Files saved to the current working directory |
| Maintainer / feedback contact (name, handle, or email) | `PROCMAP_MAINTAINER` | Defaults to the author in the frontmatter |
| Team / org name to use in copy | `PROCMAP_TEAM_NAME` | "the team" |

## About this skill

- **Built on:** A structured-interview / process-mapping approach — the same underlying discipline as Business Process Mapping (BPM) stage/step mapping, combined with a lightweight RACI (who's Responsible/Accountable/Consulted/Informed at each step) and an explicit split between rule-based decision points ("if X, then Y" — automatable) and judgment calls (not automatable without a human in the loop). The question set was shaped by actually running these interviews with team members and iterating on what a builder needs to see to make automate/keep-manual calls with confidence.
- **Why it exists:** You can now build agentic workflows, orchestrations, automations, and AI-agent skills around recurring processes. However, before automating anything, you need an accurate, consistent record of how each process actually runs today - not an idealized version, but the real process with the annoying manual parts intact.
- **What it's for:** Running a structured interview with whoever owns a process, and turning the conversation into a matched markdown + JSON pair (`procmap_<slug>.md` / `.json`) that a builder can use to decide what's safe to automate, what needs AI-assisted review, and what should stay manual. Once the pair is done, the skill files it into the configured Drive folder and posts a heads-up in the configured Slack channel if those are configured; otherwise it hands the files directly to the person.
- **Before running this:** whoever's running the interview should make sure the session/workspace can reach the configured `PROCMAP_DRIVE_FOLDER` — usually a folder inside their local Drive sync mount. The agent can only see folders the environment has actually given it access to, not the whole machine, so this step is what lets the skill file straight to Drive. If it can't be reached, the skill still works — it just falls to a direct handoff instead of filing automatically (see "After the interview" below).
- **If something's off:** wrong question, clunky flow, output that doesn't match what you need, message the maintainer listed in this skill's frontmatter rather than editing your local copy — that way improvements make it back into the shared version everyone's using.

## Before the interview: set up where the output goes

Run this setup pass with the person before starting Section A. It takes a couple of minutes and prevents the end-of-interview scramble to find a destination for the files. The exact mechanics depend on the agent environment (Claude co-work session, Hermes, another agent), but the questions are the same:

1. **Ask, but keep it light.** Lead with the easy default: "If you want, I'll just hand you both files at the end — no setup needed. Otherwise, is there a folder you'd like them filed into?" Only dig into destinations if they actually want one. If they just want the files, confirm that and move on — don't hold up the interview over where the files will live. Options, in order of preference: just hand them the files, a shared folder the person names, a folder inside their Google Drive (or other cloud sync) mount, a project workspace, or the current working location. If they name a path, verify you can actually reach it — write a small test file and delete it rather than assuming access.
2. **Set the destination.** If they chose a folder, put the chosen path in `PROCMAP_OUTPUT_DIR` (the default home for both files) or `PROCMAP_DRIVE_FOLDER` (if the chosen destination is specifically a Drive sync folder), or note it as the working directory for this session. If they just want the files, leave the destination unset — that already means "hand them over at the end." If the environment's settings are per-session, set them now; tell the person where the default lives if they're per-install.
3. **Ask about the completion notice.** Do they want a Slack (or equivalent) heads-up when the map is filed? If yes, confirm the channel exists and the agent actually has posting access before the interview starts — discovering you can't post after the interview is the failure mode this step exists to prevent.
4. **Say it back.** Restate where the files will go and who gets the notice. If anything is unset or unreachable, say plainly that the skill will fall back to handing the files directly to the person at the end.
5. **Re-check if the destination changes.** If the person picks a different destination mid-interview, re-verify access before continuing — don't carry a stale path into the filing step.

You are conducting a structured interview to document one process as it actually runs today — not how it should run, not how it would run if AI did it. Accuracy about the messy, manual, current-state reality is the entire point: a builder is going to read this later to decide what can be automated safely, so guessing or smoothing over the annoying parts defeats the purpose.

## How to run this

Ask one section at a time, and within Section C, one step at a time. Do not paste the whole questionnaire into one message — that overwhelms people and they start giving one-line non-answers to everything. Have a real conversation: ask a question, listen to the answer, ask a natural follow-up if something's vague, then move on.

If the person doesn't know an answer, write "TBD — follow up with [who]" rather than inventing something plausible-sounding. A gap you can see is more useful to a builder than a wrong answer you can't.

Work through the sections in this order:

1. **Section A** — scope (ask once, at the start)
2. **Section B** — stages (ask once)
3. **Section C** — step-by-step detail (repeat for every step, in every stage — this is most of the interview)
4. **Section D** — systems & data inventory (ask once, near the end)
5. **Section E** — automation readiness (ask once, at the end)

After each stage in Section C, check in before moving on: "Anything else happen in this stage, or should we move to [next stage]?"

## Section A — Scope the process (ask once)

1. What's the name of this process, and which motion is it part of? (webinar / ABM campaign / email nurture / email blast / other)
2. What triggers this process to start? (a date on a calendar, a campaign brief, an inbound lead, a request from another team)
3. Who ultimately receives or benefits from the output? (sales, a target account list, prospects/customers, another internal team)
4. What's the tangible output when this process is "done"?
5. How often does this run, and roughly what volume does it touch each time?
6. Who owns this process end-to-end today?

## Section B — Map the stages (ask once)

7. Walk me through the major stages this process moves through, in order, in your own words. Let them name their own stages — don't assume a Planning/Day-of/Post-process breakdown unless that's genuinely how they think about it.

## Section C — Step-level detail (repeat for every step, in every stage)

For each step, ask:

8. What happens in this step, specifically? Describe it like you're training someone new on day one.
9. What has to be true, or what do you need in hand, before you can start this step?
10. Which systems or tools do you touch here, and what do you actually do in each? Push for the object/field level if they know it (e.g., "update the Status field on the Campaign Member record in Salesforce," not just "update Salesforce").

For any step where that specificity matters — a Customer Relationship Management (CRM) field name, a dropdown option label, a particular screen, a report definition — encourage the person to hand over exact evidence instead of a paraphrase: have them copy the literal field label from the system, paste a snippet, screenshot the screen, or share a data dictionary or field reference doc. A paraphrased field name is where builders guess wrong later; an actual screenshot or copied label removes the guess.

11. Who is responsible for doing this step? Who else has to approve it or be looped in?
12. Are there decision points here — places where the next action depends on a condition? Get the actual rule in "if X, then Y" form.
13. Are there judgment calls that don't reduce to a clean rule? These are the parts that need a human in the loop rather than full automation, at least at first — flag them explicitly rather than letting them blend in with the rule-based steps.
14. What goes wrong most often at this step, and what do you do when it does?
15. What comes out of this step, and who or what receives it next?
16. Roughly how long does this take, and how much of it feels like manual busywork versus actual decision-making?

## Section D — Systems & data inventory (ask once, near the end)

17. List every system or tool this process touches, across every stage.
18. For each one, is there an existing Application Programming Interface (API) connection or integration already in use, or is this manual copy-paste/manual login today?
19. What data lives in unstructured form (email threads, Slack messages, docs, someone's memory) versus structured fields in a system?

## Section E — Automation readiness (ask once, at the end)

20. If you had a tireless intern who never got bored, which one or two steps would you hand them first?
21. Which steps would you never want fully automated, and why?
22. Does a Standard Operating Procedure (SOP), doc, or checklist already exist for this process? Where does it live, and how current is it?

## Producing the output

Once the interview is done, write two files, sharing a common `procmap_` prefix so any documented process is instantly recognizable in a folder — regardless of which of the four motions (or future motions) it is:

- `procmap_<slug>.md` — the readable version
- `procmap_<slug>.json` — the structured version

Where `<slug>` is the process name in lowercase, hyphenated (e.g. a process named "Quarterly Webinar Series" becomes `procmap_quarterly-webinar-series`).

Ask where to save them if it's not obvious from context (a project folder, a specific directory the person names); otherwise save both to the current working location (or to `PROCMAP_OUTPUT_DIR` if that's set).

### Markdown structure (`procmap_<slug>.md`)

```
# <Process Name>

## A. Scope
- Process name:
- Motion type:
- Trigger:
- Customer of the output:
- Definition of done / output:
- Frequency & volume:
- Process owner:

## B. Stages
1.
2.
...

## C. Steps
(one block per step, grouped under its stage heading)

### Stage: <stage name>

**Step <n>: <step name>**
- Description:
- Inputs / preconditions:
- Systems touched (object/field level):
- Evidence / references (screenshots, copied labels, docs):
- Owner (R) / Approver (A) / Consulted (C) / Informed (I):
- Decision rules (if/then):
- Judgment calls (non-rule-based):
- Common failure & recovery:
- Output / handoff to:
- Time / frequency of manual toil:

## D. Systems & data inventory
| System | Used for | API/integration exists? | Structured or unstructured data? |
|---|---|---|---|

## E. Automation readiness
- Best first candidates for automation:
- Steps to keep human-in-the-loop:
- Existing SOP/doc location:
```

### JSON structure (`procmap_<slug>.json`)

Follow this shape exactly — field names matter if a builder or another skill parses this later:

```json
{
  "scope": {
    "processName": "",
    "motionType": "webinar | abm_campaign | email_nurture | email_blast | other",
    "trigger": "",
    "customerOfOutput": "",
    "output": "",
    "frequency": "",
    "volume": "",
    "owner": ""
  },
  "stages": [
    {
      "stageName": "",
      "stageOrder": 1,
      "steps": [
        {
          "stepOrder": 1,
          "stepName": "",
          "description": "",
          "inputsPreconditions": [""],
          "systemsTouched": [
            { "system": "", "objectOrField": "", "action": "" }
          ],
          "evidence": [""],
          "raci": {
            "responsible": "",
            "accountable": "",
            "consulted": [""],
            "informed": [""]
          },
          "decisionRules": [
            { "condition": "", "action": "" }
          ],
          "judgmentCalls": [""],
          "commonFailureModes": [
            { "failure": "", "recovery": "" }
          ],
          "output": "",
          "handoffTo": "",
          "timeEstimate": "",
          "manualToilLevel": "low | medium | high"
        }
      ]
    }
  ],
  "systemsInventory": [
    { "system": "", "usedFor": "", "hasApiOrIntegration": true, "dataStructure": "structured | unstructured | mixed" }
  ],
  "automationReadiness": {
    "firstCandidates": [""],
    "keepHumanInLoop": [
      { "step": "", "reason": "" }
    ],
    "existingSopLocation": ""
  },
  "meta": {
    "documentedBy": "",
    "dateDocumented": "YYYY-MM-DD"
  }
}
```

Leave arrays empty (`[]`) rather than omitting them if a section genuinely has nothing — an empty `judgmentCalls` array tells a builder "checked, none found," which is different from a missing field.

Before presenting the files, sanity-check the JSON is valid (parses, matches the field names above) and that the markdown and JSON agree with each other — they're two views of the same interview, not two independent drafts.

## After the interview: review, file, and announce

Do these in order once the interview itself is done.

1. **Review with the person.** Show both files and ask if anything reads wrong before they consider it final — they lived the process, they'll catch things you got subtly wrong. If they correct or add detail after seeing the draft, update both the markdown and JSON together so they don't drift out of sync with each other.
2. **File the finals.** If `PROCMAP_DRIVE_FOLDER` is set and exists, write both files directly there with normal file tools and treat this as success. Note the folder path — you'll use it in the announcement in step 3. If the setup pass chose a different destination (`PROCMAP_OUTPUT_DIR` or the session working directory), the files are already where they belong — skip straight to the announcement.
   - **If it's not set, or set but missing:** don't keep retrying or try to reach Drive any other way — the configured destination just isn't available this session. Fall straight to the handoff below.
   - **Fallback — direct handoff.** Present both finished files to the person directly as a download (actually surface the files, don't just describe them) and tell them plainly that the configured Drive folder wasn't reachable this session, so they'll need to grab the files themselves and file them. There is no file-upload tool available to this skill — only text messages — so the agent cannot post the files into Slack on the person's behalf. Be upfront about that limitation rather than implying otherwise.
3. **Announce it.** If `PROCMAP_SLACK_CHANNEL` is set, post a short message to that channel using the Slack connector. Keep it brief — process name, motion type, owner — and adapt the last line to what happened in step 2:
   - **Filed to Drive:**
     > 📋 New process mapped: **<Process Name>** (<motion type>) — owner: <owner>. Docs: <Drive folder path>
   - **Fallback used:**
     > 📋 New process mapped: **<Process Name>** (<motion type>) — owner: <owner>. Local Drive folder wasn't connected this session — <person> has the files locally and will post/upload them here.
   If no Slack connector is connected, or the channel isn't set, tell the person the notification wasn't sent and let them post it themselves; don't skip this silently.