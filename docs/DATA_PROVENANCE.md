# Data provenance & target definition

This note distinguishes **where a sequence came from** from **where its target energy came from**. It was reconstructed from the supplied private experiment files and report figures, not from a fresh run.

## Dataset streams in the archive

1. **Synthetic sequences:** `generate_viennarna_dataset` builds random A/C/G/U strings with configurable size and length, then calls ViennaRNA folding routines to obtain minimum-free-energy targets. This stream is fully synthetic at the sequence level **and** computational at the label level.
2. **RNAcentral sequence stream:** a `RNAcentral` helper in the internship deliverable requests RNA sequences via the public API, filters ambiguous nucleotides, normalizes T to U, and computes target MFE using `RNA.fold`. In an older/parallel script the helper falls back to randomly generated sequences if the API returns no data. That fallback means **a saved plot alone does not prove every input was successfully downloaded**. The small-sample RNAcentral-source result is consequently reported with that caveat.
3. **Cleaned benchmark CSV experiments:** separate exploratory notebooks reference `benchmark_cleaned_all_length.csv`, then calculate ViennaRNA MFE targets. The supplied materials do not document that file's original curator, accession list, licensing or relationship to RNAcentral; it should **not** automatically be described as a verified RNAcentral export.

## What “real data” means here

“Real RNA” refers to **biologically sourced nucleotide sequences**. It does **not** mean that the folding-energy label was measured experimentally. For these experiments the scalar target is **ViennaRNA-predicted minimum free energy**, reported in kcal/mol. The model is therefore learning to approximate a **computational folding predictor**, not validating against physical free-energy measurements.

## Why the distinction matters

- The source distribution differs substantially between random strings and sequences found in biological databases.
- A performance difference between streams may reflect sequence distribution, sample size and setup; it is not automatically evidence of biological superiority.
- Sample-level provenance, exact preprocessing and duplicate/family overlap cannot be checked from figures alone.
- A separate held-out biological dataset with measured outcomes would be necessary before claiming biological or experimental predictive validity.

## Public references

- [RNAcentral](https://rnacentral.org/) — public collection of non-coding RNA sequences.
- [ViennaRNA Package](https://github.com/ViennaRNA/ViennaRNA) — algorithms for computational RNA secondary-structure/MFE prediction.

No input sequences, accession-level data, generated targets or derived training arrays are republished in this showcase.
