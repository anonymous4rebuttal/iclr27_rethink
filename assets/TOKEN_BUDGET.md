**Cumulative generation-token budgets on AIME 2026.** Oracle coverage (top) and majority-vote accuracy (bottom) versus the output-token budget per question, for the original model and Expert, FFN, and Layer pruning at 10%, 30%, and 50%. Curves replay saved generations using the exact recorded completion-token cost of each 32-candidate batch. In each random batch order, only fully completed batches within the budget contribute answers; the remaining budget is charged to a simulated unfinished batch, from which no answer is used. Lines average 1,000 orderings; shading shows their central 95% range. These ranges describe ordering variability within the saved generations. GLM Layer results use the available 50% configuration.

**Expert pruning retains its advantage under a common cumulative generation-token budget and majority voting.** We extend the analysis from a per-candidate length limit to a shared output-token budget for each question. We replay saved AIME generations, charging the exact recorded cost of every completed batch, including incorrect and truncated outputs. At 1,048,576 output tokens per question, 50% Expert pruning on Qwen achieves **91.78% oracle coverage and 84.85% majority-vote accuracy**, compared with 7.97% / 3.74% for FFN pruning and 0.77% / 0.00% for Layer pruning. GLM shows the same separation: **77.09% / 51.48%** for 50% Expert pruning, versus 0.00% / 0.00% for FFN and 0.20% / 0.05% for Layer. The full curves cover 10%, 30%, and 50% pruning wherever complete generation records are available. We will add these token-budget curves and the majority-vote evaluation to the revised paper.

**AIME 2026 at 1,048,576 output tokens per question.** All accuracy values are percentages averaged over 1,000 batch orderings. Qwen: Qwen3.6-35B-A3B. GLM: GLM-4.7-Flash. “—” denotes an unavailable configuration.

| Configuration | Qwen coverage | Qwen majority vote | GLM coverage | GLM majority vote |
|---|---:|---:|---:|---:|
| Original | 91.40 | 87.15 | 86.64 | 46.11 |
| Expert, 10% pruning | 91.58 | 87.91 | 83.76 | 44.56 |
| FFN, 10% pruning | 89.59 | 80.91 | 75.38 | 36.53 |
| Layer, 10% pruning | 86.37 | 68.66 | — | — |
| Expert, 30% pruning | 92.57 | 85.69 | 82.32 | 44.66 |
| FFN, 30% pruning | 67.83 | 54.06 | 22.44 | 11.54 |
| Layer, 30% pruning | 3.95 | 3.30 | — | — |
| Expert, 50% pruning | 91.78 | 84.85 | 77.09 | 51.48 |
| FFN, 50% pruning | 7.97 | 3.74 | 0.00 | 0.00 |
| Layer, 50% pruning | 0.77 | 0.00 | 0.20 | 0.05 |

*Evaluation details.* Each question receives its own equal output-token budget; input tokens are excluded and budgets are not transferred across questions. Each saved candidate was generated with an 8,192-token limit. Because token usage was logged per batch of 32 candidates, replay stops at the first batch that would exceed the budget, without skipping ahead. No answer from that batch is admitted; its charged partial cost is simulated. Majority voting selects the most frequent saved extracted answer, breaks ties uniformly, and requires no correctness labels for selection. Empty extracted answers cast no vote but still incur generation cost. Oracle and voting outcomes use the same frozen answer-correctness labels. We additionally checked numerical answer normalization; Expert remains ahead of FFN and Layer at every evaluated common ratio and budget under both voting definitions.
