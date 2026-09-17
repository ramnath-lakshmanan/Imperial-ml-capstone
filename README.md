# Imperial PCMLAI — Black-Box Optimisation Capstone

Public repo for the 13-week BBO challenge: one query per week on eight hidden functions.

**Repository:** https://github.com/ramnath-lakshmanan/Imperial-ml-capstone

## For a general reader (about 100 words)

This project is a 13-week search for good settings on eight hidden tests. Each week the course only returns a score. There is no formula. I fitted a simple statistical map of the scores so far, then picked the next setting near the best point I already had. If a new score was worse, I went back to the old best instead of chasing a bad week. The clear gain was Function 5, which rose from about 4,440 to 5,798 by keeping three inputs at 1 and only moving the first input. Some other tests barely moved. The lesson is: when tests are expensive, write down what worked and do not throw it away after one poor result.

## What is in this repo

| Path | What it is |
| --- | --- |
| `docs/DATASHEET.md` | Dataset context (Gebru-style) |
| `docs/MODEL_CARD.md` | Optimiser card (Mitchell-style) |
| `docs/BBO_Capstone_Presentation.pdf` | Short presentation of the method |
| `notebooks/bbo_proposer.ipynb` | Weekly GP-UCB proposer cell |
| `submissions/` | Week-by-week portal strings and oracle y |
| `LICENSE` | Licence for the code in this repo |

Starter `.npy` files from the course are **not** uploaded (course data). Describe them in the datasheet and keep them local.

## Method in one paragraph

Each function has its own Gaussian Process (Matérn-5/2 + white noise). Candidates are scored with UCB = mean + kappa * std around a personal-best anchor. Portal format: six decimals, values in (0, 1), hyphen-separated.

## Best observed y after Week 13

| Function | Best y | Week | Note |
| --- | --- | --- | --- |
| 1 | 4.58e-13 | 2 | Floor |
| 2 | 0.723 | 5 | Ridge; later weeks off-ridge were worse |
| 3 | -0.0057 | 11 | Week 13 dropped |
| 4 | 0.612 | 11 | Week 13 dropped |
| 5 | **5798** | 13 | High face; first input walked upward |
| 6 | -0.162 | 7 | Later weeks worse |
| 7 | **2.686** | 13 | Late local gain |
| 8 | **9.997** | 13 | Almost flat near 9.99 |

## How to run

1. Unzip the course starter data locally.
2. Open `notebooks/bbo_proposer.ipynb`.
3. Set `base` to your local `initial_data` folder.
4. Run the cell. Paste the eight printed strings into the portal.

## Licence

See `LICENSE`. Course starter data remains the programme’s material.
