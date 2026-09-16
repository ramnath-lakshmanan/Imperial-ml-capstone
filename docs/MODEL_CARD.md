# Model card: GP-UCB personal-best proposer



**Name:** GP-UCB + personal-best anchor  

**Type:** Sequential black-box optimiser (surrogate + acquisition)  

**Version:** 1.9 (policy used for Week 10)  

**Framework:** NumPy, scikit-learn (`GaussianProcessRegressor`, optional `SVC`, `MLPRegressor`)  

**Date:** 2026-09-16  



This card follows Mitchell et al., *Model Cards for Model Reporting*.



## 1. Overview



Each of the eight hidden functions has its own GP. The next query maximises



UCB(x) = mu(x) + kappa * sigma(x)



among candidates drawn in a Gaussian cloud around the personal-best `x`, after rejecting points closer than `min_dist` to an already evaluated row.



It is **not** a neural language model. LLM ideas (temperature, few-shot, format constraints) were only used as analogies in later discussion posts.



## 2. Intended use



**Suitable for.**  

Low-dimensional, expensive, derivative-free maximisation with a handful of evaluations (here: one shot per week).



**Avoid.**  

High-dimensional search, images, text, safety-critical control without extra validation, or treating the GP mean as the true function.



## 3. Details across ten rounds



| Weeks | What changed |

|---|---|

| 1 | GP + UCB, Latin-style candidates |

| 2 | Local cloud around first improvements |

| 3 | Soft-margin RBF SVM as high/low region filter |

| 4 | Tiny MLP (16-8) as a finite-difference hint only |

| 5–9 | Per-function kappa; **snap back** if y drops |

| 10 | Same rule: exploit F5 face and F3 new best; restore F2/F4/F6/F7/F8/F1 anchors |



Hyperparameters that actually move the portal string: `kappa`, local radius, `min_dist`. Kernel length-scales are fitted by sklearn.



## 4. Performance



Metric: **personal-best y** (higher is better). Not accuracy.



| Fn | Best y | Comment |

|---|---|---|

| 1 | 4.58e-13 | Still essentially a floor |

| 2 | 0.723 | Ridge at x1≈0.71, x2≈0.01; later weeks left it and fell |

| 3 | −0.006 | New best in Week 9 |

| 4 | 0.610 | Week 4 pocket; later local steps weaker |

| 5 | 4520 | Clear scaling on x2=x3=x4≈1 while raising x1 |

| 6 | −0.162 | Week 7 best |

| 7 | 2.329 | Week 7 best |

| 8 | 9.963 | Small gains then flat |



F5 is the success case for the policy. F2 is the warning case: leaving a known feature looks like exploration and reduces y.



## 5. Assumptions and limitations



**Assumptions.**  

- Local smoothness near the current best (Matérn-5/2).  

- One mode worth exploiting once a feature is found.  

- Course box is `[0, 1]^d` and maximisation.



**Failure modes.**  

- Thin spike far from all samples (likely F1).  

- Narrow ridge: a 0.02 move in x1 kills F2.  

- GP length-scale hitting the upper bound (over-smooth).  

- Overfitting a 16–8 net if it were used as the only surrogate.



**Compute.**  

One sklearn fit per function on a laptop CPU. No GPU.



## 6. Ethical considerations and transparency



No personal data. Risk is scientific, not social: over-claiming a global max.



Transparency is the mitigation:



- Every weekly `x` and `y` lives in `submissions/`.  

- The proposer cell is in `notebooks/`.  

- This card and the datasheet state the snap-back rule in words.  

- A second researcher can refit the GP if they have the starter `.npy` files.



Reproducibility is limited by the hidden oracle: nobody else can query the live functions. They can reproduce *this trace* and the next proposed `x` given the same history.



Adding more plots would help a hiring manager; it would not change the decision rule. The current structure is enough for assessment.
