# Phase 3 results: reliability-aware maintenance under bounded budgets

This directory contains the curated, machine-readable summaries of the Phase 3 study
(follow-up to the frozen paired challenges in `results/0514_corrected_interaction_v1/`).
Raw run logs, per-frame images, and the simulation harness live in the research workspace;
everything below is small, self-contained, and generated directly by the frozen runs.

## Study map

| Study | Question | Key numbers | Files |
|---|---|---|---|
| 1. Diagnosis | What is actually uncertain about a memory? | 2,281 decision-point records; staleness near-deterministic by object category (type-only CV AUC 0.9962; movable subset 0.9278) | `m1_signal_ablation.json`, `m1_decision_point_summary.json` |
| 2. Verdicts | Can a verifier false-negative mode be caught and repaired? | 4/181 verified rows are FNs (Book 2/4, Box 2/8); randomized 30-round replication: **16/16** pattern events recovered with the trigger armed vs **0/24** disarmed (Fisher exact p = 1.6e-11); paired case success 60/60 vs 48/60 vs 36/60 (McNemar p = 4.9e-4) | `m2_verifier_reliability.json`, `m2_powered_analysis.json`, `m2_powered_schedule.json`, `m2_three_list_analysis.json`, `m2_nearmatch_analysis.json`, `m2_design_freeze.json` |
| 3. Selection & budget | How should a bounded verification budget be allocated? | Frozen 97-key mixed set: deployed EV 72.2% vs uniform 61.9% (McNemar p = 6.3e-3) vs reliability-first 38.1%; learned ranker over all policy-visible signals: 73.2% (information-limited); k=2 reaches 94.8% selection / 76.3% success vs the 79.4% no-constraint ceiling (interaction-limited) | `m3_allocation_freeze.json`, `m3_analysis_k1.json`, `m3_analysis_k2.json`, `m3_analysis_unbounded.json`, `m3_information_ceiling.json` |
| 4. Limits | Why can't the headroom be cashed remotely? | Target visible from the spawn pose after the change in only **2/30** keys (6.7%) | `e4_visibility_pilot.json` |
| Audit | Do the paper numbers match the artifacts? | 60 automated checks, 0 mismatches | `paper2_claim_audit.json` |

## Provenance and discipline

- All case lists, thresholds, and (for randomized arms) the full run schedule were frozen before
  any outcome was observed; freeze hashes are inside `m2_design_freeze.json`,
  `m2_powered_schedule.json`, and `m3_allocation_freeze.json`.
- Comparisons are paired on identical cases with exact tests (McNemar, Fisher exact,
  Clopper-Pearson / Wilson intervals where applicable).
- Demos (qualitative renderings of recorded runs, re-simulated with deterministic spawn):
  `videos/m2_reliability_Box_163_baseline.mp4` (the failure mode) and
  `videos/m2_reliability_Box_163_active.mp4` (the recovery), plus seed-191 copies.
- Figures: `figures/phase3/`.

## Boundary

Bounded, auditable claims only: conditional mechanism evidence for the verdict stage
(pattern-conditional recoveries under a randomized schedule), single-run paired arms for the
allocation comparisons, and a universe-specific saturation diagnostic. No overall
false-negative-rate reduction, no unfiltered-benchmark, navigation, or cross-platform claims.
