# RFC 4180 Strict CSV Persistence with Prefix IDs

To maintain spreadsheet-friendly human inspection without database complexity, system state is stored in CSV files using strict RFC 4180 compliance (quoted fields and escaped quotes) with deterministic prefixed identifiers (`opp_001`, `per_001`). This ensures multi-line text fields like conversation notes and summaries do not corrupt row structures or line-based git operations, while providing unambiguous foreign key references across entities.
