# Phase 3 results: reliability-aware maintenance under bounded budgets

This directory contains the curated, machine-readable summaries of the Phase 3 study
(follow-up to the frozen paired challenges in `results/0514_corrected_interaction_v1/`).
Raw run logs, per-frame images, and the simulation harness live in the research workspace;
everything below is small, self-contained, and generated directly by the frozen runs.

## Study map

| Study | Question | Key numbers | Files |
|---|---|---|---|
| 1. Diagnosis | What is actually uncertain about a memory? | 2,281 decision-point records; staleness near-deterministic by object category (type-only CV AUC 0.9962; movable subset 0.9278) | `m1_signal_ablation.json`, `m1_decision_point_summary.json` |
| 2. Verdicts | Can a verifier false-negative mode be caught and repaired? | 4/181 verified rows are FNs (Book 2/4, Box 2/8); pre-registered randomized 30-round replication, analyzed at the randomized-session level: **8/8** pattern-present sessions recovered with the trigger armed vs **0/12** disarmed (Fisher exact p = 7.9e-06); paired case success 60/60 vs 48/60 with the gain concentrated in 6 of 30 sessions (round-level paired randomization p = 0.032; cluster bootstrap 95% CI [6.7, 36.7] points). The earlier row-level McNemar (4.9e-4) is superseded as anti-conservative. | `m2_verifier_reliability.json`, `m2_powered_analysis.json`, `m2_powered_cluster_analysis.json`, `m2_powered_schedule.json`, `m2_three_list_analysis.json`, `m2_nearmatch_analysis.json`, `m2_design_freeze.json` |
| 2b. Occurrence boundary | Does the failure mode recur on new cases? | No further occurrences in 30 baseline sessions on an independently screened new Box pair, 60 case-instances across six further Box seeds, or 72 Book sessions across three stack configurations (rich/default probe; fp16/fp32) — the signature's occurrence is **case-specific**, so no claim that it generalizes to new cases or classes. | `m2_newpair_attempts.json` |
| 3. Selection & budget | How should a bounded verification budget be allocated? | Frozen 97-key mixed set: deployed EV 72.2% vs uniform 61.9% (McNemar p = 6.3e-3) vs reliability-first 38.1%; five static model families under identical leave-scene-out folds reach 0.66–0.73 top-1, none materially above the deployed 0.72 (key-level bootstrap CIs include zero) — the headroom is near-saturated across tested families; k=2 reaches 94.8% selection / 76.3% success vs the 79.4% no-constraint ceiling (interaction-limited) | `m3_allocation_freeze.json`, `m3_analysis_k1.json`, `m3_analysis_k2.json`, `m3_analysis_unbounded.json`, `m3_information_ceiling.json`, `m3_model_family_analysis.json`, `m3_allocation_composition.json` |
| 4. Limits | Why can't the headroom be cashed remotely? | Target visible from the spawn pose after the change in only **2/30** keys (6.7%) | `e4_visibility_pilot.json` |
| 5. Stratified evaluation | Is the active advantage an artifact of the displacement screen? | Screened-list same-protocol re-run: active **33/36** vs passive **0/36** (McNemar p = 2.3e-10). Below-threshold stratum (47 frozen cases, 41 evaluable): active **38/41** vs passive **19/41** (p = 2.1e-05) — passive keeps non-trivial success at small displacements, so the screen was what made it fail outright, yet the active advantage persists. Repeat runs reproduce (33/36; 37/41). | `stratified_fresh_run1.json`, `stratified_fresh_repeat.json`, `stratified_stale_rerun.json`, `stratified_stale_repeat.json` |
| Audit | Do the paper numbers match the artifacts? | 104 automated checks, 0 mismatches | `paper2_claim_audit.json` |

## Provenance and discipline

- All case lists, thresholds, and (for randomized arms) the full run schedule were frozen before
  any outcome was observed; freeze hashes are inside `m2_design_freeze.json`,
  `m2_powered_schedule.json`, and `m3_allocation_freeze.json`.
- Comparisons are paired on identical cases with exact tests (McNemar, Fisher exact,
  Clopper-Pearson / Wilson intervals where applicable).
- The stratified strata were frozen before their end-to-end arms ran; the below-threshold
  stratum's active arm had previously been instrumented for decision-point logging (35/41
  evaluable successes) and the paired run reproduces it (38/41).
- Demos (qualitative renderings of recorded runs, re-simulated with deterministic spawn):
  `videos/m2_reliability_Box_163_baseline.mp4` (the failure mode) and
  `videos/m2_reliability_Box_163_active.mp4` (the recovery), plus seed-191 copies.
- Figures: `figures/phase3/` (including `stratified_evaluation.png`).

## Boundary

Bounded, auditable claims only: conditional mechanism evidence for the verdict stage
(pattern-conditional recoveries under a randomized schedule; the signature's *occurrence* is
case-specific and its prevalence outside the original pair is unestablished), single-run paired
arms for the allocation comparisons, a family-level near-saturation statement (five tested model
families, fixed feature set — not an impossibility result), and fixed-universe, single-machine
stratified evidence. No overall false-negative-rate reduction, no unfiltered-benchmark,
navigation, or cross-platform claims.
