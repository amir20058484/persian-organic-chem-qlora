````markdown
# carey-qlora-fa-mcq

A small end-to-end project: **Persian multiple-choice (MCQ) dataset (100 questions)** + **QLoRA/LoRA fine-tuning** of **Qwen2.5-3B-Instruct** on Kaggle, with **before/after accuracy** evaluation.

## What’s inside this repo
- A single **notebook** that runs the whole pipeline:
  1) load the dataset  
  2) split into **80% train / 20% test**  
  3) evaluate the **base model** (baseline)  
  4) build an SFT file (`train_sft.jsonl`)  
  5) fine-tune using **QLoRA/LoRA**  
  6) evaluate again after fine-tuning
- A **dataset JSON file (100 Persian MCQs)** based on *Advanced Organic Chemistry (Carey)*.

## Dataset format
Each question follows this structure:
```json
{
  "id": "001",
  "question": "...",
  "options": ["A: ...", "B: ...", "C: ...", "D: ..."],
  "correct": "B",
  "reasoning": "..."
}
````

## Model & Method

* **Base model:** `Qwen/Qwen2.5-3B-Instruct`
* **Loading:** 4-bit NF4 quantization (bitsandbytes) to fit on Kaggle T4 GPU
* **Fine-tuning:** LoRA adapters (QLoRA-style workflow)
* **Task:** Persian MCQ answering (output must be one of `A/B/C/D`)

## Results (this run)

* **Baseline accuracy:** 0.75
* **After fine-tuning:** 0.85
* **Absolute improvement:** +0.10

> Note: the test set contains 20 questions, so each question changes accuracy by 0.05.

## How to run

1. Open the notebook (Kaggle recommended; GPU required).
2. Place the dataset JSON in the path expected by the notebook (or update the path variable).
3. Run all cells in order.

### Dependencies

The notebook installs required packages, including:

* `transformers`, `accelerate`, `bitsandbytes`, `peft`, `datasets`

## Notes

* The initial idea was to cover the whole book, but with only 100 questions the coverage per topic is limited.
* To increase learning signal, the scope was narrowed to a **single chapter** for the final fine-tuning run.
