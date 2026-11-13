# Figure 1 · Coverage versus resources

[Back to figures](README.md) · [Figure 3](FIGURE3.md)

## Resource comparisons

**Revision: compare reasoning coverage on resource-based horizontal axes.** The panels place the fixed-budget endpoints of Figure 1 against **total parameters, active parameters, estimated FLOPs, and measured decoding time**. Each row is a task; each column is a resource measure. Mathematics uses **pass@2,048**, and code uses **pass@512**, with the same candidate budget across configurations within each task.

Both backbones include the original model and available **10%, 30%, and 50% Expert, FFN, and Layer** pruning configurations. These panels make the relation between retained coverage, model size, active computation, and measured speed directly visible. We will add these resource comparisons to the revised manuscript.

### Qwen3.6-35B-A3B

![Revised Figure 1 for Qwen3.6-35B-A3B](assets/figure1_resources_qwen36.png)

[PNG](assets/figure1_resources_qwen36.png) · [PDF](assets/figure1_resources_qwen36.pdf) · [Editable SVG](assets/figure1_resources_qwen36.svg)

### GLM-4.7-Flash

![Revised Figure 1 for GLM-4.7-Flash](assets/figure1_resources_glm47.png)

[PNG](assets/figure1_resources_glm47.png) · [PDF](assets/figure1_resources_glm47.pdf) · [Editable SVG](assets/figure1_resources_glm47.svg)

## Resource definitions

The four horizontal axes show **total parameters**, **active parameters per token**, **analytically estimated FLOPs per request**, and **measured decoding time per output token**. B denotes a billion parameters; a TFLOP is 10¹² floating-point operations, a count rather than a rate. Active parameters count the weights used for each token, including attention, routing, always-active modules, selected experts, and the output head.

FLOPs and timing use a separate fixed-length workload: **one H20 GPU, BF16, vLLM 0.25.1, 512 input tokens, 128 output tokens, and one request at a time**. The prompt is processed once, followed by 127 further generation steps; one multiply–accumulate counts as two FLOPs. Time per output token is the elapsed time between the first and last output tokens divided by 127. The plotted value is the median of five timed batches after two warm-up batches. The reasoning evaluations use an **8,192-token response limit**.

Colors identify **Expert, FFN, and Layer** pruning; circles, squares, and triangles identify **10%, 30%, and 50%** pruning. Diamonds identify the original model. Lines connect available configurations in pruning-ratio order. GLM Layer 10% removes 5/47 layers, displayed as 11% in the paper. GLM Layer 30% has resource measurements but no corresponding four-task quality result, so its quality point is omitted.

[Resource and coverage data](assets/resource_quality.csv) · [All runtime measurements](assets/runtime_sweep.csv) · [Additional accounting details](assets/CAPTIONS.md)

## Original sampling curves

The original Figure 1 shows how pass@k accumulates with the number of sampled attempts. It is retained here alongside the new fixed-budget resource comparisons.

<details>
<summary>View original Figure 1</summary>

![Original Figure 1](assets/figure1_paper.png)

[PNG](assets/figure1_paper.png) · [PDF](assets/figure1_paper.pdf)

</details>
