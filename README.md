# Code for "Near-Optimal Reinforcement Learning with Multi-Step Transition Lookahead"

Numerical experiments on the wind-farm storage-control benchmark (Section 6 and Appendix "Experiments").

## Contents

| File | Role |
|---|---|
| `planning_fixed_2.ipynb` | Main notebook: RPTAS planner, BOLA / BOLA-Bayes / MPC baselines, protocol checks (I1–I4), Figure 1. |
| `BOLA.ipynb` | Original benchmark notebook (data loading, discretisation, value iteration, BOLA). Loaded programmatically by the main notebook; does not need to be run. |
| `Wind.csv`, `Price.csv` | Benchmark data: wind commitment/realisation and electricity prices (5-minute resolution, 2020). |

## Requirements

Python 3.9+, `numpy`, `pandas`, `matplotlib`.

## Running

Place the four files in the same folder and run `planning_fixed_2.ipynb` top to bottom from that folder (the notebook reads `BOLA.ipynb` and the CSVs by relative path).

The sweep cell covers $\ell \in \{0,1,2,3\}$, forecast noise $\sigma \in \{0, 5, \dots, 100\}\%$, dictionary size $N = 8$, and 10 common-random-number seeds for the four controllers. It writes:

- `etape9_results.csv` — mean and std of the realised cumulative cost per (controller, $\ell$, $\sigma$), with the no-prediction baseline;
- `fig_cost_reduction.pdf` — Figure 1 of the paper.

The following cell prints the four protocol checks described in the appendix (I1: $\ell=0$ equalities and flatness in $\sigma$; I2: closed-loop $\geq$ open-loop; I3: adaptivity gap monotone in $\sigma$; I4: high-noise floor).

## Notes

- Evaluation window: 3,000 steps starting at index 1952, as in the original benchmark. The exogenous kernel is estimated on a disjoint training slice starting 1,000 steps after the evaluation window ends.
- All controllers share the same forecast-noise realisation per seed (standard normals drawn once per date, rescaled by $\sigma$).
- The final cell of the sweep compares our re-implementation of BOLA with the original implementation at $\sigma = 0$ and the corrected horizon index $k = \ell + 1$.
