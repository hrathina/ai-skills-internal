# Upstream Contribution Opportunities — July 1, 2026

> Issues and PRs across Fine Tuning team workstreams. Max 3 per repo, balanced
> across workstreams.

## Quick Wins

### 1. Document how to use TrainJob Progress feature
- **Repo**: kubeflow/trainer#3553
- **Workstream**: Kubeflow SDK
- **Why contribute**: Labeled good first issue and help wanted — documents the TrainJob Progress API the team exposes through the Kubeflow SDK. Good entry point for SDK contributors.
- **Effort**: Small
- **Link**: https://github.com/kubeflow/trainer/issues/3553

## Feature Contributions

### 2. Support GRPO-style post-training in Kubeflow Trainer via TrainingRuntime
- **Repo**: kubeflow/trainer#3508
- **Workstream**: Kubeflow SDK
- **What's needed**: Add TrainingRuntime and Torch plugin support for GRPO-style post-training jobs.
- **Why it matters**: Directly aligns with the team's GRPO/RLVR integration path into RHOAI via Kubeflow TrainJob and algorithm registry.
- **Effort**: Large
- **Link**: https://github.com/kubeflow/trainer/issues/3508

### 3. Reasoning reward utilities: GRPO-LEAD + graduated format reward
- **Repo**: huggingface/trl#6221
- **Workstream**: RLVR/GRPO
- **What's needed**: Add group-relative length-scaled accuracy (GRPO-LEAD) and graduated format reward utilities for reasoning tasks.
- **Why it matters**: Reward shaping utilities the team can use in GRPOTrainer-based LoRA GRPO and full-weight GRPO backends.
- **Effort**: Medium
- **Link**: https://github.com/huggingface/trl/issues/6221

### 4. Discussion: testing strategy for instructlab/training as a backend library
- **Repo**: instructlab/training#705
- **Workstream**: InstructLab Training
- **What's needed**: Define a testing strategy for using instructlab/training as an embeddable backend library (the team's SFT path).
- **Why it matters**: The team consumes instructlab/training as the SFT backend — a clear testing contract reduces integration risk in Training Hub.
- **Effort**: Medium
- **Link**: https://github.com/instructlab/training/issues/705

### 5. Create templated training notebooks for supported algorithms
- **Repo**: Red-Hat-AI-Innovation-Team/training_hub#69
- **Workstream**: Guided Examples
- **What's needed**: Notebook templates covering each supported Training Hub algorithm (SFT, OSFT, LoRA SFT, GRPO, etc.).
- **Why it matters**: Directly feeds RHOAI guided examples and release documentation for each algorithm.
- **Effort**: Medium
- **Link**: https://github.com/Red-Hat-AI-Innovation-Team/training_hub/issues/69

## Bug Fixes

### 6. sft() fails: TrainingArgs requires data_output_dir and max_batch_len but API marks them optional
- **Repo**: Red-Hat-AI-Innovation-Team/training_hub#94
- **Workstream**: Training Hub Integration
- **Impact**: sft() convenience function breaks when optional parameters are omitted — affects the primary SFT entry point the team ships.
- **Effort**: Small
- **Link**: https://github.com/Red-Hat-AI-Innovation-Team/training_hub/issues/94

### 7. lora_sft() fails with padding_free/packing conflict on Unsloth 2026.6.1
- **Repo**: Red-Hat-AI-Innovation-Team/training_hub#93
- **Workstream**: Training Hub Integration
- **Impact**: LoRA SFT path breaks on current Unsloth — blocks the team's primary LoRA fine-tuning workflow.
- **Effort**: Small
- **Link**: https://github.com/Red-Hat-AI-Innovation-Team/training_hub/issues/93

### 8. GRPOTrainer.evaluate() with environment_factory crashes on partial batches
- **Repo**: huggingface/trl#6129
- **Workstream**: RLVR/GRPO
- **Impact**: Evaluation path fails for agent/environment-based GRPO — relevant to team's GRPO rollout and reward integration work.
- **Effort**: Medium
- **Link**: https://github.com/huggingface/trl/issues/6129

### 9. AdaLora's update_and_allocate method is lost in inject_adapter_in_model()
- **Repo**: huggingface/peft#3373
- **Workstream**: SFT/LoRA Fine-Tuning
- **Impact**: AdaLoRA rank pruning silently broken after adapter injection — affects LoRA adapter workflows the team uses via PEFT.
- **Effort**: Medium
- **Link**: https://github.com/huggingface/peft/issues/3373

### 10. LoRA fine-tuning of an FP8 checkpoint is blocked
- **Repo**: huggingface/transformers#46736
- **Workstream**: SFT/LoRA Fine-Tuning
- **Impact**: get_peft_model does not clear validate_quantization_for_training for FP8 checkpoints — blocks LoRA on quantized models the team may target in training images.
- **Effort**: Medium
- **Link**: https://github.com/huggingface/transformers/issues/46736

## Reviews Needed

No training-relevant open PRs found this scan. The 7 open PRs in instructlab/training are dependency and CI/CD bumps only.

---

### Coverage Summary

| Workstream | Items found | In top 10 |
|---|---|---|
| Training Hub Integration | 12 | 2 |
| Kubeflow SDK | 8 | 2 |
| InstructLab Training | 3 | 1 |
| RLVR/GRPO | 28 | 2 |
| SFT/LoRA Fine-Tuning | 22 | 2 |
| KFP Pipelines | 0 | 0 |
| Training Images | 2 | 0 |
| Guided Examples | 3 | 1 |

Scanned: ~170 issues and 7 PRs across 7 repos
Generated: July 1, 2026

**Gaps this scan:** No KFP pipeline or training-image issues made the balanced top 10. kubeflow/training-operator returned no SDK/migration issues. Consider broader searches for pipeline component, container image, and FSDP if those workstreams need coverage next run.

---

**Want this delivered regularly?**

```
/loop 24h /research-intelligence-feed
```
