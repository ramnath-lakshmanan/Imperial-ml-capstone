# Datasheet — BBO query log (Weeks 1–13)

Framework: Gebru et al., *Datasheets for Datasets*.

## Motivation

- **Why created:** To record every portal query and oracle score for eight hidden functions in the Imperial PCMLAI BBO capstone.
- **Who created it:** The learner, from course starter `.npy` files plus weekly portal emails.
- **Funding:** Coursework only.

## Composition

- **Instances:** Starter points from the course (count varies by function) plus 13 weekly queries per function (104 weekly `(x, y)` pairs).
- **What each instance is:** A point `x` in the unit box `[0, 1]^d` and a scalar `y` from the hidden oracle.
- **Dimensions:** F1–F2: 2; F3: 3; F4–F5: 4; F6: 5; F7: 6; F8: 8.
- **Sensitive attributes:** None.
- **Missing values:** None in the weekly log. Some `y` values are numerically a floor (Function 1).
- **Recommended split:** There is no train/test split. The log is the entire optimisation history.

### Best y after Week 13

| Fn | Best y | Week of best | Week 13 y |
| --- | --- | --- | --- |
| 1 | 4.58e-13 | 2 | -8.26e-103 |
| 2 | 0.723 | 5 | 0.490 |
| 3 | -0.0057 | 11 | -0.044 |
| 4 | 0.612 | 11 | 0.493 |
| 5 | 5798 | 13 | 5798 |
| 6 | -0.162 | 7 | -0.341 |
| 7 | 2.686 | 13 | 2.686 |
| 8 | 9.997 | 13 | 9.997 |

### Week 13 query (portal email)

| Fn | x |
| --- | --- |
| 1 | [0.063374, 0.552641] |
| 2 | [0.704815, 0.000001] |
| 3 | [0.987911, 0.199775, 0.408049] |
| 4 | [0.405339, 0.384696, 0.406659, 0.417614] |
| 5 | [0.770715, 0.999999, 0.999999, 0.999999] |
| 6 | [0.518112, 0.444066, 0.631339, 0.755503, 0.127365] |
| 7 | [0.090784, 0.000001, 0.340448, 0.237765, 0.349169, 0.647671] |
| 8 | [0.120179, 0.164217, 0.152878, 0.157702, 0.799478, 0.509155, 0.202802, 0.621526] |

Earlier weekly `x` and `y` are stored in `submissions/` and in the proposer notebook.

## Collection

- **How:** Course portal. One vector per function per week. `y` returned by email.
- **Timeframe:** 13 weekly rounds.
- **Who collected it:** The learner.
- **Collection bias:** Queries were **adaptive**. Later points cluster near personal bests (Function 5 on the high face; Function 2 on a ridge). This is not a uniform sample of the cube.
- **Ethical review:** Not required. No people in the data.

## Preprocessing

- Inputs clipped to `(1e-6, 1-1e-6)` so the portal would accept the string.
- No imputation. No label transformation except GP `normalize_y=True` at fit time.
- Starter `.npy` files stay on the local machine (course data, not republished here).

## Uses

- **Intended:** Reproduce the 13-week search and the GP-UCB policy.
- **Not intended:** Claiming a known global maximum. Function 5 was still rising at 5798; other learners report higher values on the same box.
- **Impact if misused:** Low. Synthetic course oracles only.

## Distribution

- Query log and documentation: this public repo.
- Starter `.npy`: course materials only. Not hosted on GitHub.

## Maintenance

- Owner: repo author.
- No planned updates after Week 13 unless the programme asks for a correction.
- Errata: discussion-board strings sometimes used a planned fallback sample; the emails above are the source of truth for `y`.
