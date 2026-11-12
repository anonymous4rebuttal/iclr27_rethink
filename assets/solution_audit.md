# Auditing newly covered AIME problems

We compared problem-level solved sets at **2,048 candidates per problem** and
inspected the saved derivations for the newly covered cases below. A problem is
counted as covered when at least one candidate receives a correct final-answer
score. Each candidate has an **8,192-output-token limit**.

| Comparison | Pruned configurations examined | Newly covered problem–configuration pairs |
|:---|---:|---:|
| Qwen3.6-35B-A3B, main evaluation | 26 | 0 |
| GLM-4.7-Flash, main evaluation | 30 | 1 |
| Qwen3.5-122B-A10B, supplementary comparison | 1 | 1 |

The main evaluation includes the Expert, FFN, and Layer configurations and the
expert-selection variants used in the paper. The two cases were selected by
this solved-set comparison, before inspecting their derivations.
One additional GLM Layer configuration has 256 candidates per problem and
no covered problems; it is excluded from the matched-budget table above.

| Problem | Pruned model | Correct candidates, original | Correct candidates, pruned | Final answer | Derivation audit |
|:---|:---|---:|---:|---:|:---|
| Cell-loop partition | GLM-4.7-Flash; FFN, 10% pruning | 0/2,048 | 1/2,048 | 83 | Incorrect final aggregation |
| Cyclic congruence count | Qwen3.5-122B-A10B; Expert, 75% pruning | 0/2,048 | 1/2,048 | 393 | Incorrect case counts that cancel in the total |

Both final answers match the benchmark labels. **The saved derivations contain
errors, so final-answer coverage and verified derivation validity differ in
these two cases.** We retain the benchmark's final-answer scores and report the
derivation audit separately.

## Cell-loop partition

The task asks for the number of partitions of a 10 × 10 grid into five rectangular
cell loops. The saved response concludes with the following sum:

> 1 + 5 + 5 + 5 + 5 + 30 + 20 + 1 + 1 + 30 = 83.

The left-hand side is **103**, not 83. The response subsequently gives the
benchmark answer 83 without repairing this aggregation. This is an explicit
error in the presented derivation, independently of whether its geometric case
classification is complete.

## Cyclic congruence count

The task counts ordered tuples with entries in {1, 2, 3} satisfying two divisibility
conditions. With residues x₁, …, x₇ in {0, 1, 2}, these conditions are

S = x₁ + x₂ + x₃ + x₄ + x₅ + x₆ + x₇ ≡ 0 (mod 3),

T = x₁x₂x₄ + x₂x₃x₅ + x₃x₄x₆ + x₄x₅x₇ + x₅x₆x₁ + x₆x₇x₂ + x₇x₁x₃ ≡ 0 (mod 3).

The response replaces the seven specified cyclic products with arbitrary
three-element combinations of the nonzero positions. For the case with three
zeros, two ones, and two twos, it states:

> The non-zero terms in T correspond to triplets of these indices.
> There are (4 choose 3) = 4 such triplets.

It consequently accepts all 210 arrangements of that multiset. The tuple
**(0, 0, 0, 1, 1, 2, 2)** is a counterexample: S ≡ 0 but T ≡ 2 (mod 3).
The positions matter because only the seven specified products belong to T.

Exhaustive enumeration of all 3⁷ = 2,187 tuples gives 729 satisfying S ≡ 0;
their T residues 0, 1, and 2 occur **393, 168, and 168** times, respectively.
The following breakdown shows how the generated case errors cancel:

| Number of zero residues | Count claimed in the response | Exact valid count |
|---:|---:|---:|
| 0 | 0 | 0 |
| 1 | 140 | 84 |
| 2 | 0 | 168 |
| 3 | 210 | 42 |
| 4 | 0 | 56 |
| 5 | 42 | 42 |
| 6 | 0 | 0 |
| 7 | 1 | 1 |
| **Total** | **393** | **393** |

The exact count can be reproduced with this standard-library Python snippet:

```python
from collections import Counter
from itertools import product

by_zeros = Counter()
for x in product(range(3), repeat=7):
    s = sum(x)
    t = sum(x[i] * x[(i + 1) % 7] * x[(i + 3) % 7] for i in range(7))
    if s % 3 == 0 and t % 3 == 0:
        by_zeros[x.count(0)] += 1
print([by_zeros[n] for n in range(8)])
# [0, 84, 168, 42, 56, 42, 0, 1]
print(sum(by_zeros.values()))  # 393
```

## Solved-set overlap for the 122B comparison

| AIME problem group | Number of problems |
|:---|---:|
| Covered by both original and pruned models | 27 |
| Covered only by the original model | 1 |
| Covered only by the pruned model | 1 |
| Covered by neither model | 1 |

Both models cover 28/30 problems at this budget, with one different problem in
each solved set. The derivation checks above apply to the two identified newly
covered cases; they are separate from the aggregate coverage comparisons.
