---
name: career-run
description: Run the master Career OS morning pipeline (Scout -> Network -> Prioritize) and display the Daily Brief.
---

# Career Run

This skill executes the automated morning workflow:

1. Run the master pipeline:
   ```bash
   python3 -m src.runner --agent all
   ```
2. Read the generated daily brief:
   View `outputs/daily_brief.md`.
3. Present the **Top Actions (Max 3–5)** clearly to the user, highlighting:
   - High-leverage outreach tasks with pre-formatted search links.
   - Channel-adaptive drafts ready for user review.
   - Any urgent follow-ups.
