# SAM Reproducibility Study

> **Course:** Data Science
> **Team:** Rajshree Rai, Srishti Karanth  
> **Paper:** [Segment Anything (Kirillov et al., 2023)](https://arxiv.org/abs/2304.02643)  

## Overview

This study reproduces and evaluates key claims from the Segment Anything Model (SAM) paper, focusing on zero-shot segmentation performance, cross-domain generalization, and prompt type sensitivity. We tested SAM on both natural images (COCO) and specialized underwater imagery (UIIS10K) to assess its generalization capabilities beyond the training distribution.

## Reproducibility Scope

We investigated three main claims from the original paper:

### Claim 1: Zero-shot Performance on COCO
SAM achieves competitive segmentation performance on COCO val2017 using standard Average Precision (AP) metrics with bounding box prompts.

### Claim 2: Cross-Domain Generalization
SAM generalizes to specialized domains (underwater instance segmentation on UIIS10K) without fine-tuning, maintaining strong zero-shot performance on out-of-distribution data.

### Claim 3: Prompt Type Sensitivity
More spatially-informative prompts (bounding boxes) yield better segmentation than ambiguous prompts (single points).

## Datasets

| Dataset | Domain | Images | Annotations | Categories | Purpose |
|---------|--------|--------|-------------|------------|---------|
| COCO val2017 | Natural images | 50 subset | 382 instances | Multiple | Validate baseline performance |
| UIIS10K | Underwater | 50 subset | 156 instances | 7 (fish, reefs, plants, seafloor, etc.) | Test domain generalization |

## Methodology

**Model:** SAM ViT-H (sam_vit_h_4b8939.pth) - largest checkpoint  
**Hardware:** Google Colab with Tesla T4 GPU  
**Inference Time:** ~2 seconds per image  
**Prompts Tested:**
- **Box prompts:** Ground-truth bounding boxes (simulating high-quality spatial guidance)
- **Point prompts:** Single center point of bounding box (simulating minimal guidance)

**Evaluation Metrics**:
- Pixel-level: IoU, Dice coefficient, Precision, Recall
- COCO-style: AP@[0.50:0.95], AP@0.50, AP@0.75, per-size AP (small/medium/large)

## Results

### COCO Dataset (Natural Images)

**Box Prompts**:
- Mean IoU: **0.778** (±0.170)
- AP@0.50: **0.935** (93.5% of predictions exceed 50% IoU)
- AP@[0.50:0.95]: **0.616** (exceeds original paper's automatic mode AP of 46.5%)

**Point Prompts**:
- Mean IoU: **0.409** (47% drop from box prompts)
- AP@0.50: **0.441** (53% drop)
- Large objects particularly struggled (AP_large: 0.006 vs. 0.706 for boxes)

### UIIS10K Dataset (Underwater Images)

**Box Prompts**:
- Mean IoU: **0.816** 
- AP@0.50: **0.947**

**Point Prompts**:
- Mean IoU: **0.599** (27% drop)
- Severe degradation on large, amorphous regions (seafloor: 67% IoU drop)


### Validated Claims

1. **Zero-shot capability confirmed:** SAM achieves strong performance without task-specific training (COCO AP@0.50: 0.935) [file:15]
2. **Cross-domain generalization works:** UIIS performance (AP@0.50: 0.947) matched or exceeded COCO, suggesting SAM learned domain-agnostic object boundaries [file:15]
3. **Prompt quality matters:** Box prompts consistently outperformed point prompts by 27-47% across both datasets [file:15]

### 💡 New Insights

- **Simpler domains may perform better:** Underwater images with cleaner backgrounds and fewer overlapping objects actually improved performance compared to dense urban COCO scenes
- **Size sensitivity with sparse prompts:** Point prompts failed catastrophically on large objects (AP_large: 0.006), indicating single points cannot capture spatial extent



