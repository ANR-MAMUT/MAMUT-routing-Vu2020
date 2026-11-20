# Changelog: Vu2020 benchmark family

All notable changes to the Vu2020 family are recorded here. The first entry records the creation of this repository. The sections after it hold the changelogs of the 2 former repositories. They keep every entry, fact, date, count and commit reference, with the wording edited for style and the headings moved 2 levels down.

## 2026-11: family repository

- The TDVRPTW and TDVRP variants of Vu2020, kept until now in the 2 repositories `MAMUT-routing-TDVRPTW-Vu2020` and `MAMUT-routing-TDVRP-Vu2020`, form 1 repository, `MAMUT-routing-Vu2020`, mounted in MAMUT-routing at `benchmarks/Vu2020`. The histories of both former repositories are imported, each under the directory of its problem type, and joined by a merge commit. `history/` holds the maps from the former to the new commit identifiers.
- Instances and best-known solutions keep their former path after the problem type, for example `TDVRP/n=59/Vu-A1-pA-d70-w150.vrp.json`.
- The ATF sidecars move to `sidecars/`, stored once for the 2 variants: 168 sidecars kept, 168 duplicate copies removed. In 336 instances the sidecar path field now holds a path relative to the repository root, for example `sidecars/n=59/Vu-A1-pA-d70-w150.atf.json.gz`. No sidecar content changed, and the sidecar SHA-256 recorded in each instance is unchanged.
- The SHA-256 of the 336 instance files changed because of the rewritten path field. No instance identifier changed, and no best-known solution changed.
- `mamut-collection.json` at the root marks the repository as a family collection (`"layout_version": 2`).
- This layout is read by `mamut-routing-lib` 0.13 and later, which is in development and not released yet. `mamut-routing-lib` 0.12 and earlier read the former layout, which stays available read-only in the 2 archived former repositories.
- The README, LICENSE, CHANGELOG and CITATION.cff files of the 2 variants are replaced by 1 file each at the root, and a NOTICE file is added.

## Former TDVRPTW repository

### Changelog of the Vu2020 TDVRPTW BKS

All notable changes to the curated `Vu2020` TDVRPTW best-known solutions (BKS) are recorded here. The objective is **Duration** (duration minimization), and the depot departure time of each route is a decision variable. Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order. Any strict improvement is therefore real. Reminder: this family is the VRP variant curated by Onyr (see README.md), so these BKS are not comparable with published TD-TSPTW results on the underlying raw files.

#### 2026-09-24

**Re-priced under the `td-fold/2` checker contract (mamut-routing-lib 0.12.0), no route changed.** mamut-routing-lib 0.12.0 replaces the TD checker's route fold (checker contract `td-fold/1` -> `td-fold/2`). Waiting and service at a vertex are now applied exactly to the accumulated arrival times instead of being composed through a ratio interpolation, the departure window restricts the first arc without interpolation, and travel on slope-one pieces is computed by addition. Under `td-fold/1` a ready time could be off by an ulp, and on stepwise travel-time functions such an ulp could land past a step and read its upper branch. All 168 BKS were re-priced with `mamut-routing bks reprice-td`: 161 files were rewritten, 56 of them because the cost moved (by at most 4.4e-16 relative, float rounding only), the other 105 only to refresh `route_durations` / `route_departure_times` by ulps. Each BKS whose cost moved records `metadata.repriced` (previous cost, checker, contract, date). 53 optimality-stamped BKS moved (by at most 1.4e-12): their `proven_optimum` now equals the re-priced cost, and a `note` records that the proof was obtained under `td-fold/1`, that `dual_bound` is the prover's value under that arithmetic, and that re-certification under `td-fold/2` is pending.

#### 2026-08-06

**165 of the family's 166 pre-withdrawal optimality certificates re-derived from scratch and re-stamped** under the 4-solve agreement protocol (cold and warm starts crossed with the 2 labeling modes, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns) on Grid'5000, on the repaired kayros 1.5.1 build. The campaign was motivated by the 2026-08-05 withdrawal of 1 certificate in the Vu2020 TDVRPTW family and the subsequent finding that the certifying builds carried since-repaired pricing defects (documented in the kayros 1.5.1 release notes and CHANGELOG). Every re-issued stamp certifies bit-exactly the previously stored value. No solution data changed, and the stamps' provenance now cites this campaign and build. Vu-A2-pB-d98-w100 (n=59, certificate withdrawn 2026-08-05) stays a plain best-known solution: the campaign again refused to certify it, with 3 solves proving 1846.8070188618972 and 1 labeling mode still closing at the withdrawn 1847.2395980636322, which is consistent with the withdrawal.

#### 2026-08-05

**Vu-A2-pB-d98-w100 (n=59): optimality certificate WITHDRAWN, best known improved 1847.239598063632 -> 1846.8070188618972 (-0.0234 %, 12 routes).** A checker-valid solution strictly below the certified value (1847.2127599998892, 11 routes, found independently by 2 heuristic solver configurations) falsified the 2026-07-10 certificate. The investigation reproduced the certified value bit-for-bit at the certifying commit and traced the failure to a pricing defect repaired 1 commit later, on 2026-07-11: at abscissas where a partial route's duration is set-valued (a departure-choice set), the pricing read an arbitrary representative instead of the minimum, so improving columns were silently discarded, and the same incomplete pricing produced the matching dual bound. The defect is deterministic and identical across the protocol's 4 runs, so 4-way agreement could not catch it on this instance. The certifying build, warm-started from the falsifying solution, itself proves the lower value. Re-solves on repaired builds prove 1846.8070188618972, validated bit-exactly by the canonical checker (an 11-route representation of the same optimum exists at 1846.8070188618974). The optimality claim is withdrawn and this entry is now a plain best-known solution. The family's remaining certificates are undergoing a full re-verification campaign on a repaired build, and optimality stamps will only be re-issued by that campaign.

#### 2026-07-18

**Vu-A1-pA-d80-w180 (n=99) independently re-confirmed, no data changes.** The last main-wave refusal of this family (refused 2026-07-10 for checker-infeasible pricing in 3 of its 4 runs, then certified by the 2026-07-13 long-limit continuation) was re-run on the kayros 1.1.0 release candidate (commit f8c788c) under the full 4-run protocol (time limit 7200 s, Grid'5000 reservation 6781269): all 4 solves reached Optimum at the stored proven optimum bit-exactly (maximum disagreement 0.0), each with an audited exact-pricing phase and 0 checker-infeasible priced columns. The 2026-07-13 certificate stands re-validated on the current production build.

#### 2026-07-13

**7 further BKS stamped proven optimal, all 7 also checker-valid strict improvements** by the long-limit continuation of the weekend re-certification (2026-07-11/12/13, Grid'5000; time limits raised to 1800 s, 7200 s at n=99). 5 of these (Vu-A4-pA-d70-w100, Vu-A5-pB-d90-w60 at n=59; Vu-A2-pB-d80-w40, Vu-A4-pB-d98-w150, Vu-A5-pB-d90-w40 at n=79) had been refused as 4-way disagreements by the main wave. The longer limits closed the labeling-mode gap (the set-valued-duration repair had already removed the mispricing, the modes then agreeing once both reached proof), and the 2 at n=99 (Vu-A1-pA-d80-w180, Vu-A4-pB-d98-w150) closed for the first time. Protocol unchanged: 4 independent exact solves (cold and warm starts crossed with the 2 labeling modes) agreeing on 1 checker-exact value, an audited exact-pricing phase in every run, and no checker-infeasible priced column. The exact value-jump arm is folded direction-aware where present. This family now carries 166 certified TDVRPTW BKS, 163 of them strict improvements over the previously heuristic references.

#### 2026-07-11 (second entry)

9 further BKS improved and **10 further BKS stamped proven optimal** under the clean-runs refinement of the 4-run protocol: on these instances 1 or 2 of the 4 runs skipped a checker-infeasible priced column (an ulp-scale time-window boundary artifact of the solver's internal arithmetic, e.g. a route violating a deadline by 2e-13 s) and were excluded. The certificate stands on the remaining clean runs (all Optimum at 1 agreeing value, at least 2 runs including a cold start, exact-pricing phase audited).

#### 2026-07-11

**149 BKS stamped proven optimal** (`metadata.optimality`), the family's first optimality certificates. Each stamp certifies 4 independent exact solves (cold and warm starts x 2 labeling modes) agreeing on the value, an audited exact-pricing phase in every run, 0 checker-infeasible priced columns, and canonical-checker re-validation at stamping time. Instances whose runs disagreed, timed out, or priced a checker-infeasible column carry NO stamp.

#### 2026-07-10

147 BKS improved by exact solves recorded as ordinary best-known solutions (no optimality stamps): a full re-certification campaign ran the exact solver (kayros lera branch-price-and-cut, HiGHS backend) 4 times per instance (cold and warm starts, 2 labeling modes) on Grid'5000 and only values with 4-way agreement, an audited exact-pricing phase, and canonical-checker re-validation were accepted. The previous references at these sizes were heuristic (TD-ILS seeds), so improvements are broad: all 3 sizes covered (n=59/79/99), largest single improvement about -3.5%. Optimality stamping is handled separately from this fold.

#### 2026-07-08

162 of 168 BKS improved (mean -1.54%, largest single improvement -6.01%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative). Stored BKS marked proven optimal were left untouched.

#### 2026-07-06 (local-search sweep)

165 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

#### 2026-07-06 (initial seeding sweep)

165 BKS added, reaching full 168/168 coverage, from the initial large-scale seeding sweep across all 4 TD families run on 2026-07-04 (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

#### 2026-07-03

Family populated (168 curated instances + ATF sidecars) with 3 BKS re-validated and re-priced from a former heuristic BKS store (2024–2026).

## Former TDVRP repository

### Changelog of the Vu2020 TDVRP BKS

All notable changes to the curated `Vu2020` TDVRP best-known solutions (BKS) are recorded here. The objective is **Duration** (duration minimization), and the depot departure time of each route is a decision variable. Costs are the authoritative output of the canonical checker (`mamut_routing_lib.td.check_td_solution`): exact IEEE-754 double arithmetic, no epsilon thresholds, routes in canonical order (sorted by first customer), total summed in that order. Any strict improvement is therefore real. Reminder: this family is the VRP variant curated by Onyr (see README.md), so these BKS are not comparable with published TD-TSPTW results on the underlying raw files.

#### 2026-09-24

**Re-priced under the `td-fold/2` checker contract (mamut-routing-lib 0.12.0), no route changed.** mamut-routing-lib 0.12.0 replaces the TD checker's route fold (checker contract `td-fold/1` -> `td-fold/2`). Waiting and service at a vertex are now applied exactly to the accumulated arrival times instead of being composed through a ratio interpolation, the departure window restricts the first arc without interpolation, and travel on slope-one pieces is computed by addition. Under `td-fold/1` a ready time could be off by an ulp, and on stepwise travel-time functions such an ulp could land past a step and read its upper branch. All 168 BKS were re-priced with `mamut-routing bks reprice-td`. No cost moved, and 2 files were rewritten only to refresh `route_durations` / `route_departure_times` by ulps.


#### 2026-07-08

129 of 168 BKS improved (mean -0.17%, largest single improvement -0.84%) by a 20,808-run anytime-strategy head-to-head campaign on Grid'5000: kayros 0.4.0.dev0 (TD-ILS, TD-ACO+LS, and an ACO-then-ILS budget split, all over the granular time-dependent local search), per-size time limits (120 s for n<=30, 300 s for n<=60, 600 s for n<=100), seeds {42, 123, 456}, single-threaded runs. Improve-only fold: for each instance the campaign-best solution was re-priced by the canonical checker before writing (checker cost authoritative). Stored BKS marked proven optimal were left untouched.

#### 2026-07-06 (local-search sweep)

All 168 BKS improved by the first sweep of kayros 0.2.0.dev0 TD-ACO with time-dependent local search (tree-evaluated VND, every accepted move repriced by the checker-identical fold), 10 seeds per instance on Grid'5000.

#### 2026-07-06 (initial seeding sweep)

Initial BKS population, 168/168: the TDVRP variant had no earlier heuristic solutions, so all BKS come from the initial large-scale seeding sweep across all 4 TD families run on 2026-07-04 (kayros 0.0.1 TD-ACO, Grid'5000, 10 seeds per instance, 13 520 runs total).

#### 2026-07-03

Family populated (168 curated instances + ATF sidecars), no BKS yet.
