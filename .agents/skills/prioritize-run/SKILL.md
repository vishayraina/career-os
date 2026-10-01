---
name: prioritize-run
description: Run the Career Prioritizer standalone to re-rank current pipeline state and refresh the Daily Career Brief.
---

# Prioritize Run

Recompute the daily action priorities from existing opportunities and relationships:

```bash
python3 -m src.runner --agent prioritizer
```

Outputs the refreshed `outputs/daily_brief.md` adhering strictly to the 3–5 daily action quota.
