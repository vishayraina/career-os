# Pipeline Orchestration and Execution Order

To balance the need for fresh opportunity enrichment with fast ad-hoc agent runs, the automated morning workflow runs sequentially (Scout → Network Manager → Prioritizer) so that newly found opportunities are immediately analyzed for network paths. Standalone skills (`/scout-run`, `/network-run`, `/prioritize-run`) operate independently on current disk state and can be triggered in parallel. This prevents race conditions during the primary brief generation while maintaining developer flexibility.
