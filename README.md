# Homework 03 - Data Stream Frequency Estimation

## Overview

This repository contains the implementation developed for Homework 03 of the Data Stream course.

The goal of the assignment is to approximate the frequency of elements appearing in a large data stream using a memory-efficient probabilistic algorithm. Instead of storing exact counters for every possible item, the algorithm uses multiple hash functions and a compact matrix of counters to estimate frequencies with provable error bounds.

The implemented method follows the algorithm described in the homework specification and is closely related to the Count-Min Sketch data structure.

---

## Problem Statement

Given a stream

S = {a₁, a₂, ..., aₜ}

where each element belongs to a large universe of possible values, the objective is to estimate the frequency

F[i]

of every item i while using significantly less memory than exact counting.

The algorithm guarantees that, for every item i,

F[i] ≤ F̂[i] ≤ F[i] + εt

with probability at least

1 − δ.

---

## Algorithm

The algorithm maintains:

* K independent hash functions
* A matrix H of size K × J
* Compact counters updated online as new stream elements arrive

For each incoming element:

1. Compute its hash value for each hash function.
2. Increment the corresponding counter.
3. To estimate the frequency of an element, return the minimum counter value among all hash functions.

### Parameters

* ε (epsilon): controls the additive error
* δ (delta): controls the confidence level

The homework uses:

* δ = e⁻⁵
* ε = e × 10⁻⁴

which produce:

* K = 5 hash functions
* J = ⌈e / ε⌉ counters per row

---

## Hash Functions

The following hash functions are provided by the assignment:

g₀(i) = (3i + 1561) mod 123457

g₁(i) = (17i + 277) mod 123457

g₂(i) = (38i + 394) mod 123457

g₃(i) = (61i + 13) mod 123457

g₄(i) = (78i + 246) mod 123457

The actual bucket index is computed as:

hₖ(i) = gₖ(i) mod J

---

## Repository Structure

```text
.
├── src/
│   ├── count_min_sketch.py
│   ├── hash_functions.py
│   └── main.py
│
├── data/
│   └── stream_dataset.txt
│
├── results/
│   ├── plots/
│   └── reports/
│
├── requirements.txt
├── README.md
└── LICENSE
```

---

## Installation

Clone the repository:

```bash
git clone https://github.com/<your-username>/<repository-name>.git
cd <repository-name>
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## Usage

Run the main script:

```bash
python main.py
```

The program processes the data stream, updates the sketch structure, and provides frequency estimates for queried items.

---

## Results

The implementation was tested on large-scale streams containing millions of elements.

The probabilistic approach significantly reduces memory consumption while maintaining accurate frequency estimates within the theoretical bounds defined by ε and δ.

---

## Learning Objectives

* Streaming algorithms
* Probabilistic data structures
* Hash-based frequency estimation
* Space-efficient computation
* Approximation guarantees

---

## Author

Developed as part of the Data Stream course homework assignment.
