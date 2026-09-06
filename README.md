# Rare-Disease-MVA-Hackathon-2026
Repository for the Sage Bionetworks / MVA Society / Hugging Face Rare Disease Hackathon 2026 variant prioritization (Track 1) and drug repurposing analysis (Track 2) for a clinically undiagnosed case of Mosaic Variegated Aneuploidy (MVA).

# MVA Rare Disease Hackathon 2026: Victoria Rios-Vazquez

Submission repository for the **Sage Bionetworks × MVA Society × Hugging Face** Rare Disease Hackathon, addressing a real, clinically undiagnosed case of Mosaic Variegated Aneuploidy (MVA).

> This work was made possible through the Hackathon, organized by Sage Bionetworks in partnership with the MVA Society, Hugging Face, and BEACON (The Benchmarking, Evaluation, and Assessment Consortium for Science), with prize sponsorship from AWS and Anthropic. We are deeply grateful to the child and their family who generously contributed their data and their story to advance research into this rare disease. We acknowledge their trust in making this Hackathon possible.

---

## Overview

This repository contains our submissions for both hackathon tracks:

| Track | Goal | Status | Link |
|---|---|---|---|
| 🧬 Track 1 — Variant Prediction | Predict the disease-causing variant(s) from genomic + phenotypic data | [in progress / submitted] | [`track1_variant_prediction/`](./track1_variant_prediction) |
| 💊 Track 2 — Drug Repurposing | Propose approved-drug candidates based on the disrupted mechanism | [in progress / submitted] | [`track2_drug_repurposing/`](./track2_drug_repurposing) |

**Team:** Victoria Rios-Vazquez
**Display name on leaderboard:** victoriariosvzz
**Institution(s):** Radboudumc

---

## Repository structure

```
track1_variant_prediction/   Code, notebooks, submissions, and report for Track 1
track2_drug_repurposing/     Code, notebooks, and report for Track 2
shared/                      Notes and utilities used by both tracks (no raw data)
docs/                        Methods descriptions, LLM disclosure, data deletion log
```

See each track's own README for details on approach and how to reproduce results.

---

## Data access & handling

This project uses the gated MVA Hackathon dataset (~85 GB, single-subject WGS + VCF + HPO phenotype terms), accessed under the Hackathon Data Transfer Agreement.

- **No raw data is stored in this repository.** All `.vcf`, `.vcf.gz`, `.bam`, `.cram`, and `.fastq` files are excluded via `.gitignore`.
- Data was accessed only through the approved Hugging Face gated-dataset mechanism and was not redistributed through any other channel.
- Per the Hackathon Rules, all data will be deleted from all personal/cloud environments within 30 days of Hackathon close, and deletion will be confirmed by email to the organizers. See [`docs/data_deletion_log.md`](./docs/data_deletion_log.md).

---

## Reproducing our results

1. Request your own access to the dataset via the [Hackathon's Hugging Face dataset page].
2. Clone this repo and install dependencies:
   ```bash
   git clone https://github.com/your-org/your-repo.git
   cd your-repo
   pip install -r track1_variant_prediction/requirements.txt
   ```
3. Set your own `HF_TOKEN` (never commit tokens) and run the download script:
   ```bash
   python track1_variant_prediction/src/download_data.py
   ```
4. Follow the notebooks in each track folder in order.

Full environment details (tool versions, compute used) are in each track's `config/` folder.

---

## AI assistance disclosure

Per the Hackathon's 28 Aug 2026 update, any use of LLM/AI assistance is documented in [`docs/llm_disclosure.md`](./docs/llm_disclosure.md) and referenced in both track reports.

---

## License

Submission code, reports, and predictions in this repository are released under **CC BY 4.0**, per the Hackathon's submission-reuse terms. The underlying dataset itself is **not** included and remains subject to the Hackathon Data Transfer Agreement - it is gated, non-redistributable, and must be independently requested.

## Citation

If referencing this work, please cite the Hackathon dataset using the reference provided on the Hackathon's Synapse page at the time of publication, and include the acknowledgement above.
