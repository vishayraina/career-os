---
name: log-action
description: Log outcomes, update opportunity stages, record contacts, and update relationship follow-up dates.
---

# Log Action

Guide the user through updating system state after executing daily actions, or parse conversational feedback:

1. If updating an opportunity status:
   ```bash
   python3 -m src.runner --agent log --opp-id <ID> --status <applied|screening|technical|offer|rejected> --notes "<NOTES>"
   ```
2. If adding or updating a person in the network:
   ```bash
   python3 -m src.runner --agent add-person --name "<NAME>" --company "<COMPANY>" --role "<ROLE>" --opp-id "<OPP_ID>" --notes "<NOTES>"
   ```
3. After logging, confirm the update to the user and refresh the brief if desired.
