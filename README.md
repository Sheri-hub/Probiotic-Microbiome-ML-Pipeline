# Probiotic Microbiome → Poultry Performance: ML Pipeline (Synthetic Data Demo)

A small, self-initiated exploratory project applying machine learning to the
kind of problem found at the intersection of probiotic genomics, poultry gut
microbiome data, and performance prediction — built to deepen my own
understanding of this space, inspired by published multi-strain probiotic
and poultry-performance research.

## What this is

This notebook builds and validates a complete ML pipeline on a **synthetic
(simulated) dataset with a known, deliberately built-in ground truth**. This
is a deliberate design choice, not a shortcut: using synthetic data with a
known answer makes it possible to verify that every stage of the pipeline
(filtering, normalization, CLR transformation, model training, feature
importance) is implemented *correctly*, before trusting it on real
biological data where there's no answer key to check against.

## Pipeline stages

1. **Synthetic data generation** — simulating realistic microbiome count
   data (variable sequencing depth, sparsity, compositional structure)
   across 60 birds and 20 bacterial taxa, with 3 taxa deliberately built as
   true drivers of feed conversion ratio (FCR) and weight gain, and the
   remaining 17 as pure noise.
2. **Preprocessing** — filtering rare/low-abundance taxa, converting to
   relative abundance, and applying a Centered Log-Ratio (CLR)
   transformation to address the compositional nature of microbiome data.
3. **Modeling** — a Random Forest Regressor predicting FCR and weight gain
   from CLR-transformed microbiome features, chosen over Gradient Boosting
   for its greater robustness to overfitting on small sample sizes.
4. **Validation** — 5-fold cross-validation, appropriate given the small
   (60-bird) sample size typical of real animal feeding trials.
5. **Interpretability** — Random Forest feature importance, used to check
   whether the model correctly recovers the taxa deliberately built as true
   drivers.

## Honest framing of results

Cross-validated R² values are modest (roughly 0.08–0.15) by design — the
synthetic data includes realistic irreducible biological noise, so a very
high R² would actually indicate an unrealistically clean simulation rather
than a better pipeline. The more meaningful validation is that feature
importance correctly ranks the built-in true drivers above the noise taxa
for the FCR target, with a partial (2 of 3) recovery for weight gain —
reflecting the genuine statistical difficulty of detecting weaker effects
in small samples.

## Why synthetic data first

Real microbiome studies typically don't come as ready-to-use abundance
tables — they start as raw sequencing reads requiring quality control,
ASV/OTU inference, and taxonomic classification (e.g., via DADA2 or
QIIME2) before any ML step is even possible. This project focuses on
validating the downstream ML methodology first; extending the pipeline to
real public data and to raw-read processing are natural next steps, noted
in the final section of the notebook.

## Tech stack

Python, numpy, pandas, scikit-learn.

## Status

Exploratory / learning project — not a peer-reviewed or production
pipeline. Built to demonstrate applied understanding of probiotic genomics
and ML methodology.
