# Extracted Bcl-x and BIM data — status

## BIM — COMPLETE
- **67/67 sequences** from Liu et al., Oncotarget 2017, Supplementary Table 1 (PMC5652800)
- Files: `results/bim_aso_walk.csv`, merged into `results/aso_literature_dataset.csv`
- Labels: 8 active, 5 counter_therapeutic, 8 weak, 46 neutral_or_other
- Panel size after merge: **85** sequences (3 genes)

## Bcl-x — PARTIAL (unchanged)
- 1 full scrambled control sequence recovered from patent text
- Additional actives quantitatively described but sequences still in non-text patent tables
- Not merged as a full walk into the scoring panel

---

## Prior extraction notes

# Extracted Datasets: Bcl-x and BIM ASO Walks

Priority 1 and 2 extraction, per Prof. Yokota's request. **Honest scope note
up front**: for both sources, the actual nucleotide sequences for the
systematic walks are stored in patent/paper tables that are embedded as
images or table objects, not as extractable text — repeated fetch attempts
on both primary sources confirmed this. What follows is exactly what could
and could not be verified from accessible text, with no sequence or value
invented for the gaps.

---

## Priority 1: Bcl-x (US Patent 6,214,986, "Antisense modulation of bcl-x
expression" — a related, more specific patent than originally catalogued
6,210,892; the two are part of the same Ionis/Isis patent family)

### Sequences with full nucleotide sequence available (extracted verbatim)

| ID | Sequence (5'→3') | Position/target | Label | Quantitative effect | Source |
|---|---|---|---|---|---|
| ISIS 15691 | GACATCCCTTTCCCCCTCGG | scrambled, no defined bcl-x target | **inactive** (negative control) | <20% reduction in bcl-x expression, p<0.3 (not significant); vs. ISIS 16009's ~90% reduction (p<0.01) in the same SCID-hu xenograft experiment (n=3) | Patent 6,214,986, Example 24-26 (SEQ ID No. 41) |

### Sequences with confirmed quantitative activity but sequence NOT extracted (locked in table images)

| ID | Position/target | Label | Quantitative effect (from accessible text) | Sequence status |
|---|---|---|---|---|
| ISIS 22783 | Exon 1 of bcl-xl transcript (not bcl-xs) | **active** | Changes bcl-xs:bcl-xl ratio from 17% to 293% without reducing total bcl-x mRNA (Example 28) | Not extracted — Table 7/8 |
| ISIS 15999 | Bcl-x (region not specified in accessible text) | **active** | IC50 < 25 nM; 70% reduction at 25 nM, >90% at 50-200 nM (Example 19) | Not extracted — Table 3/5 |
| ISIS 16009 | Bcl-x (region not specified in accessible text) | **active** | IC50 40-50 nM; ~90% reduction in vivo (SCID-hu, n=3, p<0.01); 46% increase in caspase-3 activation | Not extracted — Table 3/5 |
| 12-oligo 5'ss walk (Table 14-16, patent 6,210,892) | 5' ends at 24, 26, 29, 31, 33, 37, 39, 41, 43, 44, 45, 47 nt upstream of bcl-x 5' splice site | not determined | Referenced as an ASO walk with a bcl-xs/bcl-xl ratio readout per position; individual per-position values not reached in this fetch (document truncated before Examples section) | Not extracted — positions only |

**Bcl-x count: 1 sequence + label extracted; 3 additional real, quantitatively-described active compounds identified with confirmed effect sizes but no extractable sequence; 1 walk (12 positions) confirmed to exist with design parameters but no values extracted.**

---

## Priority 2: BIM (Liu, Bhadra et al. 2017, *Oncotarget* 8:77567-77585,
open access, PMC5652800 / DOI 10.18632/oncotarget.20658)

**Update**: the full open-access main text was successfully retrieved in this
round (previously only the abstract was available). This confirms, with
certainty, that **all 67 ASO sequences 

...(truncated in git history if needed)
