# Instruction Tuning Large Language Models

**CIS6930 — Large Language Models · Fall 2025 · University of Florida**

Parameter-efficient instruction tuning of **Mistral-7B-Instruct** with LoRA/QLoRA on a
blended instruction corpus, specialized into an AI teaching assistant for the CIS6930
course.

**Contributors:** Christopher Bowers, Gowtham Mende, Ian Arnold, Sanjeev Kamath

---

## Overview

Instruction-tuned LLMs power many conversational and educational systems, but still
struggle to reliably follow complex instructions and to explain technical content at
varying levels of difficulty. Academic settings add a second constraint: limited access to
large proprietary instruction datasets and to the compute needed for full-model
fine-tuning or large-scale RLHF.

This project asks whether **parameter-efficient fine-tuning (LoRA/QLoRA)** on a curated
blend of instruction datasets can improve Mistral-7B's instruction-following while
simultaneously adapting it to serve as a course-specific tutor — all under constrained GPU
memory.

The goal is twofold:

1. Improve general instruction-following across **conversational, code/math, and symbolic
   reasoning** tasks.
2. Specialize the model as an **AI teaching assistant** for CIS6930 via an additional
   stage on course lecture slides.

## Approach

The pipeline trains low-rank adapters on top of a **4-bit quantized Mistral-7B-Instruct**
backbone, updating only a small fraction of parameters so the base model's strengths are
preserved. Training blends three complementary datasets under a standardized
pre-processing and merging strategy, followed by a specialization stage on course
material.

| Dataset | Role |
|---|---|
| **UltraChat 200k** | Multi-turn dialogue |
| **Infinity-Instruct** | Code- and math-oriented instructions |
| **Symbolic IT** | Logic and structured reasoning |
| **CIS6930 lecture slides** | Course-specific specialization stage |

## Results

- On a **400-example held-out instruction set**, mean token-level F1 improved from
  **~0.31 → ~0.34** — roughly a **10% relative gain** — with the largest benefits on
  moderately difficult Infinity-Instruct tasks.
- A complementary experiment on CIS6930 lecture slides reported **low perplexity** and
  **high token-level reconstruction F1**, indicating the model closely captures course
  material and can reproduce key slide content while retaining fluency.

These results suggest that parameter-efficient adaptation with LoRA/QLoRA, paired with a
carefully curated blend of instruction datasets, is an effective and computationally
practical strategy for building course-specific "AI tutors" on top of strong open-source
base models.

## Datasets

- [UltraChat 200k](https://huggingface.co/datasets/HuggingFaceH4/ultrachat_200k) — multi-turn dialogue
- [Infinity-Instruct](https://huggingface.co/datasets/BAAI/Infinity-Instruct) — code and math instructions
- [Symbolic Instruction Tuning](https://huggingface.co/datasets/sail/symbolic-instruction-tuning) — logic and structured reasoning

## Model

- **Base:** Mistral-7B-Instruct (4-bit quantized)
- **Adaptation:** LoRA / QLoRA low-rank adapters
- **Backbone strategy:** quantized base + trainable adapters for fine-tuning under
  constrained GPU memory

## Future Work

- Preference-based training (e.g. DPO/RLHF-style alignment)
- Evaluation on public instruction-following benchmarks
- Broader specialization stages beyond a single course

## Reference

Full write-up: `LLM_Paper.pdf` — *Instruction Tuning Large Language Models*, CIS6930,
Fall 2025.
