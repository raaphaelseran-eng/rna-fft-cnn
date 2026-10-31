# Experimental interpretation & limitations

These notes accompany the original figure exports contained in `figures/`. They do not document a new execution of the experiments.

## Small natural-source-sequence example

The results document describes a **80-sequence train / 20-sequence test** FFT/CNN experiment using RNAcentral sequences, and the corresponding displayed plot reports:

| Metric | Reported value |
| --- | ---: |
| R² | 0.761 |
| Pearson r | 0.915 |
| RMSE | 5.008 kcal/mol |
| Test size | 20 |

A separate **synthetic-sequence** example in the same report shows R² = 0.460, Pearson r = 0.725 and RMSE = 3.313 kcal/mol. These are **individual result snapshots**; different distributions and potentially distinct setups mean the two rows should not be interpreted as a controlled generalization comparison. The training inputs for the RNAcentral-source plot cannot be independently checked without the archived run/dataset.

## Controlled data-size comparisons

The supplied results document contrasts CNN and MLP architectures using FFT-based or one-hot inputs. It reports three regression metrics — MSE, Pearson r and R² — in figures titled **“Résidus”**, i.e. evaluation after a GC-content baseline adjustment. Under FFT features, CNN generally outperforms the MLP in this figure; the one-hot comparison is more mixed, particularly at lower dataset sizes. One should not infer universal architectural superiority from a single sweep.

The synthetic comparison ranges shown in the plots span approximately **5,000–25,000** training examples. We report the plotted trends, not a claim of cross-validated performance or identical compute budgets.

## Generalization outside the training sequence length

The paired figures train with synthetic sequences of length **L = 60** and evaluate from **L = 20 to 160**. These show stronger Pearson correlation for FFT over one-hot at several nearby lengths, alongside **very negative residual R²** far from the training length for **both** representations. Squared error rises substantially for longer sequences. This should be read as **sensitivity to distribution shift**, not evidence that FFT enables unrestricted extrapolation.

### Why R² can be very negative

When predictions fail badly relative to variation in the residual target, R² can fall far below zero. Pearson correlation alone can remain positive even when predictions are badly calibrated. Displaying R², correlation **and** MSE together avoids hiding that discrepancy.

## GC-content adjustment / avoiding a composition shortcut

The research analyzes residual target energy after accounting for an easily available confounder: sequence GC fraction. The later evaluation implementation describes fitting a linear baseline on training data and calculating residual metrics after removing its prediction from both model predictions and labels. This is an evaluation technique, **not an assurance that the model avoids all compositional biases**.

The original archive contains exploratory scripts and variants with changing evaluation interfaces; not all figures can be matched to a specific saved checkpoint and data split. Reported raw-target R² and GC-residual R² should be treated as **different quantities**.

## Saliency

Averaged spectral saliency plots indicate a dominant low-frequency region. The source report also notes that other mid/high frequencies may be used but averaging can drive their saliency toward zero. These maps generate hypotheses about frequency dependence and long-range interactions; without controlled perturbations, they do **not** identify a unique causal RNA motif or physical mechanism.

## What was not validated here

- Reproduction from saved weights and exact data splits.
- Independently measured physical RNA folding energies.
- A robust multi-seed confidence interval for the small RNAcentral-source experiment.
- Family-disjoint or organism-disjoint sequence generalization.
- Fully audited ownership and rights to distribute the plots.

These limits are intentionally explicit to make the public showcase scientifically credible.
