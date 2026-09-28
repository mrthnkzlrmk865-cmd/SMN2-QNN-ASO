# SMN2 splice-site models and ASO ranking under small real ground truth

A from-scratch computational study comparing classical and quantum models for RNA splice-site prediction, then testing sequence-based ASO scoring on a curated retrospective panel (now **85 sequences across SMN2, DMD, and BIM**).

**Author:** Murathan Kızılırmak (high school student, Turkey)  
**Contact:** mrthn.kzlrmk.865@gmail.com  
**Write-up draft:** short student manuscript (PDF/DOCX) prepared for feedback

---

## In one paragraph

With limited real biological labels, a simple position weight matrix (PWM) generalised better than a quantum neural network (QNN) and other higher-capacity models for multi-gene splice-site prediction. An ASO scoring pipeline based on complementarity can separate some real SMN2 silencer-targeting sequences from unrelated controls under holdout, but fails when two ASOs bind the same element with opposite effects, and fails cleanly on a full BIM ASO walk (gene-stratified holdout, n=33). Simple supervised sequence classifiers trained on SMN2/DMD do not transfer to BIM. Negative findings are reported as results. Larger multi-target quantitative datasets and features beyond raw sequence are the logical next steps—not further elaboration of the current score.

---

## Key results (honest)

| Area | Finding |
|------|---------|
| Splice-site prediction | PWM strongest under matched splits and gene-level holdout; QNN did not show a stable advantage |
| ASO score (SMN2 scope) | After redesign, can rank real silencer-targeting sequences above scrambled/unrelated controls; L14-type failure fixed only by a narrow, literature-based 10C rule |
| ASO score (BIM) | **No separation** on holdout (active ≈ inactive; p ≈ 0.89, n=33) |
| Supervised sequence ML | SMN2/DMD → BIM: transfer failure (AUC ≈ 0.44). Within BIM: weak ranking only (AUC ≈ 0.64–0.67), does not beat majority baseline on accuracy |
| Region scanning | Three transparent scanners did not reliably recover known SMN2 regulatory regions |

**Do not use pooled “overall p” without gene breakdown** — SMN2 signal plus a large neutral BIM mass can look significant while BIM itself is unsolved.

---

## Dataset status

### Splice-site data
- Genes: SMN2, BRCA1, TP53, CFTR, HBB
- Alignment-verified exon/intron boundaries; GT–AG checked
- See `data/` and early `src/01_*` scripts

### Retrospective ASO panel (`results/aso_literature_dataset.csv`)

| | Count |
|--|------:|
| **Total sequences** | **85** |
| Active | 18 |
| Inactive / other | 67 |
| SMN2 | 15 |
| DMD | 3 |
| **BIM** | **67** |

- BIM walk: Liu et al., *Oncotarget* 2017; Supplementary Table 1; PMC5652800
- Full sequences: `results/bim_aso_walk.csv`
- Activity detail: active (8 efficient), counter_therapeutic (5), weak (8), neutral_or_other (46)
- Dev/holdout assigned by fixed, label-blind rules before scoring

Bcl-x walk sequences remain only partially extracted (patent tables image-locked); see `EXTRACTED_BCLX_BIM_DATA.md` and `CANDIDATE_DATASETS.md`.

---

## What this project is not claiming

- Not a validated general ASO ranker for new candidates
- Not a recommendation to synthesise new oligos from the current score
- Not evidence that quantum ML beats classical methods on this task with current data size

---

## Repository layout

```
data/          genomic and mRNA FASTA
src/           numbered experiment scripts
results/       metrics, ASO panel CSV, BIM walk, JSON summaries
figures/       comparison and evaluation plots
README.md      this file
CANDIDATE_DATASETS.md   catalog of further quantitative walks
```

Main dependencies: see `requirements.txt`.

---

## How to read the work

1. Splice-site benchmarks → `figures/final_comparison.png`, gene-holdout figures, early `src/` scripts
2. ASO score diagnosis and redesign → mid `src/` scripts, scoring diagnosis figures
3. Expanded panel + gene-stratified evaluation → `results/aso_literature_dataset.csv`, holdout reports
4. Supervised transfer experiment → gene-stratified sequence-model metrics (SMN2→BIM vs within-BIM)

---

## Project stance (after expert feedback)

- Prefer **independent holdout** and **gene-stratified** reporting
- Prefer **growing real quantitative panels** over adding ad-hoc score features
- Treat **negative results** as conclusions
- Consolidate into a short write-up before wet-lab or synthesis claims

Guidance from Professor Toshifumi Yokota is gratefully acknowledged; remaining errors are the author’s.

---

## License / use

Code and curated tables are provided for reproducibility and educational review. Upstream sequences and activity labels belong to their original publications and patents; cite those sources when reusing the panel.
