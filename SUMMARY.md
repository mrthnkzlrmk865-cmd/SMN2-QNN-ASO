# Project Summary

**Quantum Neural Networks for SMN2 Splice-Site Prediction and ASO Candidate Design**

## Who I am

I'm a high school student in Turkey pursuing independent research at the
intersection of computational biology and quantum machine learning, motivated by
an interest in Spinal Muscular Atrophy (SMA) and RNA-splicing-targeted therapy.

## What I built

A from-scratch pipeline that (1) extracts real, alignment-verified splice-site
ground truth from five human genes (SMN2, BRCA1, TP53, CFTR, HBB) rather than
relying on simplified or synthetic labels, (2) trains a quantum neural network
(parametric circuit, simulated IBM noise) to classify these sites, (3) benchmarks
it against classical ML (SVM, MLP) and two from-scratch classical bioinformatics
baselines (a PWM and a 1st-order Markov/WAM model) under identical train/test
conditions, and (4) uses the trained model's output to dynamically score ASO
candidates targeting SMN2's ISS-N1 splicing-silencer region.

## Key result

Across every model, a simple domain-specific PWM using the full sequence window
outperformed both the quantum model and classical general-purpose ML (F1 = 0.72 vs.
0.36–0.38), while a more complex Markov model that captures neighboring-position
dependencies (the methodological precursor to MaxEntScan) performed *worse* than
the simple PWM (F1 = 0.57) — a clear overfitting signature given only ~50-56
positive training examples per site type. 5-fold cross-validation further showed
that any apparent quantum advantage over classical models on a single train/test
split did not replicate. The project's central, data-driven finding is that
**model complexity needs to be matched to the amount of real biological ground
truth available** — a more defensible and generalizable conclusion than either a
naive "quantum wins" or "quantum loses" claim.

## What I am looking for

I would value feedback on the experimental design (particularly the qubit-count
ablation and cross-validation setup), guidance on how to responsibly scale the
ground-truth dataset beyond five genes, and — if there is interest — the
possibility of a lightweight collaboration or mentorship around wet-lab validation
of the top-ranked ASO candidates. All code, data, and results are openly available
and fully reproducible with fixed random seeds.

**Repository:** available on request / linked from GitHub profile.
