# Glossary

Vocabulary for the campaign to contribute to Google Cloud's open agentic science engine and turn that work into a path to a Google role.

## Goal

**Target Role**
A Customer Engineer position in Google Cloud's life sciences customer engineering org (Joe Timmons's team): customer-facing, technical, deploying AI for pharma and biotech.
_Avoid_: "working with Google" (too vague; also covers partner/consultant and research roles, which are not the goal)

## Contribution

**OASE**
The Open Agentic Science Engine: Google Cloud's Apache-2.0 multi-agent drug-discovery research engine, an application inside the GoogleCloudPlatform/LifeSciences repository.
_Avoid_: "the engine repo" (OASE is not its own repository)

**Upstream**
The official GoogleCloudPlatform/LifeSciences repository, where OASE contributions must ultimately be merged.

**Working Copy**
The private copy of OASE inside this repo, where contributions are drafted. It is pinned to a recorded Upstream commit.
_Avoid_: "fork" (the fork is a separate thing, used only to deliver a Port)

**Port**
Moving a finished change from the Working Copy onto a branch of the fork, based on latest Upstream, so it can be opened as a PR.

**Tool**
An OASE subcommand that fetches from one data source or runs one computation, records provenance for everything it produces, and reports **Blocked** rather than inventing a value. The unit a **Shared Rails** contribution adds.
_Avoid_: "connector", "integration", "plugin"

**Blocked**
The outcome a **Tool** reports when its data source or dependency is unavailable. It is a legitimate result, not a failure to hide.

**Shared Rails**
Path 2 of the engine's partner call: wiring a high-quality public dataset into an engine tool so every user starts from a stronger baseline. The chosen contribution path.
_Avoid_: "data partnership" (implies proprietary data, Path 1)

## Science

**Stage 1 Gate**
The first of OASE's four pre-clinical gates: is there enough causal evidence for the target, and is it tractable for the chosen modality? DepMap evidence feeds its functional-validation criterion.

**Gene Effect**
DepMap's Chronos score for how much knocking out a gene impairs a cell line's growth. Scaled so the median common-essential gene scores -1 and the median nonessential gene scores 0.
_Avoid_: "essentiality score", "CERES score" (CERES is the retired predecessor)

**Dependency**
A cell line *depends on* a gene when DepMap's dependency probability is at least 0.5. This is distinct from *noteworthy depletion* (gene effect below -0.5), which is a cutoff on the score rather than a probability.
_Avoid_: using "dependent" for a gene-effect cutoff

**Common Essential**
A gene that nearly every cell line depends on, per DepMap's own call. A property of the gene across lines, not of one cell line. Being common essential is a liability for a drug target, not a strength.
_Avoid_: "essential gene" (ambiguous between one line and all lines)

## People

**Reviewer Tier**
The order in which the campaign seeks attention: (1) maintainers who review and merge PRs (in practice Nathan Hodson, with Preston Holmes also committing), (2) Joe Timmons, (3) the wider LinkedIn TechBio audience. Tiers 1 and 2 are approached in parallel. Tier 3 waits until the work has merged Upstream.
