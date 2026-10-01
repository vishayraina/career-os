---
name: network-run
description: Run Network Manager standalone to audit overdue follow-ups and discover contact paths for active opportunities.
---

# Network Run

Execute the Network Manager independently:

```bash
python3 -m src.runner --agent network
```

Summarize:
- Any overdue follow-ups from `data/people.csv`.
- Generated 1-click Google X-ray search URLs for target hiring managers and recruiters.
- Adaptive message drafts for outreach.
