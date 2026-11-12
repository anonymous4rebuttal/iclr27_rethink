# Figure 3 · Output length, quality, and truncation

[Back to figures](README.md) · [Figure 1](FIGURE1.md)

## Paper figure

**Output length, accuracy and coverage across pruning ratios.** The paper figure uses left axes for mean output tokens (thousands) and right axes for pass@1/pass@512. Grey rings mark configurations with at least 20% truncation. This is the figure from the paper, reproduced at high resolution.

![Paper Figure 3](assets/figure3_paper.png)

[Full-resolution PNG](assets/figure3_paper.png) · [Vector PDF](assets/figure3_paper.pdf)

## Enlarged panels

Each enlarged view uses separate rows for **mean output length**, **pass@1 and pass@512**, and **truncation rate**. The columns compare Expert, FFN, and Layer pruning; the horizontal axis is the pruning ratio. Length is shown directly in tokens. Untruncated responses include both correct and incorrect answers. The revised views use the verified [length data](assets/length_diagnostics.csv).

| Task | Qwen3.6-35B-A3B | GLM-4.7-Flash |
| --- | --- | --- |
| AIME 2026 | [PNG](assets/length_qwen36_aime2026.png) · [PDF](assets/length_qwen36_aime2026.pdf) · [SVG](assets/length_qwen36_aime2026.svg) | [PNG](assets/length_glm47_aime2026.png) · [PDF](assets/length_glm47_aime2026.pdf) · [SVG](assets/length_glm47_aime2026.svg) |
| Minerva Math | [PNG](assets/length_qwen36_minerva.png) · [PDF](assets/length_qwen36_minerva.pdf) · [SVG](assets/length_qwen36_minerva.svg) | [PNG](assets/length_glm47_minerva.png) · [PDF](assets/length_glm47_minerva.pdf) · [SVG](assets/length_glm47_minerva.svg) |
| HumanEval+ | [PNG](assets/length_qwen36_humanevalplus.png) · [PDF](assets/length_qwen36_humanevalplus.pdf) · [SVG](assets/length_qwen36_humanevalplus.svg) | [PNG](assets/length_glm47_humanevalplus.png) · [PDF](assets/length_glm47_humanevalplus.pdf) · [SVG](assets/length_glm47_humanevalplus.svg) |
| LiveCodeBench-v6 | [PNG](assets/length_qwen36_lcb.png) · [PDF](assets/length_qwen36_lcb.pdf) · [SVG](assets/length_qwen36_lcb.svg) | [PNG](assets/length_glm47_lcb.png) · [PDF](assets/length_glm47_lcb.pdf) · [SVG](assets/length_glm47_lcb.svg) |

### Enlarged example: Qwen on AIME

![Figure 3 enlarged Qwen AIME view](assets/length_qwen36_aime2026.png)

## Resource comparisons

These views retain Figure 3's length, pass@1/pass@512, and truncation measurements while replacing the pruning-ratio axis with resource measures. They cover the original model and available 10%, 30%, and 50% configurations. Every task uses **pass@512** for coverage. [Joined figure data](assets/figure3_resource_data.csv).

| Task | Qwen3.6-35B-A3B | GLM-4.7-Flash |
| --- | --- | --- |
| AIME 2026 | [PNG](assets/figure3_resources_qwen36_aime2026.png) · [PDF](assets/figure3_resources_qwen36_aime2026.pdf) · [SVG](assets/figure3_resources_qwen36_aime2026.svg) | [PNG](assets/figure3_resources_glm47_aime2026.png) · [PDF](assets/figure3_resources_glm47_aime2026.pdf) · [SVG](assets/figure3_resources_glm47_aime2026.svg) |
| Minerva Math | [PNG](assets/figure3_resources_qwen36_minerva.png) · [PDF](assets/figure3_resources_qwen36_minerva.pdf) · [SVG](assets/figure3_resources_qwen36_minerva.svg) | [PNG](assets/figure3_resources_glm47_minerva.png) · [PDF](assets/figure3_resources_glm47_minerva.pdf) · [SVG](assets/figure3_resources_glm47_minerva.svg) |
| HumanEval+ | [PNG](assets/figure3_resources_qwen36_humanevalplus.png) · [PDF](assets/figure3_resources_qwen36_humanevalplus.pdf) · [SVG](assets/figure3_resources_qwen36_humanevalplus.svg) | [PNG](assets/figure3_resources_glm47_humanevalplus.png) · [PDF](assets/figure3_resources_glm47_humanevalplus.pdf) · [SVG](assets/figure3_resources_glm47_humanevalplus.svg) |
| LiveCodeBench-v6 | [PNG](assets/figure3_resources_qwen36_lcb.png) · [PDF](assets/figure3_resources_qwen36_lcb.pdf) · [SVG](assets/figure3_resources_qwen36_lcb.svg) | [PNG](assets/figure3_resources_glm47_lcb.png) · [PDF](assets/figure3_resources_glm47_lcb.pdf) · [SVG](assets/figure3_resources_glm47_lcb.svg) |

### Resource example: Qwen on AIME

![Figure 3 Qwen AIME resource view](assets/figure3_resources_qwen36_aime2026.png)

## Resource measurement definitions

The resource comparisons use **total parameters**, **analytically estimated FLOPs per request**, and **measured time per output token (TPOT)**. Resource measurements use one H20 GPU, BF16, 512 input tokens, 128 output tokens, and one request at a time. TPOT is the median across five timed batches after two warm-up batches. These fixed-length measurements describe execution cost; the reasoning evaluations separately use an 8,192-token response limit.

Colors identify pruning structures; marker shapes identify 10%, 30%, and 50% pruning. Diamonds mark the original model. Lines connect available configurations in pruning-ratio order. **GLM Layer 30% has resource measurements but no corresponding four-task quality result**, so it has no quality point. GLM Layer 10% removes 5/47 layers, displayed as 11% in the paper's length plot. See the [resource data](assets/resource_quality.csv), [all runtime measurements](assets/runtime_sweep.csv), and [accounting details](assets/CAPTIONS.md).
