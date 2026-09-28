# Candidate quantitative ASO-walk / mutagenesis datasets

Catalog for further expansion (search-and-organize). Scoring logic is not changed here.

## Fully extracted in this repo

| Source | Gene | Size | Status |
|--------|------|------|--------|
| Liu et al. Oncotarget 2017, Supp. Table 1 (PMC5652800) | BIM | **67 ASOs** | **Extracted** → `results/bim_aso_walk.csv`, merged into `aso_literature_dataset.csv` (panel n=85) |
| SMN2 / DMD literature + patent-sourced sequences | SMN2, DMD | 18 prior rows | In main panel |

## High priority, not fully sequence-extracted

| Source | Gene | Notes |
|--------|------|--------|
| US Patent family (bcl-x ASO walks / examples) | Bcl-x | Design and some quantitative effects documented; many sequences still in image/PDF tables |
| NAR 2022 SMN2 intron-7 pentamer saturation mutagenesis | SMN2 | ~1023 mutants at a fixed 5-nt window overlapping ISS-N1 critical region; needs supplementary table parse |
| IKBKAP/ELP1 tiling walk (NAR 2018) | IKBKAP | Different disease; tiling screen |
| DMD foundational / positional studies (Errington 2003; Aartsma-Rus 2023) | DMD | Walks / meta position–efficiency |

## Provisional

| Source | Gene | Notes |
|--------|------|--------|
| SDCCAG8 ASO walk (bioRxiv preprint) | SDCCAG8 | Verify after peer review |

## Honest note

BIM extraction closed the largest accessible gap for a third gene with full nucleotide strings. Further “substantially larger multi-target” growth still depends on parsing patent/supplement tables (Bcl-x, SMN2 pentamer) or new public walks. Prefer curated quantitative labels over inventing sequences.
