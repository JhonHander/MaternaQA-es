---
language: es
license: apache-2.0
tags:
  - medical-qa
  - obstetrics
  - spanish
  - maternal-health
  - perinatal-health
  - question-answering
  - synthetic-data
  - sft
  - grounded-generation
  - maternaqa
dataset_info:
  splits:
    - name: train
      num_examples: 5093
    - name: validation
      num_examples: 306
    - name: test
      num_examples: 328
  config_names:
    - sft_closed_book
    - sft_grounded
    - qa_flat_jsonl
    - final
---

# MaternaQA-es

<p align="center">
  <img src="https://huggingface.co/datasets/iue-edu/MaternaQA-es/resolve/main/app-icon.png" alt="MaternaQA-es / MaternaCare-ES visual identity" width="180" />
</p>

**MaternaQA-es** is a Spanish clinical question-answering dataset for research on maternal and perinatal health. It contains **5,727 synthetic QA pairs** generated from a curated obstetrics corpus and released in formats suitable for supervised fine-tuning, grounded generation, and audit/evaluation workflows.

The dataset focuses on pregnancy, labor, postpartum care, fetal monitoring, obstetric risk factors, and related maternal-health topics. It is designed for Spanish-language NLP research, not for direct clinical use.

## Why this dataset?

High-quality Spanish medical QA resources are still limited, especially for maternal-health scenarios. MaternaQA-es provides a reproducible dataset derived from clinical PDF sources, with explicit train/validation/test splits and grounded variants that preserve source context for safer evaluation and fine-tuning.

## Dataset variants

| Configuration | Description | Typical use |
|---|---|---|
| `sft_closed_book` | Conversational SFT records where the model receives only the question. | Closed-book instruction tuning and baseline comparison. |
| `sft_grounded` | Conversational SFT records where the model receives source context plus the question. | Grounded answer generation and domain adaptation. |
| `qa_flat_jsonl` | Flat records with question, answer, source context, provenance, and metadata. | Auditing, analysis, evaluation, and custom preprocessing. |
| `final` | Final generation outputs, reports, summaries, and evaluation artifacts. | Reproducibility and quality inspection. |

## Splits

| Split | QA pairs | Source chunks | Source PDFs |
|---|---:|---:|---:|
| Train | 5,093 | 1,744 | 52 |
| Validation | 306 | 101 | 2 |
| Test | 328 | 108 | 3 |
| **Total** | **5,727** | **1,953** | **57** |

The upstream extraction pipeline processed 63 PDFs, extracted 5,856 pages, retained 5,176 pages, generated 2,268 chunks, and published 2,223 language-model chunks with no train/validation/test source leakage.

## Quick start

```python
from datasets import load_dataset

# Grounded SFT format: context + question -> answer
dataset = load_dataset("iue-edu/MaternaQA-es", "sft_grounded")
print(dataset["train"][0])
```

For audit-oriented records:

```python
from datasets import load_dataset

records = load_dataset("iue-edu/MaternaQA-es", "qa_flat_jsonl")
print(records["test"].features)
```

## Quality signals

The dataset includes generation and evaluation reports to support transparent inspection.

### Grounding overlap during generation

These values come from the available final generation reports. The train generation report covers 5,015 generated QA pairs, while the released train split contains 5,093 final QA pairs.

| Split/report | Average overlap | Low-grounding rate |
|---|---:|---:|
| Train generation report | 0.6836 | 0.0054 |
| Validation generation report | 0.6890 | 0.0065 |
| Test generation report | 0.6613 | 0.0152 |

### RAGAS sample evaluation

| Split/sample | Faithfulness | Answer relevancy |
|---|---:|---:|
| Train sample, n=300 | 0.7726 | 0.6466 |
| Validation sample, n=100 | 0.7826 | 0.6812 |
| Test sample, n=100 | 0.7132 | 0.5583 |

RAGAS scores are quality indicators for dataset inspection. They are **not** clinical validation and should not be interpreted as proof of medical correctness.

## Related models

QLoRA adapters trained with the grounded SFT variant are available at:

- [`iue-edu/MaternaCare-ES-gemma4-qlora`](https://huggingface.co/iue-edu/MaternaCare-ES-gemma4-qlora)
- [`iue-edu/MaternaCare-ES-medgemma-qlora`](https://huggingface.co/iue-edu/MaternaCare-ES-medgemma-qlora)

Fine-tuning and evaluation code is maintained separately in [`JhonHander/MaternaCare-ES`](https://github.com/JhonHander/MaternaCare-ES). The dataset creation pipeline is maintained in [`NicolasHoyosDevss/MaternaQA-es`](https://github.com/NicolasHoyosDevss/MaternaQA-es).

## Intended use

MaternaQA-es is intended for:

- Spanish-language medical NLP research.
- Supervised fine-tuning experiments for maternal-health QA.
- Evaluation of grounded vs. closed-book answer generation.
- Dataset analysis, benchmarking, and academic reproducibility.

## Out-of-scope use

MaternaQA-es must **not** be used as a standalone medical authority. Models trained on this dataset should not be used for diagnosis, treatment decisions, triage, emergency care, or patient-facing clinical guidance without independent clinical validation, safety testing, and professional oversight.

## Limitations

- The QA pairs are synthetic and should be audited before high-stakes research claims.
- Source documents and generated answers may reflect guideline scope, publication age, and extraction artifacts.
- Automatic metrics evaluate textual properties; they do not replace expert medical review.
- The dataset is Spanish-language and focused on obstetrics/maternal-health contexts, so results may not generalize to other languages or medical specialties.

## License

Released under the Apache 2.0 license, subject to the terms and constraints of the original source materials and responsible research use.
