# Is `broadinstitute/depmap_omics/data` a DepMap data route?

Researched 2026-10-22 against `master` @ `80f54c2` (2026-01-20).

**TL;DR**
1. **No.** `data/` holds Terra pipeline configs, sample manifests and reference files for *omics* processing. It has no gene-effect data, and every bucket or Taiga ID it names refuses anonymous access.
2. **Partially, from next door:** `broadinstitute/chronos` links to a public, CC BY 4.0, versioned Figshare article, "Chronos parameters (Public 26Q1)". It contains a ~413–431 MB `gene_effect.csv`. So "Figshare stops at 24Q4" is not true for gene effect.
3. **ADR 0001:** keep Open Targets as the per-gene route. Correct the Figshare premise and add the Chronos article as a considered option.

## 1. What's in `data/`

There are 2,129 tree entries totalling about 220 MB (`git/trees/master?recursive=1`).

- **`19Q4/` … `24Q4/`, `21Q4v2/`, `24Q2-08-28-2024/`:** Terra workspace configs saved for each quarter: WDLs, `conf_*/inputs_*/outputs_*.json`, `all_configs.json` and `worflow_list.csv`. Each folder is 0.03–2 MB. `data/README.md`: "our saved workspace parameters … all the info that would be required to copy our workspace".
- **`25Q2/`:** holds only `omics_profile_to_sequencing_id.csv` (165 KB), a `PR-…`→`CDS-…` ID map.
- **Sample manifests:** `sample.tsv`, `sample_cn.tsv`, `sample_rna.tsv`, `sample_for_pons.tsv` (19 MB), `ccle2_bams.csv`, `CCLE2_bams_old.tsv`, `find_new_sequencing_samples.tsv`, `*_embargoed.txt`, `*_tohave.txt`.
- **Genomic references and masks:** `repeatMasker_*.bed`, `segDup*.bed`, `segdup_regions.csv` (18 MB), `str_hg38.bed`, `hg38_cytoband.gz`, `wes_agilent_hg19_baits.interval_list`.
- **Legacy CN segments:** `legacy_segments.csv` (21 MB) and `prevsegments.csv` (22 MB).
- **Gene and ID mapping:** `genemapping_19Q1.csv`, `new_and_old_hgnc_symbols.txt`, `genes_to_mask_minus_rescue_ensg.txt`, `naming.csv`, `duplicates.json`, `md5s.json`.
- **`genesets/`:** 17 `.gmt` files totalling 40 MB, including `msigdb.v7.2.symbols.gmt` (26 MB). There are also 363 QC PDFs in `rna_qc_plots/`.
- **CRISPR guide coordinates:** `avana_guides.bed`, `humagne_guides.bed`, `ky_*guides.bed`, `tkov_25Q2.bed` and `brunello_25Q2.bed` (38 MB). These are sgRNA loci, not screen results. `depmapomics/constants.py` `GUIDESBED` feeds them into the `OmicsGuideMutationsBinary*` outputs, which flag mutations that overlap guides.
- **No gene-effect or dependency matrix** is anywhere in `data/`.
- **Pointers to the real data:**
  - `data/README.md` says `gs://ccle_default_params` moved to `gs://ccleparams`.
  - `data/24Q4/WGSconfig/all_configs.json` cites `gs://ccleparams`, `gs://fc-…` / `gs://fc-secure-…` Terra buckets, and public Broad/GATK reference buckets.
  - `depmapomics/config_prod.py` names `gs://cclebams`, Taiga IDs (`depmap-a0ab`, `expression-d035`, `cn-latest-d8d4`, …) and Terra workspaces (`broad-firecloud-ccle/DepMap_hg38_RNAseq`, …). All of these are omics.

## 2. Public readability (anonymous, read-only)

- `storage.googleapis.com/storage/v1/b/{ccleparams,ccle_default_params,cclebams,fc-f36b…,fc-38a1…}/o` all return **401** ("Anonymous caller does not have storage.objects.list").
- Taiga `cds.team/taiga/api/dataset/depmap-a0ab` returns **403**.
- The successor bucket `depmap-omics-public-ref` returns **400**: "requester pays bucket". It holds pipeline references, not release data, according to DepMap's Devin McCabe on 2026-10-20 (forum.depmap.org/t/4646). That post also names a new code repo, `broadinstitute/depmap-omics-pipelines-public`.

## 3. Coverage

- The repo covers only omics: CN, mutation, expression, fusion and fingerprinting. The README calls it "code that processes data for the biannual DepMap data release". Upload and Taiga steps are "internal only", in `broadinstitute/depmap_omics_upload` (README.md, lines 75–186).
- CRISPR processing lives in `broadinstitute/chronos` (BSD-3-Clause, `LICENSE`).
- `chronos/Data/DepMapDataURLs.json` (commit `2ff2ba8`, 2026-03-24, "update with public 26Q1 params") links to Figshare files that `chronos/fetch_parameters.py` downloads.
- That Figshare article is **10.6084/m9.figshare.31660582**, "Chronos parameters (Public 26Q1)", CC BY 4.0. v1 was published 2026-03-11 and v2 on 2026-08-03. Each file has an MD5.
  - I range-read the first 800 KB of `gene_effect.csv` anonymously: v1 file 62677015 (431,100,903 B) and v2 file 67214582 (413,544,937 B). Both have 18,531 `SYMBOL (Entrez)` columns and `ACH-…` row IDs. The v2 values match v1 at lower precision.
  - A second article, `33148280`, "post 26Q1 Public with latest Chronos updates", exists too.
- **Unverified:** whether this `gene_effect.csv` equals the portal's `CRISPRGeneEffect` (copy-corrected and scaled). The Chronos docstring says only "DepMap release format". **Unverified:** row count. Neither article includes a dependency-probability file.

## 4. Licensing

- `depmap_omics` has **no licence**: `GET /repos/…/license` returns 404, the tree has no LICENSE/COPYING/setup.py, `pyproject.toml` has no `license` field, and the README states no terms.
- Default copyright therefore applies. OASE can cite and read it, but should not copy or redistribute its code or files.
- Third-party files such as MSigDB `.gmt` carry their own terms (**unverified**).
- Chronos code is BSD-3. The Figshare parameters are CC BY 4.0, per the article metadata. How that relates to DepMap's portal terms is **unverified**.

## 5. Release cadence

- There are 10 tags, 21Q2 through 24Q4, and 5 GitHub Releases (22Q2, 22Q4, 23Q2, 23Q4, 24Q2). The releases have **0 assets**, and their bodies are empty or one line ("23Q4 DepMap Omics release").
- Nothing has been tagged after 24Q4. The newest config folder is `data/25Q2`, and the last commit touching `data/` is `3a32fc9` (2025-04-11).
- Tags record pipeline state, not data. DepMap's jkobject says to look "at the corresponding commit (named 22Q1 for 22Q1…)" (forum.depmap.org/t/1241).
- No tag or release carries a data manifest.

## 6. Bottom line

- **The `depmap_omics/data` premise: no.** It is processing configuration, and its buckets are private.
- **Overall: partially.** Figshare 31660582 is public, scriptable over the Figshare REST API, and version-pinned by article version, file ID and MD5. It is also two years newer than the 24Q4 files. It does not beat Open Targets for OASE's per-gene pattern, though:
  - it is a ~420 MB bulk matrix, cell lines × genes, so one gene needs the whole file;
  - it has no dependency probability;
  - its equivalence to `CRISPRGeneEffect` is unverified.
- **Change ADR 0001's text, not the decision:**
  - Correct "Figshare stopped receiving releases at 24Q4" to say Chronos 26Q1 gene effect is on Figshare.
  - Add Figshare 31660582 as a considered option: rejected as bulk, possibly useful to cross-check which DepMap release Open Targets uses.
  - Keep the depmap_omics rejection as written.
