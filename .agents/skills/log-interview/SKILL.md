---
name: log-interview
description: Log interview debriefs, record round outcomes, capture weaknesses and learnings, and update readiness progression.
---

# Log Interview

Guide the user through debriefing an interview round and feeding lessons back into the Progressive Interviews ladder:

1. **Log an Interview Round**:
   ```bash
   python3 -m src.runner --agent log-interview \
     --opp-id <OPP_ID> \
     --round "<ROUND_NAME>" \
     --outcome <Passed|Failed|Pending|Withdrawn> \
     --topics "<TOPICS>" \
     --weaknesses "<WEAKNESSES>" \
     --learnings "<LEARNINGS>" \
     --surprises "<SURPRISES>" \
     --conf-before <1-5> \
     --conf-after <1-5>
   ```

2. **Inspect Current Progression Readiness**:
   ```bash
   python3 -m src.runner --agent progression
   ```

3. **Override Recommendation for an Opportunity**:
   ```bash
   python3 -m src.runner --agent set-progression \
     --opp-id <OPP_ID> \
     --progression-action <take_now|protect_for_later|in_prep|auto> \
     --stage <practice|low_stakes|moderate|target|stretch|foreign|labs>
   ```
