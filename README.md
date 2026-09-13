Imperial College / Emeritus — Professional Certificate in Machine Learning and AI.

Section 1 — Project overview

This project is a sequential maximisation challenge on eight hidden functions (2D to 8D). Each function starts with a small initial design. I may submit one new input per function per week. The formula, gradients and plots of the true surface are never shown.

The goal is not a perfect global max. It is to improve the best observed y under a tight evaluation budget and to keep a clear record of why each query was chosen. That matches real ML work: hyperparameter search, simulator tuning, lab experiments and any API that returns a score but not a derivative.

The high-level idea is model → decide → observe → update. I fit a cheap surrogate on the points I already have, pick the next x, wait for the portal y, then revise the search.

This capstone supports the career move into applied ML: limited data, no closed-form objective, and a need to justify decisions to someone who was not at the keyboard.

Section 2 — Inputs and outputs

What the hidden function receives

A vector x in the unit cube [0, 1]^d.

Function	Dimension	Starter n
1,2	           2	                10
3	           3	                15
4,5	           4	                30/20
6	           5	                20
7	           6	                30
8	           8	                40

Portal format (six decimal places, hyphen, each piece starts with 0.):

Example 3D :

0.040729-0.915411-0.504944
What it returns

One scalar y. I maximise y. Scales differ a lot (F1 near 0, F5 in the thousands, F8 near 10), so each function is modelled on its own. I never mix the eight y columns.

What my code receives / returns

Receives the growing (X, y) history. Returns one new x string per function.

Section 3 — Challenge objectives

•  Raise the best y on each of the eight oracles.

•  One query per function per week; results arrive after the portal processes the batch.

•  Structure unknown: peaks, flats, ridges, possible noise.

•  Success is a thoughtful trail (explore vs exploit, what the last y taught) as well as the number itself.

Limits: no extra evaluations, no look at the formula, coordinates must stay in (0, 1) at six decimals.

Section 4 — Technical approach (Weeks 1–3)

Week 1. Gaussian Process (Matérn-5/2, inputs standardised, y normalised) + UCB. High kappa, global candidates. Seven of eight functions beat the starter best. F1 stayed flat (y ≈ 0).

Week 2. Same GP. Lower kappa and a local cloud around last week’s point on the winners. Linear and logistic checks used only as direction hints (F2 ridge near x1 ≈ 0.7; F5 high x2–x4). Results: F2, F4, F5, F8 improved (F4 first became positive; F5 2831 → 4442). F3, F6, F7 got worse. F1 first showed a faint signal (4.6e-13 at about [0.52, 0.33]).

Week 3. GP-UCB still picks the point. A soft-margin RBF SVM labels high vs low (y ≥ median) and gives a small bonus to “high” candidates. Losers are pulled back toward their personal best; winners stay local. F5 continues toward the (x2,x3,x4) ≈ 1 corner.

Exploration vs exploitation. Week 1 explore. Weeks 2–3 exploit where y rose, keep exploring F1 until the spike is clear. Unique part of the workflow: do not delete a bad point; use it so the SVM learns the low region. One model per function; never compare raw y across functions.
