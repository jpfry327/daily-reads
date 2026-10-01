# What to post

This file decides which papers go on the site. The scheduled task reads it on every run.
Edit it to change what gets picked. The task's steps are in `README.md`.

## Main topics

Every post has exactly one main topic, and it goes first in `topics`. The main topic is the heading the page shows for the post.

| Main topic | What counts |
|---|---|
| `Chordoma` | Any paper about chordoma, of any kind and from any journal or preprint server: molecular, clinical, surgical, radiotherapy, case reports. Always overrides the other topics. |
| `Cancer genomics` | Genomic, transcriptomic or epigenomic analysis of tumours, in any cancer type. What matters is the approach, discovery or topic: a study of one cancer type is in if the idea would interest people working on other cancers. |
| `RNA biology` | Alternative splicing, RNA modifications (m6A, pseudouridine, inosine, A-to-I editing), RNA processing, decay, localisation and translation control. |
| `Methods` | Bioinformatics methods, tools and benchmarks of any kind, including general ones (aligners, pangenomes, workflow tools). Long-read, single-cell and spatial methods come first. |

If a paper fits more than one, choose in this order: Chordoma, Cancer genomics, RNA biology, Methods.

## Approach

Decide whether each paper is **computational** (no new wet-lab data: new methods, re-analysis of public data, modelling), **mixed** (new experiments plus substantial computational work), or **experimental** (mainly bench work). All three can be posted.

- Priority: computational first, then mixed, then experimental.
- Tag every purely computational paper `Computational`, so the reader can filter for them.
- **Tier 1 journals: post every purely computational biology paper**, even if it is outside the main topics. Give it the closest main topic, usually `Methods`. The point is to see what computational work makes it into those journals.

## Tags

After the main topic, add up to 3 tags from this list. Only use names from this list. If something important has no tag, leave it out and mention it in the run summary.

`Computational`, `Long-read`, `Single-cell`, `Spatial`, `Splicing`, `RNA modifications`, `Isoforms`, `Deconvolution`, `Machine learning`, `Benchmarking`, `Multi-omics`, `Epigenomics`, `Structural variants`, `Copy number`, `Tumour evolution`, `Immuno-oncology`, `Sarcoma`, `Pan-cancer`, `Clinical`, `Resource`

## Out of scope

Skip these unless they are about chordoma:

- plant, microbial, ecological and veterinary genomics
- protein structure prediction and design, unless it is directly about RNA
- papers that only apply standard tools to one cohort with no new method, resource or clear biological finding
- editorials, news, research highlights and commentaries
- reviews, except from the review journals listed below

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
| Molecular Cell | `Mol Cell` |
| Nature Structural & Molecular Biology | `Nat Struct Mol Biol` |
| Nature Cell Biology | `Nat Cell Biol` |

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
| Nature Medicine | `Nat Med` |
| Nature Computational Science | `Nat Comput Sci` |
| Cell Reports Methods | `Cell Rep Methods` |
| Bioinformatics | `Bioinformatics` |
| Nature Machine Intelligence | `Nat Mach Intell` (PubMed indexes only part of this journal) |
| Genes & Development | `Genes Dev` |

**Review journals: post reviews if on-topic.** Search these with `Review[pt]` so news items and research highlights are left out.

| Journal | PubMed `[ta]` |
|---|---|
| Nature Reviews Genetics | `Nat Rev Genet` |
| Nature Reviews Cancer | `Nat Rev Cancer` |
| Nature Reviews Molecular Cell Biology | `Nat Rev Mol Cell Biol` |
| Trends in Genetics | `Trends Genet` |

**Every other journal: skip**, unless the paper is about chordoma.

Save the tier on each post as `"tier": 1`, `"tier": 2` or `"tier": "review"`. Chordoma papers from other journals get `"tier": null`. Reviews count toward the 10-journal-paper limit and rank after Tier 1 research papers.

## Preprints (bioRxiv)

- Preprints have no journal to go on, so judge them like Tier 2: post only if the paper is a close match to a main topic.
- Preprints are kept out of the main feed. They only show when the reader picks "Preprints" under "Show". Chordoma preprints also show in the Chordoma section.
- Never make a preprint `featured`.
- Categories to scan every run: `bioinformatics`, `genomics`, `cancer biology`, `genetics`.
- `search_preprints` has no keyword search, so finding chordoma preprints means reading every title in those categories, including `cancer biology`.

## How many to post

- Each run looks back **7 days**, not just since the last run. Anything that fits and hasn't been posted yet can still be picked, so a paper missed one day can be caught the next.
- Each run adds at most:
  - **10 journal papers**. If there are more candidates, rank by tier first, then by approach (computational, mixed, experimental), then by how closely they match. So the order is Tier 1 computational, Tier 1 mixed, Tier 1 experimental, Tier 2 computational, and so on.
  - **10 preprints**
  - **no limit on chordoma papers**
- Posting nothing is fine on slow days. Never lower the bar to fill the quota.

## PubMed searches

Search by entry date (`datetype: "edat"`) over the last 7 days. Run one search per main topic, each limited to the Tier 1 and Tier 2 journals with `[ta]`, one search of the review journals with `Review[pt]` added, and one search for `chordoma` with no journal limit. Page with `retstart` until every result has been read.

Starting keywords per topic. Add or change keywords here as needed.

- Cancer genomics: `(cancer OR tumor OR tumour OR neoplasm) AND (genomic* OR transcriptom* OR "whole genome" OR "single-cell" OR mutation* OR "copy number" OR epigenom*)`
- RNA biology: `splicing OR "RNA modification" OR m6A OR pseudouridine OR "RNA editing" OR isoform* OR "RNA-binding protein" OR "mRNA decay"`
- Methods: `"long-read" OR nanopore OR PacBio OR "single-cell" OR "spatial transcriptomics" OR deconvolution OR benchmark* OR "computational method"`

Also run one search of the Tier 1 journals for computational work outside the main topics, e.g. `("computational" OR "machine learning" OR "deep learning" OR algorithm OR "foundation model" OR "re-analysis")`.
