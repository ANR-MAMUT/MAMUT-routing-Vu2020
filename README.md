# Vu2020 benchmark family (TDVRPTW and TDVRP, Arigliano-generator TD travel times, larger sizes, curated VRP variant)

Benchmark family repository of [MAMUT-routing](https://github.com/ANR-MAMUT/MAMUT-routing) (ANR MAMUT, ANR-22-CE22-0016), mounted there as `benchmarks/Vu2020`. It holds the 2 problem variants of the family, TDVRPTW (time windows kept) and TDVRP (time windows removed). Self-contained: instances, canonical arrival-time-function (ATF) sidecars and best-known solutions ship together.

## What this family is, and what it is not

The raw data extends the Arigliano et al. **TD-TSPTW** generator (same IGP model, 73 speed zones, 3 shared speed profiles) to larger sizes, as used by Vu et al. (2020): sizes 59/79/99 customers, congestion depth Delta ∈ {70, 80, 90, 98}, traffic patterns A/B, absolute TW widths ∈ {40, 60, 80, 100, 120, 150, 180}, 5 instances each. The result is an 840-instance full factorial with no demands, capacities, fleet sizes or service times. Instance names keep the `Vu-` prefix, and the recorded `instance_origin` is `Ari2018` (same generator).

This canonical family is the **VRP variant curated by Onyr (2024–2026)**. Demands, vehicle capacities, fleet sizes and gaussian service times were generated once by a former generation pipeline and are pinned here verbatim (recorded per instance in `metadata.legacy_attribute_source`). The TDVRPTW variant keeps the raw time windows verbatim. 2 raw files have customer windows ending after the depot due date. They are kept, and the unusable tail is cut by ATF-domain restriction at check time. The TDVRP variant removes the time windows (TDVRP twin). The family is therefore **not** comparable with published TD-TSPTW results on the underlying raw files.

## Curated subset (deterministic)

The family ships **168 instances** per variant, not the full 840. It keeps one instance per structural cell `(n, Delta, pattern, width)`, candidates holding a best-known solution of a former heuristic BKS store first, remaining ties by ascending SHA-256 of the canonical instance name. Names `Vu-A<id>-p<Pattern>-d<Delta>-w<Width>` under `n=<customers>` carry all structural parameters. The TDVRPTW and TDVRP variants hold the same 168 instance names. The full raw trees remain available upstream.

## Layout

- `TDVRPTW/n=<customers>/` and `TDVRP/n=<customers>/` hold the instances (`<Name>.vrp.json`) and their best-known solutions (`<Name>.bks.Duration.json`) of each variant, for example `TDVRPTW/n=59/Vu-A1-pA-d70-w150.vrp.json` and `TDVRP/n=59/Vu-A1-pA-d70-w150.bks.Duration.json`.
- `sidecars/n=<customers>/` holds the ATF sidecars, stored once and shared by the 2 variants, for example `sidecars/n=59/Vu-A1-pA-d70-w150.atf.json.gz`. Each instance references its sidecar by a path relative to the repository root, with the SHA-256 of the sidecar content.
- `mamut-collection.json` at the root marks the repository as a family collection (`"layout_version": 2`).
- `history/` holds the commit maps of the former repositories (see the history section below).

## Format

- `<Name>.vrp.json`: the instance in the MAMUT shape, with depot 0, mandatory display `coordinates` (deterministic stress-layout embedding of the symmetric raw distance matrix, near-exact here, quality in `metadata.coordinates_method`), `horizon` `[0, depot due date]`, and a `td` block with the sidecar path and the sidecar's storage-independent sha256.
- `<Name>.atf.json.gz`: the canonical ground truth, one arrival-time NDCPWLF per arc, exact IGP consolidation (breakpoints at every speed-zone boundary crossing, exact forward Ichoua re-evaluation, last zone extended beyond the horizon). All sidecars are gzipped in this repository.
- `<Name>.bks.Duration.json`: best known solution under the `Duration` objective. Costs are always the canonical checker's output.

Population pipeline: `populate_td_ari_vu` v1 (curation tooling maintained outside this repository, links will be published with the benchmark paper).

## Library compatibility

This layout is read by `mamut-routing-lib` 0.13 and later, which is in development and not released yet. `mamut-routing-lib` 0.12 and earlier read the former layout, which stays available read-only in the archived former repositories [MAMUT-routing-TDVRP-Vu2020](https://github.com/ANR-MAMUT/MAMUT-routing-TDVRP-Vu2020) and [MAMUT-routing-TDVRPTW-Vu2020](https://github.com/ANR-MAMUT/MAMUT-routing-TDVRPTW-Vu2020).

## Provenance and licensing

Underlying raw data: the Arigliano-generator TD-TSPTW distribution at Vu et al. (2020) sizes. MAMUT-authored curation artifacts are distributed under the MIT license (see [LICENSE](LICENSE)). This notice does not relicense the underlying third-party benchmark definitions.

## Citation and benchmark authorship

The authors, in citation order, are Adrien Pichon, Florian Rascoussier, Romain Billot, Lina Fahed, Alexandru-Liviu Olteanu, Marc Sevaux and Christine Solnon. The 2 lead authors are ordered alphabetically by surname, followed by the scientific supervisors in alphabetical order. Their ORCID identifiers are recorded in [CITATION.cff](CITATION.cff).

The listed authors contribute the TDVRPTW and TDVRP adaptations of the larger Arigliano-generator TD-TSPTW instances used by Vu et al. (2020): generated demands, capacities, fleet sizes and service times, deterministic selection of 168 instances across the source parameters, and construction and validation of the published arrival-time-function representation. The TDVRPTW variant retains the original time windows. The TDVRP variant removes them to support comparison with the corresponding TDVRPTW instances. Cite this repository for these benchmark-design, generation and selection contributions, and cite the larger Arigliano-generator TD-TSPTW instances used by Vu et al. (2020) for the original travel-time data and generator. The pinned legacy attribute files document the generated values used in the adaptation. Individual solution-contributor credits are recorded with the solutions. Record the exact Git commit used in experiments.

## History of this repository

Until November 2026 the 2 variants lived in 2 repositories, `MAMUT-routing-TDVRPTW-Vu2020` and `MAMUT-routing-TDVRP-Vu2020`. Their histories are imported here. Each history was rewritten under the directory of its problem type (`TDVRPTW/` or `TDVRP/`), and a merge commit joins the 2. The rewrite gave every former commit a new identifier. `history/` holds 1 map per former repository, `history/commit-map-MAMUT-routing-TDVRPTW-Vu2020.tsv` and `history/commit-map-MAMUT-routing-TDVRP-Vu2020.tsv`. Each map is tab-separated, with the former commit identifier and the new one on each line, after a header line. See [CHANGELOG.md](CHANGELOG.md) for the reorganisation.
