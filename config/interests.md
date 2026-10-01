# What to post

This file decides which papers go on the site. The scheduled task reads it on every run.
Edit it to change what gets picked. The task's steps are in `README.md`.

## Main topics

Every post has exactly one main topic, and it goes first in `topics`. The main topic is the heading the page shows for the post.

| Main topic | What counts |
|---|---|
| `Chordoma` | Any paper about chordoma, of any kind and from any journal or preprint server: molecular, clinical, surgical, radiotherapy, case reports. Always overrides the other topics. |
| `Cancer genomics` | Genomic, transcriptomic or epigenomic analysis of tumours. Computational methods and large analyses come first, but experimental cancer genomics counts too. |
| `RNA biology` | Alternative splicing, RNA modifications (m6A, pseudouridine, inosine, A-to-I editing), RNA processing, decay, localisation and translation control. |
| `Methods` | Bioinformatics methods, tools and benchmarks, especially for long-read, single-cell and spatial data. |

If a paper fits more than one, choose in this order: Chordoma, Cancer genomics, RNA biology, Methods.

## Tags

After the main topic, add up to 3 tags from this list. Only use names from this list. If something important has no tag, leave it out and mention it in the run summary.

`Long-read`, `Single-cell`, `Spatial`, `Splicing`, `RNA modifications`, `Isoforms`, `Deconvolution`, `Machine learning`, `Benchmarking`, `Multi-omics`, `Epigenomics`, `Structural variants`, `Copy number`, `Tumour evolution`, `Immuno-oncology`, `Sarcoma`, `Pan-cancer`, `Clinical`, `Resource`

## Out of scope

Skip these unless they are about chordoma:

- plant, microbial, ecological and veterinary genomics
- protein structure prediction and design, unless it is directly about RNA
- papers that only apply standard tools to one cohort with no new method, resource or clear biological finding
- reviews, editorials and commentaries

## Journal tiers (PubMed)

The journal is the main signal for journal papers. Being on-topic is not enough on its own.

**Tier 1: post if on-topic.**

| Journal | PubMed `[ta]` |
|---|---|
| Nature | `Nature` |
| Science | `Science` |
| Cell | `Cell` |
| Nature Genetics | `Nat Genet` |
| Nature Methods | `Nat Methods` |
| Nature Biotechnology | `Nat Biotechnol` |
| Cancer Cell | `Cancer Cell` |
| Nature Cancer | `Nat Cancer` |
| Cancer Discovery | `Cancer Discov` |
| Nature Medicine | `Nat Med` |
| Molecular Cell | `Mol Cell` |
| Nature Structural & Molecular Biology | `Nat Struct Mol Biol` |

**Tier 2: post only if it is a close match to a main topic.** A close match means the paper's main point is one of the topics above, not that it uses one of the techniques along the way.

| Journal | PubMed `[ta]` |
|---|---|
| Genome Biology | `Genome Biol` |
| Genome Research | `Genome Res` |
| Cell Genomics | `Cell Genom` |
| Genome Medicine | `Genome Med` |
| Nature Communications | `Nat Commun` |
| Science Advances | `Sci Adv` |
| Cell Systems | `Cell Syst` |
| Molecular Systems Biology | `Mol Syst Biol` |
| eLife | `Elife` |
| PNAS | `Proc Natl Acad Sci U S A` |
| Nucleic Acids Research | `Nucleic Acids Res` |
| RNA | `RNA` |
| Cancer Research | `Cancer Res` |
| Clinical Cancer Research | `Clin Cancer Res` |

**Every other journal: skip**, unless the paper is about chordoma.

Save the tier on each post as `"tier": 1` or `"tier": 2`. Chordoma papers from other journals get `"tier": null`.

## Preprints (bioRxiv)

- Preprints have no journal to go on, so judge them like Tier 2: post only if the paper is a close match to a main topic.
- Preprints are kept out of the main feed. They only show when the reader picks "Preprints" under "Show". Chordoma preprints also show in the Chordoma section.
- Never make a preprint `featured`.
- Categories to scan every run: `bioinformatics`, `genomics`, `cancer biology`, `genetics`.
- `search_preprints` has no keyword search, so finding chordoma preprints means reading every title in those categories, including `cancer biology`.

## How many to post

- Each run looks back **7 days**, not just since the last run. Anything that fits and hasn't been posted yet can still be picked, so a paper missed one day can be caught the next.
- Each run adds at most:
  - **10 journal papers** (Tier 1 first, then Tier 2, then the closest matches)
  - **10 preprints**
  - **no limit on chordoma papers**
- Posting nothing is fine on slow days. Never lower the bar to fill the quota.

## PubMed searches

Search by entry date (`datetype: "edat"`) over the last 7 days. Run one search per main topic, each limited to the Tier 1 and Tier 2 journals with `[ta]`, plus one search for `chordoma` with no journal limit. Page with `retstart` until every result has been read.

Starting keywords per topic. Add or change keywords here as needed.

- Cancer genomics: `(cancer OR tumor OR tumour OR neoplasm) AND (genomic* OR transcriptom* OR "whole genome" OR "single-cell" OR mutation* OR "copy number" OR epigenom*)`
- RNA biology: `splicing OR "RNA modification" OR m6A OR pseudouridine OR "RNA editing" OR isoform* OR "RNA-binding protein" OR "mRNA decay"`
- Methods: `"long-read" OR nanopore OR PacBio OR "single-cell" OR "spatial transcriptomics" OR deconvolution OR benchmark* OR "computational method"`

## Still to fill in

- Labs and authors to always include: _none yet_
- Topics to always exclude beyond the list above: _none yet_
