# Model card — GP-UCB proposer (Weeks 1–13)

Framework: Mitchell et al., *Model Cards for Model Reporting*.

## Model details

- **Name:** Weekly GP-UCB proposer for eight hidden functions.
- **Type:** Gaussian Process regression + Upper Confidence Bound, one model per function.
- **Kernel:** ConstantKernel × Matérn-5/2 + WhiteKernel.
- **Inputs:** Scaled with `StandardScaler`. `normalize_y=True`.
- **Acquisition:** UCB = mu + kappa * std. Kappa, local radius and min-distance are set per function.
- **Policy:** Personal-best anchor. If last `y` is worse than the stored best, the next cloud is centred on the stored best (snap-back).
- **Framework:** scikit-learn, NumPy, Jupyter.
- **Licence:** See repo `LICENSE`. Course data is not included.

## Intended use

- **Primary:** Propose one in-box point per function per week in the Imperial BBO portal.
- **Out of scope:** Production hyperparameter search without a new study; claiming a certified global max.

## Factors and limitations

- Sample size is small (starter points + 13 queries). Length-scale fits can warn; those warnings were treated as harmless.
- Adaptive sampling biases the GP toward visited clusters.
- Function 1 never left a numerical floor. Function 8 is almost flat near 9.99.
- Portal format (six decimals, open unit interval) constrains the output of `fmt()`.

## Metrics / performance

Best observed oracle `y` after Week 13:

| Fn | Best y | Week | Week 13 y |
| --- | --- | --- | --- |
| 1 | 4.58e-13 | 2 | ~0 |
| 2 | 0.723 | 5 | 0.490 |
| 3 | -0.0057 | 11 | -0.044 |
| 4 | 0.612 | 11 | 0.493 |
| 5 | **5798** | 13 | 5798 |
| 6 | -0.162 | 7 | -0.341 |
| 7 | **2.686** | 13 | 2.686 |
| 8 | **9.997** | 13 | 9.997 |

What worked: locking Function 5 at `x2=x3=x4 ≈ 1` and raising `x1` (0.12 to 0.77).  
What failed: leaving Function 2’s ridge (`x2 ≈ 0.01`).

## Evaluation data

The evaluation *is* the oracle. There is no held-out test set. Each week is one expensive evaluation.

## Ethical considerations

No personal data. No deployment risk beyond a course leaderboard. Transparency artefacts are this card, the datasheet, and the public notebook.

## Assumptions

- The hidden maps are stable across weeks.
- A ridge or face that raised `y` is worth pinning.
- One query per week is too few for a high-kappa search after a feature is known.

## Caveats

Week 13 Function 5 at 5798 is a personal best, not a proven global maximum. A peer reported 7044 on the same function. Function 2’s Week-13 return to the ridge recovered only to 0.490, not 0.723.
