# What to post

This file decides which papers go on the site. The scheduled task reads it on every run.
Edit it to change what gets picked. The task's steps are in `README.md`.

## Main topics

Every post has exactly one main topic, and it goes first in `topics`. The main topic is the heading the page shows for the post.

| Main topic | What counts |
|---|---|
| `Chordoma` | Any paper that mentions chordoma in its title or abstract, of any kind and from any journal or preprint server: molecular, clinical, surgical, radiotherapy, case reports, and mixed series (e.g. spine tumours) that include chordoma cases. Always overrides the other topics. |
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

## Tabs

The site has four main tabs. Every post belongs to exactly one:

| Tab | What goes in it |
|---|---|
| **Tier 1** | Research papers from Tier 1 journals, and reviews from the review journals. The featured row is here. |
| **Tier 2** | Research papers from Tier 2 journals. |
| **Preprints** | Preprints from bioRxiv and medRxiv, except chordoma preprints. |
| **Chordoma** | Every chordoma post, whatever the journal, plus chordoma preprints. |

## Preprints (bioRxiv)

- Preprints have no journal to go on, so judge them like Tier 2: post only if the paper is a close match to a main topic.
- Preprints go in the Preprints tab. Chordoma preprints go in the Chordoma tab.
- Never make a preprint `featured`.
- Categories to scan every run: `bioinformatics`, `genomics`, `cancer biology`, `genetics`.
- `search_preprints` has no keyword search, so finding chordoma preprints means reading every title in those categories, including `cancer biology`.
- A revised version (v2, v3, ...) of an older preprint counts as new if its DOI has never been posted.

## How many to post

- Each run looks back **7 days**, not just since the last run. Anything that fits and hasn't been posted yet can still be picked, so a paper missed one day can be caught the next.
- Each run adds at most, per tab:
  - **Tier 1: 10 papers** (research papers and reviews together). Rank by approach (computational, mixed, experimental), then by how closely they match. Reviews rank after research papers.
  - **Tier 2: 10 journal papers**, ranked the same way.
  - **Preprints: 10 preprints**, ranked the same way.
  - **Chordoma: 10 papers**. Prefer molecular and genomic work, then clinical studies, then case reports.
- Posting nothing is fine on slow days. Never lower the bar to fill a tab's limit.

## PubMed searches

Search by entry date (`datetype: "edat"`) over the last 7 days. PubMed refuses a query with more than 20 boolean operators (`AND`, `OR`, `NOT` all count), so use exactly these searches. Page with `retstart` until every result has been read.

`[ta]` matches the exact journal: `"Nature"[ta]` does **not** match Nature Communications or other Nature journals. What it does match is news, corrections and comments, which the `NOT` part removes. When you check a paper's metadata, also drop anything whose `article_types` is News, Comment, Editorial, Published Erratum or Letter.

Let `RESEARCH` = `hasabstract NOT (News[pt] OR Comment[pt] OR Editorial[pt] OR "Published Erratum"[pt] OR Letter[pt])`.

1. **Tier 1, no topic keywords** (read every title; this is how off-topic computational papers are found):
   `("Nature"[ta] OR "Science"[ta] OR "Cell"[ta] OR "Nat Genet"[ta] OR "Nat Methods"[ta] OR "Nat Biotechnol"[ta] OR "Cancer Cell"[ta] OR "Nat Cancer"[ta] OR "Cancer Discov"[ta] OR "Mol Cell"[ta] OR "Nat Struct Mol Biol"[ta] OR "Nat Cell Biol"[ta]) AND RESEARCH`
2. **Tier 2 specialist journals, no topic keywords:**
   `("Genome Biol"[ta] OR "Genome Res"[ta] OR "Cell Genom"[ta] OR "Genome Med"[ta] OR "Nucleic Acids Res"[ta] OR "RNA"[ta] OR "Genes Dev"[ta] OR "Nat Med"[ta] OR "Nat Comput Sci"[ta] OR "Cell Rep Methods"[ta]) AND RESEARCH`
3. **Tier 2 systems, cancer and computational journals, no topic keywords:**
   `("Cell Syst"[ta] OR "Mol Syst Biol"[ta] OR "Cancer Res"[ta] OR "Clin Cancer Res"[ta] OR "Bioinformatics"[ta] OR "Nat Mach Intell"[ta]) AND RESEARCH`
4. **Tier 2 broad journals, RNA and methods keywords** (these journals are too large to read in full):
   `("Nat Commun"[ta] OR "Sci Adv"[ta] OR "Proc Natl Acad Sci U S A"[ta] OR "Elife"[ta]) AND hasabstract AND (splicing OR isoform* OR "long-read" OR nanopore OR "single-cell" OR "spatial transcriptomics" OR m6A OR "RNA modification" OR deconvolution OR benchmark*)`
5. **Tier 2 broad journals, cancer genomics keywords:**
   `("Nat Commun"[ta] OR "Sci Adv"[ta] OR "Proc Natl Acad Sci U S A"[ta] OR "Elife"[ta]) AND hasabstract AND (cancer OR tumor OR tumour) AND (genomic* OR transcriptom* OR "single-cell" OR "whole genome" OR "copy number" OR epigenom*)`
6. **Review journals:**
   `("Nat Rev Genet"[ta] OR "Nat Rev Cancer"[ta] OR "Nat Rev Mol Cell Biol"[ta] OR "Trends Genet"[ta]) AND Review[pt]`
7. **Chordoma, any journal:** `chordoma[tiab]`

Add or change keywords in searches 4 and 5 as needed, but count the operators.
