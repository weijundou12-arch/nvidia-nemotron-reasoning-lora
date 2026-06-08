# NVIDIA Nemotron Reasoning LoRA Project

This repository documents my project for the **NVIDIA Nemotron Model Reasoning Challenge** on Kaggle. The goal of the competition is to improve reasoning accuracy using NVIDIA Nemotron open models on a novel reasoning benchmark. The final submission is expected to produce a compatible LoRA adapter for the Nemotron-3-Nano-30B base model.

## Project Overview

This project focuses on lightweight fine-tuning and error-driven improvement for reasoning tasks. The main technical direction is to use **LoRA-based supervised fine-tuning**, prompt engineering, synthetic reasoning data generation, and category-level failure analysis to improve the model's reasoning performance.

The project is based on three main stages:

1. Understanding the competition, dataset format, baseline solution, and Nemotron model architecture.
2. Designing reasoning prompts, Chain-of-Thought style supervision, and synthetic data for difficult reasoning categories.
3. Studying high-scoring training pipelines and building a reproducible LoRA fine-tuning workflow.

## Main Methods

### 1. LoRA Fine-tuning

The project uses Low-Rank Adaptation to fine-tune the model efficiently without updating all base model parameters. The main focus is on training lightweight adapter weights rather than full model fine-tuning.

Key ideas include:

* LoRA adapter training
* Rank-limited adapter configuration
* Target module selection for attention, feed-forward, and MoE-related layers
* Mixed precision training
* Adapter saving and conversion for Kaggle submission

### 2. Prompt Engineering and CoT Design

Prompt engineering is used to improve model reasoning behavior, especially for puzzle-style tasks. The project explores:

* Zero-shot prompting
* Few-shot prompting
* Chain-of-Thought style reasoning traces
* Structured final-answer formatting using `\boxed{}`

### 3. Synthetic Data Generation

Synthetic data generation is used to strengthen weak reasoning categories. The main idea is to create additional reasoning examples with explicit rule discovery, candidate enumeration, verification, and elimination steps.

The current priority is to improve difficult symbolic reasoning tasks such as cryptarithm-style problems, where the model often fails to infer hidden transformation rules.

### 4. Error Analysis

The project includes category-level error analysis to identify the main sources of score loss. Instead of blindly increasing training data, the workflow first analyzes failed examples and then creates targeted training data for the weakest categories.

Important analysis targets include:

* Problem category
* Predicted answer
* Ground-truth answer
* Whether `\boxed{}` extraction succeeds
* Minimum log probability
* Category-level accuracy
* Error pattern summary

## Current Project Status

The current project has completed the following components:

* Competition rule and baseline understanding
* Nemotron model architecture review
* Prompt engineering and CoT strategy review
* High-score training code analysis
* LoRA fine-tuning pipeline study
* Adapter conversion and submission workflow review
* Initial improvement plan based on category-level failure analysis

The next improvement direction is to build a targeted second-stage SFT dataset for weak reasoning categories, especially cryptarithm-style problems.

## Proposed Improvement Direction

The most practical next step is:

> Targeted second-stage LoRA SFT using synthetic cryptarithm reasoning data.

The new data should not disrupt the original training order. Instead, it should be used as a separate second-stage fine-tuning step with a lower learning rate and a small number of training steps.

The synthetic examples should follow a clear reasoning format:

1. Identify candidate transformation rules.
2. Enumerate possible mappings.
3. Verify each candidate against given examples.
4. Eliminate inconsistent rules.
5. Produce the final answer in `\boxed{}` format.

## Repository Structure

```text
nvidia-nemotron-reasoning-lora/
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
├── notebooks/
│   └── 0603-adapter.ipynb
├── src/
│   ├── data/
│   │   ├── build_synthetic_data.py
│   │   └── filter_training_examples.py
│   ├── training/
│   │   ├── train_lora.py
│   │   └── train_second_stage.py
│   ├── inference/
│   │   ├── generate_predictions.py
│   │   └── extract_final_answer.py
│   ├── evaluation/
│   │   ├── evaluate_predictions.py
│   │   └── error_analysis.py
│   └── utils/
│       └── common.py
├── configs/
│   ├── lora_config.yaml
│   └── training_config.yaml
├── reports/
│   ├── project_summary.md
│   ├── error_analysis.md
│   └── improvement_plan.md
├── slides/
│   ├── lesson1_competition_baseline.pptx
│   ├── lesson2_prompting_synthetic_data.pptx
│   └── lesson3_high_score_code_analysis.pptx
└── outputs/
    └── .gitkeep
```

## Notes

This repository is intended for project documentation and reproducible code organization. Large files such as Kaggle datasets, model checkpoints, LoRA adapter weights, and submission archives should not be committed to GitHub.

## Disclaimer

This is an educational and research-oriented repository for the Kaggle NVIDIA Nemotron Model Reasoning Challenge. It does not include private competition data, API keys, or large model weights.
