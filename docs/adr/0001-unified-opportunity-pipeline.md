# Unified Opportunity Pipeline Entity

To track job opportunities without state desynchronization between discovered roles and active submissions, we represent each opportunity and its application stage within a single `Opportunity` entity rather than maintaining separate `opportunities.csv` and `applications.csv` tables. An opportunity begins at discovery and transitions through application and interview stages on the same record, associating referral and recruiter relationships via foreign IDs. This eliminates dual-status conflicts while keeping the data model minimal for the MVP.
