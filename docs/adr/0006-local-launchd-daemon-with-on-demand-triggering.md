# Local Launchd Daemon with On-Demand Triggering

To guarantee that the Daily Career Brief is ready every morning before the user starts their day—without depending on an open IDE session or continuous cloud server polling—the automated morning pipeline is scheduled via a lightweight local macOS `launchd` service. On-demand execution remains available at all times through Antigravity slash commands (`/career-run`, `/scout-run`, etc.).
