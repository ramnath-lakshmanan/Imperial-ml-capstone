# Datasheet: Imperial PCMLAI BBO capstone query log



**Dataset name:** BBO weekly query-and-response log  

**Owner:** course learner (Imperial-ml-capstone)  

**Version:** 1.0 (Weeks 1–9; Week 10 in progress)  

**Last updated:** 2026-09-16  

**Format:** per-week Markdown in `submissions/` plus arrays in `notebooks/capstone_bbo.ipynb`



This datasheet follows Gebru et al., *Datasheets for Datasets*.



## 1. Motivation



**Why this set exists.**  

The Imperial Black-Box Optimisation challenge hides eight scalar functions. Each week the portal accepts one input vector per function and returns one `y`. The log exists so every later query can use the full history, and so a reviewer can see what was tried.



**Task it supports.**  

Sequential maximisation of eight unknown functions on `[0, 1]^d` with one evaluation per function per week.



**Who created it.**  

The learner. Starter `(x, y)` files were issued by the course. Weekly `y` values come from the official oracle email.



**Funding.**  

None beyond the professional-certificate programme.



## 2. Composition



**What is in it.**



| Source | Contents |

|---|---|

| Course starter | `initial_inputs.npy`, `initial_outputs.npy` for functions 1–8 |

| Weeks 1–9 | eight `x` vectors and eight scalar `y` values per week |

| Derived | personal-best table, anchors used by the proposer |



**Dimensionality.**  

F1–F2: 2-D. F3: 3-D. F4–F5: 4-D. F6: 5-D. F7: 6-D. F8: 8-D.



**Size (after Week 9).**  

Starter design (about 10 points per function, course-issued) plus 9 weekly points → about 19 labelled pairs per function. Not a large public corpus.



**Format.**  

Inputs are real numbers in `(0, 1]`. Portal strings use six decimal places, hyphen-separated (`0.123456-0.654321`). Outputs are floats; F1 is near machine zero except one Week-2 spike; F5 is in the thousands.



**Best observed `y` after Week 9.**



| Function | Best y | Week of best |

|---|---|---|

| 1 | 4.58e-13 | 2 |

| 2 | 0.723 | 5 |

| 3 | −0.0062 | 9 |

| 4 | 0.610 | 4 |

| 5 | 4520 | 9 |

| 6 | −0.162 | 7 |

| 7 | 2.329 | 7 |

| 8 | 9.963 | 5 |



**Gaps.**  

- F1 is almost all floor values.  

- Later weeks cluster on F5’s `x2=x3=x4≈1` face and F2’s `x1≈0.71` strip.  

- Large regions of F7 and F8 are unsampled.  

- Course starter files are **not** in the public repo (binary course data).



**Sensitive data.**  

None. No people, no text, no identifiers.



**Errors / noise.**  

Oracle `y` is treated as exact. Portal format errors (wrong decimals) were rejected and corrected; those rejected strings are not part of the labelled set.



## 3. Collection process



**How queries were generated.**  

Gaussian Process (Matérn-5/2) + UCB, with a local cloud around the current personal best. Week 3 added a soft-margin SVM high/low filter. Week 4 added a small MLP only as a gradient hint. From Week 5 the policy was: if `y` falls, snap the next centre back to the stored best.



**Time frame.**  

One official round per week across the capstone calendar (Weeks 1–9 collected; Week 10 next).



**Who was involved.**  

The learner chose `x`. The course oracle returned `y`. No crowd workers.



**Sampling strategy.**  

Not i.i.d. Adaptive, exploit-heavy after a feature is found. That is a **collection bias**, not a bug.



## 4. Preprocessing and uses



**Transforms.**  

`StandardScaler` on `x` before the GP. `normalize_y=True` in sklearn. Inputs clipped to `[1e-6, 1-1e-6]` and printed to six decimals. No other cleaning.



**Intended uses.**  

- Reproducing this capstone trace.  

- Teaching sequential design / Bayesian optimisation.  

- Comparing a new acquisition rule on the *same* history (offline).



**Inappropriate uses.**  

- Training a general LLM or image model.  

- Claiming a global optimum for any function.  

- Publishing the course starter `.npy` if the programme forbids redistribution.  

- Mixing F5’s scale with F1’s zeros in one un-normalised model.



## 5. Distribution and maintenance



**Where.**  

Public GitHub repo `Imperial-ml-capstone`, folders `submissions/` and `notebooks/`. Starter `.npy` stay on the local machine (`Downloads\Initial_data_points_starter (2)\initial_data`).



**Terms.**  

Weekly `(x, y)` logged by the learner may be shown for assessment. Course starter files remain course property.



**Maintainer.**  

The learner. Each new portal email appends one row per function to `submissions/weekNN.md` and to the notebook dictionaries.



**Retention.**  

Keep until the programme ends, then as a portfolio artefact.



**Citation.**  

This repository plus Gebru et al. (2021), Datasheets for Datasets.
