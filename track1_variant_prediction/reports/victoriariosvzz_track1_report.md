# Track 1 Methods Report -- victoriariosvzz

**Team name:** victoriariosvzz
**Model number:** 1

## 1. Describe your model/approach in detail

A genome-wide, multi-pass, data-first variant prioritization pipeline, benchmarked
against current literature (Stenton et al. CAGI6 RGP; AI-MARRVEL; LA-MARRVEL/RareCollab
2026) before design. Pass 1: ~5M variants restricted to protein-coding CDS regions
(with a splice-site buffer), annotated via Ensembl VEP, modeled for autosomal
recessive inheritance, and coarsely ranked using consequence severity, gnomAD rarity,
and ontology-based phenotype similarity (pyhpo). Pass 2 (top 40 candidates): deep
pathogenicity scoring (CADD/REVEL/AlphaMissense), gene constraint (pLI/LOEUF from
gnomAD), a loss-of-heterozygosity structural-variant proxy, SpliceAI review links, and
an approximate ACMG/AMP-inspired evidence engine. Pass 3: the top 12 candidates and
their full structured evidence are passed to Claude for a holistic reranking pass,
mirroring the language-aware reranking architecture reported by LA-MARRVEL/RareCollab,
reasoning across all evidence types simultaneously rather than a fixed linear weighted
sum, including assigning each candidate's primary/secondary status directly, based
on its own judgment, not rank position. Literature review of the top candidate remains
the final manual-performed step.

## 2. AI assistance disclosure

Anthropic API, Claude Sonnet, standard pay-as-you-go API terms, no training on
patient data per Anthropic's standard commercial API terms. Used architecturally
as a holistic evidence-reranking step (Pass 3 above), disclosed explicitly because
it affects the submitted ranking itself.

## 3. Automated output or manual curation?

The submitted ranking is the fully automated output of the pipeline, including the
LLM reranking step. A manual literature review (ClinVar and functional-biology
literature search) of the top candidate was subsequently performed to confirm and
interpret the finding. This review informed the interpretation below but did not
alter the automated ranked list itself.

## 4. Public vs proprietary data

Publicly available data only, plus the Anthropic API (a commercial compute service,
not a proprietary dataset).

## 5. Public data sources used (detail)
- GENCODE basic annotation GTF (gencode.v50.basic.annotation.gtf.gz)
- Ensembl VEP REST API
- HPO project gene-phenotype annotations (via pyhpo)
- myvariant.info / dbNSFP (CADD, REVEL, AlphaMissense)
- gnomAD v2.1.1 gene constraint metrics (pLI, LOEUF)
- ClinVar (manual confirmation of top candidate)
- Broad Institute SpliceAI Lookup (manual review)

## 6. Proprietary data used

None (Anthropic API is a commercial service used under standard API terms, not a
proprietary data source).

## 7. Compound heterozygous pair output capability

Yes. Phasing is not confirmed (proband-only sequencing), stated as a limitation.

## 8. Secondary/incidental findings handling

Each candidate's finding_type (primary vs. secondary) is set directly by the LLM's
own holistic evidence review, not by rank position, so a candidate the review
itself flags as unlikely retains "secondary" even if it scored within the numeric
top ranks. One recurring pattern worth naming: several candidates clustered in the
MHC/HLA region on chromosome 6 (a region well documented for short-read
variant-calling artifacts due to extreme polymorphism), all correctly downweighted
to secondary by this review rather than filtered out upstream. A possible future
refinement would be an explicit MHC-region exclusion filter earlier in the pipeline.

## 9. Runtime and cost estimate

Approximately 39.9 minutes end-to-end. Colab free tier + free public
APIs; Anthropic API usage for one reranking call over 12 candidates (~$0.09).

## 10. Method abstract

A multi-pass pipeline explicitly designed against current (2024-2026) literature on
rare disease variant prioritization: genome-wide structural filtering, ontology-based
phenotype similarity, deep in-silico pathogenicity (CADD/REVEL/AlphaMissense), gene
constraint, an indirect structural-variation proxy, an approximate ACMG/AMP evidence
engine, and, mirroring the newest reported architecture in this space (LA-MARRVEL,
RareCollab), a final LLM-based holistic reranking pass over the fully computed
evidence table.

**Strengths:** every stage is grounded in a specific cited methodology rather than ad
hoc design; no candidate gene was selected from clinical literature before
computation. Phenotype information entered only through an ontology-based
similarity score, and literature review was deliberately deferred until after the
ranking was produced; the LLM reranking step reasons holistically across evidence
types in a way a fixed formula cannot; gene constraint and an ACMG-inspired evidence
engine close two gaps identified in earlier iterations of this approach.

**Limitations:** this is NOT a reimplementation of AI-MARRVEL/3ASC/LA-MARRVEL, which
are trained/production systems built over years; no true CNV calling or confirmed
segregation (no trio data available, so compound-heterozygous phasing is inferred,
not confirmed); the ACMG-lite tags are a simplified heuristic, not a certified
classification; the LLM reranking has not itself been benchmarked against a labeled
validation set.

**Top candidate and clinical interpretation:** The pipeline's single primary finding
is a compound-heterozygous BUB1B variant pair (chr15:40209701 T>G, stop_gained;
chr15:40220612 T>G, missense; EPCR=0.69), converging across every evidence layer:
variant consequence, gene constraint, ontology-based phenotype similarity, and the
LLM holistic reranking pass all ranked it first. Manual literature review confirmed
chr15:40209701 T>G (NM_001211.6:c.2210T>G, p.Leu737Ter) as an exact match to a
ClinVar entry already classified pathogenic for Mosaic variegated aneuploidy
syndrome 1 (MVA1) which provides a direct clinical confirmation.
The second variant, chr15:40220612 T>G (missense, AlphaMissense=0.923),
was not found in ClinVar or other queried databases and appears novel; however, a
truncating-plus-missense compound-heterozygous configuration is biologically
consistent with the BUB1B literature, where complete biallelic loss of function is
not compatible with survival and viable MVA1 cases require at least one
partially-functional (hypomorphic) allele. This fits the BUB1B-associated subtype of
MVA specifically: BUB1B is the only established MVA-causing gene with a strong
reported childhood-cancer link (rhabdomyosarcoma, Wilms tumor), distinguishing it
from the CEP57 and TRIP13 subtypes, neither of which carries a comparable cancer
association. All secondary candidates (IGBP1, HLA-B, and a cluster of HLA-DRB1 calls)
scored substantially lower and were independently flagged by the LLM reranking step
as biologically implausible or likely technical artifacts, the latter consistent with
the well-documented difficulty of variant calling in the highly polymorphic MHC
region.
