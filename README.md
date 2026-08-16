# Quantum Neural Networks for SMN2 Splice-Site Prediction and ASO Candidate Design

A from-scratch pipeline comparing classical and quantum machine learning models for
predicting RNA splice sites across five human genes, applied to antisense
oligonucleotide (ASO) candidate scoring for SMN2 exon-7 inclusion therapy.

## About this project

I'm a high school student in Turkey with a strong interest in computational biology,
quantum machine learning, and their intersection with genetic medicine — this project
grew out of curiosity about Spinal Muscular Atrophy (SMA) and the ISS-N1 splicing
mechanism that nusinersen (Spinraza) exploits therapeutically. I built the full
pipeline myself, from raw NCBI sequence data to trained quantum circuits, without
using pre-built bioinformatics libraries for the core splice-site detection logic.

## What was done

1. **Real, multi-gene ground truth.** Genomic and mRNA sequences for five genes
   (SMN2, BRCA1, TP53, CFTR, HBB) were aligned using a k-mer seed-and-extend
   algorithm to recover exact exon/intron boundaries. All 68/68 introns found were
   independently confirmed against the canonical GT...AG splicing rule — no labels
   were assumed or hand-picked.
2. **Quantum feature encoding.** Nucleotide windows around each splice site were
   mapped to Bloch-sphere rotation angles and fed into a 4-qubit (and, in ablation
   experiments, 2/6/8-qubit) parametric quantum circuit trained under a simulated
   IBM hardware noise model.
3. **Systematic model comparison**, all on the same stratified train/test split:
   - A majority-class baseline
   - Classical SVM and MLP (matched to the same feature budget as the QNN)
   - The trained QNN
   - A from-scratch Position Weight Matrix (PWM) classifier using the full
     22-nt window
   - A from-scratch 1st-order Markov (Weight Array Model) classifier — the
     methodological precursor to MaxEntScan
4. **Robustness checks:** a qubit-count ablation (2/4/6/8 qubits), 5-fold
   cross-validation on the best-performing quantum configuration, and four
   class-balancing strategies (random oversampling, SMOTE, random undersampling,
   class weighting).
5. **Dynamic ASO candidate scoring** for the SMN2 ISS-N1 region, combining the
   trained QNN's output probabilities with a real nearest-neighbor thermodynamic
   free-energy calculation and an off-target alignment scan — no hardcoded scores.

**Key finding:** the trained QNN performed comparably to classical general-purpose
ML models (SVM, MLP) under an identical, limited feature budget, but a much simpler
domain-specific model — a plain PWM using the *full* sequence window — outperformed
all of them by a wide margin. Cross-validation further showed that any single-split
advantage the QNN appeared to have over classical models did not hold up under
repeated resampling. The central conclusion of this project is therefore **not**
about quantum vs. classical superiority, but about **matching model complexity to
the amount of real biological ground truth available** — a lesson that emerged
directly from the data, not assumed in advance.

## Main results

All models evaluated on an identical stratified 80/20 train/test split (test set:
108 windows, 27 positive).

| Model | Accuracy | Precision | Recall | F1 |
|---|---|---|---|---|
| Baseline (majority class) | 75.0% | 0.000 | 0.000 | 0.000 |
| SVM (4 features) | 63.0% | 0.324 | 0.444 | 0.375 |
| MLP (4 features) | 54.6% | 0.225 | 0.333 | 0.269 |
| QNN (4 qubits) | 54.6% | 0.280 | 0.519 | 0.364 |
| **PWM (0th-order, full window)** | **86.1%** | **0.731** | **0.704** | **0.717** |
| MaxEnt-like (1st-order Markov/WAM) | 81.5% | 0.684 | 0.481 | 0.565 |

See `figures/final_comparison.png` for the corresponding plot and
`results/final_comparison.csv` for the raw numbers.

![Final comparison](figures/final_comparison.png)

### Qubit-count ablation (2 / 4 / 6 / 8 qubits)

The main QNN result above uses 4 qubits. To test whether more qubits (i.e. more of
the 22-nt window) improve the quantum model, four configurations were trained on
an identical, class-balanced 100-sample training subset and evaluated on the same
108-sample test set:

| Qubits | Layers | Params | Accuracy | Precision | Recall | F1 | Training time |
|---|---|---|---|---|---|---|---|
| 2 (pooled) | 1 | 4 | 61.1% | 0.333 | 0.556 | 0.417 | 5.5s |
| 4 (truncated) | 2 | 16 | 44.4% | 0.254 | 0.630 | 0.362 | 6.6s |
| 6 (truncated) | 2 | 24 | 44.4% | 0.280 | 0.778 | 0.412 | 16.4s |
| 8 (truncated) | 2 | 32 | 49.1% | 0.267 | 0.593 | 0.368 | 145.4s |

![Qubit ablation comparison](figures/qubit_ablation_comparison.png)
![Qubit ablation loss curves](figures/qubit_ablation_loss_curves.png)

F1 does not increase monotonically with qubit count — 6 qubits gives the best
F1/training-time trade-off, and 8 qubits is ~9x slower than 6 for a *lower* F1.
This is consistent with the project's central finding: more capacity does not
help without more data.

### Full classical-vs-quantum comparison at every qubit count

The same 2/4/6/8-feature budgets were also given to SVM and MLP (trained on the
same balanced 100-sample subset), so every model is compared under an identical,
matched feature count:

| Qubits | Baseline F1 | SVM F1 | MLP F1 | QNN F1 |
|---|---|---|---|---|
| 2 | 0.000 | 0.339 | 0.400  | 0.417 |
| 4 | 0.000 | 0.250 | 0.371 | 0.362 |
| 6 | 0.000 | 0.289 | 0.371 | 0.412 |
| 8 | 0.000 | 0.296 | 0.320 | 0.368 |

![Full comparison by qubit](figures/full_comparison_by_qubit.png)

 The 2-feature MLP result is degenerate (100% recall, 25% accuracy — it predicts
"positive" for every sample) and should not be read as genuine skill. Excluding
that cell, QNN is the top or joint-top F1 at every remaining qubit count on this
particular split — but see the cross-validation result below before drawing any
conclusion from that.

### Is the QNN's edge real? 5-fold cross-validation (6-qubit configuration)

Because the table above comes from a single train/test split, the 6-qubit result
was re-run under 5-fold stratified cross-validation to check whether QNN's
apparent advantage holds up:

| Model | F1 (mean ± std across 5 folds) |
|---|---|
| Baseline | 0.000 ± 0.000 |
| SVM | 0.356 ± 0.032 |
| **MLP** | **0.391 ± 0.013** |
| QNN | 0.337 ± 0.060 |

![Cross-validation boxplot](figures/cross_validation_6qubit_boxplot.png)

Under cross-validation, QNN's fold-to-fold standard deviation (0.060) is larger
than its mean gap to MLP (0.054) — so the single-split "QNN wins" result above did
**not** replicate. This is the direct evidence behind this project's claim that no
quantum advantage is supported by the data.

### Class-balancing experiments (4-qubit configuration)

Four balancing strategies were tested on the training set only (test set always
kept in its original, imbalanced form):

| Technique | SVM F1 | MLP F1 | QNN F1 |
|---|---|---|---|
| Original (imbalanced) | 0.000 | 0.059 | 0.325 |
| Random Oversampling | 0.413 | 0.395 | 0.426 |
| SMOTE | 0.388 | 0.395 | 0.341 |
| Random Undersampling | 0.409 | 0.433 | 0.418 |

![Balancing experiments comparison](figures/balancing_experiments_comparison.png)

Balancing is critical for SVM/MLP (both are near-useless without it: SVM collapses
to predicting the majority class every time) but far less important for the QNN,
whose probability-regression loss (MSE against soft labels) already behaves more
gracefully under imbalance than a hard decision-boundary classifier.

### Why did the simple PWM outperform the more complex MaxEnt-like model?

The 1st-order Markov model estimates a full 4×4 conditional transition matrix at
every window position, which is a much larger parameter space than the ~50-56
positive training examples available per site type can reliably constrain. Its
training-set F1 was 0.90-0.94 (near-perfect) but its held-out test F1 dropped to
0.565 — a clear overfitting signature. The plain PWM, with roughly an order of
magnitude fewer parameters, generalized far better on the same data. This is a
direct empirical illustration of the bias–variance trade-off: **model capacity
needs to be matched to the amount of training evidence available, not to how
sophisticated the biological signal could in principle be modeled.**

## Honest limitations

- **Sample size.** Even after combining five genes, there are only 136 real
  positive splice-site examples. This is orders of magnitude smaller than the
  datasets real genome-wide tools (e.g. SpliceAI) are trained on, and it is the
  single biggest constraint on every result in this repository.
- **No quantum advantage was found.** Under a matched, low-dimensional feature
  budget, the QNN performed comparably to (not clearly better than) classical
  SVM/MLP, and 5-fold cross-validation showed its apparent single-split edge over
  classical models did not replicate — fold-to-fold F1 variance exceeded the
  QNN–classical performance gap.
- **Feature truncation.** SVM, MLP, and the main QNN only used 4 of the 22
  available window positions, for computational tractability on quantum circuit
  simulation. The qubit-ablation experiment (2/4/6/8 qubits) explores this
  trade-off directly but does not fully resolve it.
- **ASO scoring is exploratory.** Binding-affinity and exon-inclusion scores for
  ASO candidates are derived from the (currently modest-performing) trained QNN
  and a simplified thermodynamic model — they are a proof-of-concept scoring
  pipeline, not a wet-lab-validated ranking.

## How to reproduce

```bash
pip install -r requirements.txt
```

Run scripts in `src/` in numerical order from the repository root:

```bash
python3 src/01_build_dataset.py
python3 src/02_train_qnn.py
python3 src/03_benchmark_classical_vs_quantum.py
python3 src/04_qubit_ablation.py            # or: 04_qubit_ablation.py <2|4|6|8>
python3 src/05_full_comparison_by_qubit.py
python3 src/06_cross_validation_6qubit.py   # or: 06_...py <0-4> per fold
python3 src/07_pwm_baseline.py
python3 src/08_maxent_like_baseline.py
python3 src/09_class_balancing_experiments.py  # or: 09_...py <0-3> per technique
python3 src/10_design_aso_candidates.py
```

Scripts `04`, `06`, and `09` are computationally expensive (quantum circuit
simulation) and can be run one configuration/fold/technique at a time by passing
an index argument, useful if you need to checkpoint long runs.

All random seeds are fixed (42) for reproducibility.

## Future work

- **Wet-lab validation** of the top-scoring ASO candidates against the SMN2
  ISS-N1 region — transfection into an SMA patient cell line (e.g. GM03813)
  followed by RT-PCR quantification of exon 7 inclusion would be the natural next
  step to test whether the computational ranking has any biological signal.
- Expanding ground truth beyond five genes (e.g. using GENCODE genome-wide splice
  annotations) to test whether the PWM-vs-MaxEnt-vs-QNN ranking changes with two
  or three orders of magnitude more training data.
- Re-running the qubit-ablation experiment with the full 22-position window
  (rather than a 4/6/8-feature truncation) using amplitude encoding, to test
  whether the current feature bottleneck — not model class — is really the
  limiting factor for the QNN.
- A more rigorous off-target scan (real local alignment / seed-extend against the
  full genome rather than a single gene region) before any candidate is
  considered for synthesis.

## Repository structure

```
data/       raw genomic + mRNA FASTA files (5 genes)
src/        pipeline scripts, run in numerical order
results/    JSON/CSV outputs (trained parameters, metrics, scores)
figures/    all plots referenced above
```
