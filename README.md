# Data Streaming & Multi-Armed Bandits

Two assignments from a university course on algorithms for massive data, solved in Python (Polars, NumPy, Matplotlib):

1. **Frequency estimation on a data stream** with a Count-Min-Sketch-style algorithm, with theoretical error guarantees.
2. **Multi-armed bandits**: ε-greedy vs UCB1 vs Thompson sampling, plus a clinical-safety variant where arms are drugs and low rewards are complications.

Notebooks and commentary are written in Italian.

## Repository structure

```text
data-streaming/
├── assignment.pdf                  # problem statement
├── count_min_sketch_coding.ipynb   # implementation, experiments, plots
└── count_min_sketch_theory.ipynb   # proofs of the error bounds
bandit/
└── multi_armed_bandit.ipynb        # strategy design, simulation, comparison
```

## 1. Frequency estimation on a stream

Given a stream `S = {a₁, …, aₜ}`, the goal is to estimate the frequency `F[i]` of every item without keeping one counter per item.

The algorithm keeps a `K × J` matrix of counters and `K` hash functions. Each incoming item increments one counter per row; the estimate is the minimum across rows. For every item:

```
F[i] ≤ F̂[i] ≤ F[i] + εt    with probability at least 1 − δ
```

With `δ = e⁻⁵` and `ε = e · 10⁻⁴` this gives `K = 5` hash functions and `J = ⌈e/ε⌉` counters per row.

- **Coding notebook**: initialization, streaming and querying phases, plots of estimated vs true frequencies, discussion of the results.
- **Theory notebook**: derivation of the bound and the probabilistic analysis.

## 2. Multi-armed bandits

`T = 10,000` pulls over 4 arms, comparing:

- **ε-greedy**, with exploration probability decaying as ~1/t
- **UCB1**, which picks the arm with the highest upper confidence bound
- **Thompson sampling**, which samples from each arm's posterior

**Clinical variant.** Each arm is a drug with unknown side effects, and a reward below 0.2 counts as a complication. The strategies are modified to minimize complications within the same budget, then compared again. This shifts the objective from pure reward maximization to safe exploration.

## How to run

```bash
pip install polars pandas numpy pyarrow matplotlib seaborn
jupyter notebook
```

> The bandit notebook imports `bandit_classes`, the `BanditGame` helper module provided in the course, which is not included here. The data-streaming notebook expects the course stream files in a local data folder.

## Topics

Streaming algorithms · probabilistic data structures · hashing · concentration bounds · reinforcement learning · exploration vs exploitation · Bayesian inference
