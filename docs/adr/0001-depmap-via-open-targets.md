# DepMap data reaches OASE through Open Targets, not DepMap itself

The DepMap Tool gets per-gene CRISPR **Gene Effect** data from the Open Targets GraphQL API, pinned to Open Targets' data version. It does not call the DepMap portal or download a DepMap release. As of 2026-10-22, every DepMap portal endpoint (including the advertised no-captcha download API) returns a Cloudflare challenge. The only current public bulk copy is a whole-genome matrix on Figshare (see below). Open Targets is per-gene like every other OASE Tool, and OASE already queries that host for genetics. We accept two known gaps: dependency probability is not exposed, and the underlying DepMap release is not stated. Both are reported as gaps, never filled in.

## Considered Options

- **Figshare 24Q4 bulk files**: rejected. They include dependency probability, but are two years stale, about 430 MB each, and would make this the first OASE Tool that caches a bulk release.
- **Figshare "Chronos parameters (Public 26Q1)"** (doi 10.6084/m9.figshare.31660582, linked from broadinstitute/chronos; CC BY 4.0, versioned with checksums): rejected as the fetch route. It is one ~420 MB whole-genome matrix with no dependency probability. See `docs/research/depmap-omics-data-folder.md`.
- **Wait for DepMap API keys**: rejected. Staff said "likely after 26Q3", with no date.
- **broadinstitute/depmap_omics**: not a data route. It is the Broad's Terra pipeline for processing omics releases and has no licence file. CRISPR Gene Effect comes from Chronos (broadinstitute/chronos, BSD-3) instead.

## Consequences

- When DepMap ships scriptable API access, revisit this. The `dependency_probability` threshold is UNRESOLVED until then.
- DepMap's newer terms reportedly restrict commercial use. The PR raises this for the maintainers to decide rather than settling it.
