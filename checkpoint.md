---
name: checkpoint
description: Save task state before compaction or resume from saved checkpoint. Use "/checkpoint" to save, "/checkpoint resume" to continue where you left off.
---

# Checkpoint Skill

Save and resume task context across compactions or sessions.

## Usage

- `/checkpoint` or `/checkpoint save` — Save current task state
- `/checkpoint resume` — Load saved state and continue automatically

## When Saving (`/checkpoint` or `/checkpoint save`)

Create/update the checkpoint file at:
`~/.claude/projects/{project}/memory/CHECKPOINT.md`

The checkpoint file MUST contain:

```markdown
---
saved_at: {ISO timestamp}
status: pending
---

## Task
{One-line description of what's being built}

## Current State
{What's done, what's in progress}

## Files & Locations
{List each file with specific line numbers and what to change}

## Next Steps
{Numbered list of exactly what to do next}

## Resume Prompt
{A minimal, self-contained prompt that Claude can execute immediately}
```

**Rules for saving:**
1. Be specific — include file paths, line numbers, variable names
2. The Resume Prompt should be copy-paste executable, no questions needed
3. Mark status as "pending" when saved
4. Ask the user to confirm the checkpoint looks correct before finishing

## When Resuming (`/checkpoint resume`)

1. Read `~/.claude/projects/{project}/memory/CHECKPOINT.md`
2. If no checkpoint exists or status is "completed", tell user there's nothing to resume
3. If checkpoint exists with status "pending":
   - Show the user what task is being resumed (one line)
   - Execute the Resume Prompt directly — start building, don't ask permission
   - When task is complete, update status to "completed"

## Example Checkpoint File

```markdown
---
saved_at: 2026-04-25T16:30:00Z
status: pending
---

## Task
Add expandable calculation breakdowns for DCF/EV-EBITDA/P-E in valuation modal

## Current State
- Planning complete, no code written yet
- Identified all backend variables and frontend locations

## Files & Locations
- Backend: `valuation_app/valuation_professional.py` line 596 — add detail dicts to return
- Frontend: `valuation_app/static/js/app.js` line 1436 — make method cards expandable
- CSS: `valuation_app/static/css/styles_professional.css` — add accordion styles

## Next Steps
1. Add dcf_details, ev_ebitda_details, pe_details to return dict in valuation_professional.py
2. Update showValuationResults() to render expandable accordions
3. Add CSS for accordion expand/collapse

## Resume Prompt
Read memory file project_valuation_detail_task.md for full spec. Build the valuation detail expansion: (1) Backend valuation_professional.py line 596 — add detail dicts to return, (2) Frontend app.js showValuationResults ~line 1436 — make cards expandable, (3) CSS accordion styles. Start with backend.
```

## Notes
- The project path is derived from current working directory
- Only one checkpoint per project (overwrites previous)
- User can manually edit CHECKPOINT.md if needed
