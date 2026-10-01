# Spec: Career OS MVP

**Labels:** `ready-for-agent`

## Problem Statement

As an experienced AI/ML engineer specialized in ASR, audio models, and ML systems, Vishay spends excessive time and cognitive bandwidth manually tracking job boards, identifying recruiters, avoiding low-signal agency spam, and deciding how to prioritize limited daily career energy. Context switching between disparate platforms (LinkedIn, company ATS boards, email, spreadsheets) leads to missed follow-ups, low conversion rates on cold applications, and decision fatigue regarding which opportunities offer true international compensation ($250k+) and visa sponsorship.

## Solution

Career OS: A lightweight agentic career operating system that executes an automated morning pipeline (Opportunity Scout → Network Manager → Career Prioritizer → Daily Career Brief) while keeping Vishay in 100% control of all external actions. The system maintains state in strict RFC 4180 CSV files with deterministic prefixed IDs, tracks opportunities through a unified pipeline, performs automated deduplication, discovers warm outreach paths via 1-click Google X-Ray search links, generates channel-adaptive outreach drafts, and strictly caps daily recommendations at 3–5 high-leverage actions while explicitly deferring lower-priority leads.

## User Stories

1. As a career-driven engineer, I want the system to read my core profile, target roles, and target geographies from markdown files, so that all recommendations are strictly personalized to my career level and visa constraints.
2. As a user, I want the Opportunity Scout to evaluate opportunities across 12 target geographies (US, UK, France, Switzerland, Netherlands, Germany, Canada, Singapore, UAE, Ireland, Japan, Australia), so that I don't miss high-paying foreign positions.
3. As a user, I want the Opportunity Scout to prioritize roles where my ASR, speech AI, and ML systems optimization background gives me an unfair advantage, so that I focus on high-probability opportunities.
4. As a user, I want the Opportunity Scout to automatically deduplicate job postings using a canonical key, so that previously evaluated or archived roles are never resurfaced.
5. As a user, I want the Opportunity Scout to limit daily newly discovered opportunities to 3–5 items, so that I am never overwhelmed by job board noise.
6. As a user, I want the Network Manager to monitor existing relationships in my network, so that I am alerted whenever a follow-up date has arrived or is overdue.
7. As a user, I want the Network Manager to identify relevant point-of-contact roles (Recruiters and Hiring Managers) for newly discovered opportunities, so that I can pursue warm introductions rather than cold applications.
8. As a user, I want the Network Manager to generate 1-click Google X-Ray search links for target contacts, so that I can view verified LinkedIn profiles safely without scraping or bot detection.
9. As a user, I want the Network Manager to generate zero-fluff, peer-to-peer connection drafts under 300 characters for LinkedIn, so that I can copy-paste them directly into connection requests.
10. As a user, I want the Network Manager to generate concise 100–150 word technical drafts for emails or warm follow-ups, so that my outreach conveys deep domain competence without filler.
11. As a user, I want the Career Prioritizer to rank all candidate actions according to a strict leverage hierarchy (Interviews > Overdue Follow-ups > Warm Outreach > New Opportunity Applications), so that my time is spent on the highest conversion activities.
12. As a user, I want the Career Prioritizer to enforce a strict maximum quota of 3–5 actions in the Daily Career Brief, so that my daily action plan is realistic and consumable in under 5 minutes.
13. As a user, I want the Career Prioritizer to explicitly list deferred tasks and opportunities in an "IGNORE / DEFER (SKIP TODAY)" section, so that I have peace of mind knowing what was intentionally set aside.
14. As a user, I want the Daily Career Brief to explain "Why These Matter", so that I understand the strategic rationale behind every recommendation.
15. As a user, I want all opportunity data persisted in a strict RFC 4180 CSV with deterministic IDs (`opp_001`, `opp_002`), so that multi-line text and special characters do not corrupt the database.
16. As a user, I want all network contacts persisted in a strict RFC 4180 CSV with deterministic IDs (`per_001`), so that relationships can link unambiguously to opportunities via foreign keys.
17. As a user, I want a single command (`/career-run` or `python -m src.runner --agent all`) to execute the entire morning pipeline sequentially, so that fresh opportunities are enriched with network paths before prioritization.
18. As a user, I want the ability to run Opportunity Scout, Network Manager, or Career Prioritizer standalone and in parallel, so that I can perform ad-hoc checks whenever needed.
19. As a user, I want an interactive logging command (`/log-action` or `python -m src.runner --agent log`) to update opportunity statuses and notes, so that my pipeline state stays current.
20. As a user, I want a quick command to add a new person to my network (`python -m src.runner --agent add-person`), so that spontaneous interactions are captured immediately.
21. As a user, I want a local background daemon (`launchd`) scheduled at 07:30 AM daily, so that my Daily Career Brief is generated and waiting on disk before I begin work.
22. As a user, I want a strict human-in-the-loop guarantee where no messages, emails, or job applications are ever submitted automatically, so that my professional reputation remains completely protected.

## Implementation Decisions

### Architectural Seams & Core Modules
- **CLI Command Runner (`src.runner`)**: Acts as the single high-level seam for external execution. Exposes deterministic commands for `--agent all`, `--agent scout`, `--agent network`, `--agent prioritizer`, `--agent log`, and `--agent add-person`.
- **Antigravity Skills Interface (`.agents/skills/`)**: Lightweight wrappers (`career-run`, `scout-run`, `network-run`, `prioritize-run`, `log-action`) providing conversational entrypoints and natural language feedback.

### Data Schemas & Persistence (ADRs 0001 & 0002)
- Unified Opportunity Pipeline: Merges application lifecycle metadata directly into `Opportunity` rather than using separate disjoint tables.
- Strict RFC 4180 CSV Persistence with Prefix IDs:

```text
Opportunity Record:
- id: str (PK: opp_xxx)
- company, role, level, team, location, remote_policy: str
- visa_sponsorship, relocation_support: Enum (Yes, Likely, Unknown, No)
- compensation, posting_date, url: str
- fit_category: str (e.g. Speech / Voice / Audio, AI Infrastructure / Inference)
- fit_score: Enum (Strong, Moderate, Low)
- why_relevant, stretch_skills: str
- status: Enum (discovered, considering, applied, screening, technical, offer, rejected, archived)
- date_applied: str (YYYY-MM-DD)
- primary_contact_id: str (FK: per_xxx)
- network_status: Enum (None, Identified, Contacted, In-Conversation, Referred)
- notes: str

Person Record:
- id: str (PK: per_xxx)
- name, company, role, url: str
- relationship_status: Enum (cold, aware, warm, engaged, advocate)
- last_contact_date, next_follow_up_date: str (YYYY-MM-DD)
- conversation_summary, next_step: str
- opportunity_id: str (FK: opp_xxx)
- notes: str
```

### Orchestration & Scheduling (ADRs 0004 & 0006)
- Morning Execution: Runs sequentially (Scout → Network Manager → Prioritizer) to guarantee newly scraped roles receive immediate contact research.
- Standalone Execution: Scout, Network, and Prioritizer can be triggered individually or concurrently on demand.
- Local Daemon: Scheduled via macOS `launchd` plist targeting 07:30 AM daily.

### Outreach Standards (ADR 0005)
- LinkedIn: Strict character limit (<300 characters), peer-to-peer technical context, zero sycophancy or generic flattery.
- Email / Direct Note: 100–150 words highlighting technical overlap (ASR/inference) and a low-friction call to action.

## Testing Decisions

### What Makes a Good Test
- Tests must exclusively evaluate **external observable behavior** across the highest seam (`src.runner`), asserting on CSV state mutations, deduplication guarantees, quota bounds, and generated brief content.
- Tests must NOT bind to private helper functions, internal scraping scrapers, or volatile heuristics.

### Test Matrix
1. **Pipeline Execution Test**: Running `python -m src.runner --agent all` against empty/seeded CSVs produces properly formatted rows with sequential IDs (`opp_001`, `per_001`) and writes `outputs/daily_brief.md`.
2. **Deduplication Test**: Re-running Opportunity Scout against already ingested companies and roles yields 0 newly created records.
3. **Quota & Deferral Test**: When more than 5 high-priority actions exist, the Prioritizer strictly truncates top actions to 5 and pushes the rest to `## ⏸ IGNORE / DEFER (SKIP TODAY)`.
4. **Status & Interaction Logging Test**: Invoking `--agent log` with valid `--opp-id` updates the status (e.g. `applied`), stamps `date_applied`, and updates notes without corrupting CSV formatting.
5. **RFC 4180 CSV Integrity Test**: Fields containing commas, newlines, and quotes are properly enclosed in double quotes and read back identically.

### Prior Art
- Existing test patterns leverage Python's built-in `unittest` or `pytest` with temporary working directory fixtures (`tmp_path`) to ensure test isolation from production data files.

## Out of Scope

- Automated submission of job applications or automated web form filling.
- Automated sending of LinkedIn messages, connection requests, or cold emails without user review.
- Direct scraping of authenticated LinkedIn member data or session hijacking.
- Web UI dashboards, microservices, cloud relational databases, or vector databases.
- Multi-user authentication or team tenancy (Career OS is strictly a single-user personal OS).

## Further Notes

- **Domain Glossary**: Adheres strictly to terms defined in [GLOSSARY.md](file:///Users/vishayraina/Desktop/career-os/GLOSSARY.md).
- **Target Universe**: Sourcing references the complete 12-geography list and company fit signals in [career/target_companies.md](file:///Users/vishayraina/Desktop/career-os/career/target_companies.md).
- **Issue Tracker**: Stored locally in `docs/specs/0001-career-os-mvp.md`. Can be mirrored to GitHub Issues once GitHub CLI authentication (`gh auth login`) is configured.
