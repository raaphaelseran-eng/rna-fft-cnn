# Methods & interpretation (public overview)

## High-level workflow

```text
RNA sequence (A / C / G / U)
          |
    sequence encoding
          |
    +-----------------------+
    | one-hot representation|
    | FFT-derived features  |
    +-----------------------+
          |
  compact CNN / MLP baseline
          |
  ViennaRNA-MFE estimate
          |
  evaluate: R² / Pearson / error
          |
  stratify by length, source and GC baseline
```

**Technology:** Python, PyTorch, NumPy, SciPy, scikit-learn, ViennaRNA; visualization and scientific comparison of architectures and preprocessing approaches.

## Why Fourier features?

The motivation in the research presentation is to supply a smaller CNN with features related to sequence composition, periodicity and relationships between nucleotide indicators, rather than relying only on a raw positional representation. Whether these features help depends on the benchmark, input normalization and architecture.

## Spectral motif interpretation

The supplied five-page technical note derives a connection between **linear auto/cross-correlation** and Fourier transforms of one-hot nucleotide indicator signals. With sufficient zero-padding (at least `2L - 1` samples), the reconstructed correlation counts the number of pairs of nucleotides at each separation, without cyclic wrap-around. There are **16 ordered nucleotide-pair channels** for A/C/G/U.

This is a mathematical motivation for using spectral representations. It does not mean that one can read the learned network's biological mechanisms directly from an averaged frequency saliency map.

## Separating a GC-only explanation

In RNA sequences, GC fraction can strongly co-vary with predicted folding energy. The study used a baseline depending only on `%GC` and examined what remained after baseline adjustment. The idea is to test whether the model is doing more than matching a gross composition statistic.

**Critical evaluation rule:** a GC baseline should be fitted on training data only and then applied to held-out data. Residual metrics and raw-energy metrics answer different questions and must be labelled separately.

## Disclosure boundary

No training pipeline, internal scripts, datasets, model weights, network parameters or private reports are included in this public overview. The methods are described conceptually to make the research understandable without exposing laboratory implementation.
