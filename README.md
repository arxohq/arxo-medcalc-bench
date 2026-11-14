# Arxo Materia 0.1: a 4B model reaches 90.2% on MedCalc-Bench Verified with executable medical knowledge

MedCalc-Bench asks a model to read a clinical note and compute one of 55 medical calculators: creatinine clearance, CHA2DS2-VASc, APACHE II and others. We split the task in two. Arxo Materia 0.1, a 4B model based on Qwen3.5-4B, reads the clinical note and writes down the patient facts. Arxo computes the answer from those facts and returns it with the steps that led to it.

Arxo Materia with Arxo is in early access. This repository publishes Arxo Materia 0.1 standalone, `Arxo-Materia-4B-MedCalc-0.1`, which reads the note and answers on its own. It covers the 55 calculators of MedCalc-Bench and is evaluated on its test split. The name comes from *materia medica*, the body of medical knowledge physicians worked from for centuries.

On the 1,100 test cases of MedCalc-Bench Verified, scored with the benchmark's own evaluator:

| Setup | Model | Accuracy |
|---|---|---|
| Model extracts facts, Arxo computes | Arxo Materia 0.1 (4B), based on Qwen3.5-4B | 992 / 1100 (90.2%) |
| Model extracts facts, Arxo computes | Claude Opus | 1054 / 1100 (95.8%) |
| Model answers alone, calculator tool for arithmetic | Arxo Materia 0.1, standalone variant | 84.9 ± 2.5% |
| Model answers alone, no tools | Arxo Materia 0.1, standalone variant | 82.2 ± 0.4% |
| Base model, no training, six examples in the prompt | Qwen3.5-4B | 471 / 1100 (42.8%) |

The 90.2% row is one training run. The two "answers alone" rows are Arxo Materia 0.1 standalone, the model published here; the figures are means over three training seeds. Its outputs, per-case scores and adapters are in `standalone/` and in the Release `v0.1`.

## By calculator type

| Setup | Formulas (720 cases) | Scoring scales (380 cases) |
|---|---|---|
| Arxo Materia 0.1 + Arxo | 709 (98.5%) | 283 (74.5%) |
| Claude Opus + Arxo | 718 (99.7%) | 336 (88.4%) |
| Qwen3.5-4B, no training, six examples | 444 (61.7%) | 27 (7.1%) |

<details>
<summary>All 55 calculators, correct answers out of 20</summary>

| Formula calculators | Arxo Materia 0.1 + Arxo | Arxo Materia 0.1 standalone, no tools, mean of three seeds | Qwen3.5-4B, no training, one example |
|---|---|---|---|
| Adjusted Body Weight | 20 | 20.0 | 20 |
| Albumin Corrected Anion Gap | 20 | 19.3 | 20 |
| Albumin Corrected Delta Gap | 20 | 20.0 | 20 |
| Albumin Corrected Delta Ratio | 20 | 19.7 | 20 |
| Anion Gap | 19 | 19.7 | 20 |
| Body Mass Index (BMI) | 20 | 20.0 | 20 |
| Body Surface Area Calculator | 20 | 20.0 | 20 |
| CKD-EPI Equations for Glomerular Filtration Rate | 19 | 15.0 | 3 |
| Calcium Correction for Hypoalbuminemia | 20 | 20.0 | 20 |
| Creatinine Clearance (Cockcroft-Gault Equation) | 20 | 15.7 | 13 |
| Delta Gap | 20 | 19.0 | 19 |
| Delta Ratio | 20 | 20.0 | 20 |
| Estimated Date of Conception | 20 | 20.0 | 19 |
| Estimated Due Date | 20 | 1.7 | 5 |
| Estimated Gestational Age | 20 | 9.0 | 19 |
| Fibrosis-4 (FIB-4) Index for Liver Fibrosis | 20 | 20.0 | 15 |
| Fractional Excretion of Sodium (FENa) | 20 | 19.3 | 20 |
| Framingham Risk Score for Hard Coronary Heart Disease | 16 | 16.0 | 4 |
| Free Water Deficit | 20 | 19.7 | 15 |
| HOMA-IR (Homeostatic Model Assessment for Insulin Resistance) | 18 | 20.0 | 20 |
| Ideal Body Weight | 20 | 20.0 | 20 |
| LDL Calculated | 20 | 20.0 | 19 |
| MDRD GFR Equation | 19 | 16.3 | 2 |
| MELD Na (UNOS/OPTN) | 18 | 10.3 | 13 |
| Maintenance Fluids Calculations | 20 | 19.3 | 3 |
| Mean Arterial Pressure (MAP) | 20 | 20.0 | 20 |
| Morphine Milligram Equivalents (MME) Calculator | 20 | 18.3 | 4 |
| QTc Bazett Calculator | 20 | 20.0 | 20 |
| QTc Framingham Calculator | 20 | 20.0 | 20 |
| QTc Fridericia Calculator | 20 | 20.0 | 20 |
| QTc Hodges Calculator | 20 | 20.0 | 20 |
| QTc Rautaharju Calculator | 20 | 20.0 | 20 |
| Serum Osmolality | 20 | 20.0 | 18 |
| Sodium Correction for Hyperglycemia | 20 | 19.7 | 20 |
| Steroid Conversion Calculator | 20 | 20.0 | 3 |
| Target weight | 20 | 20.0 | 20 |

| Scoring scales | Arxo Materia 0.1 + Arxo | Arxo Materia 0.1 standalone, no tools, mean of three seeds | Qwen3.5-4B, no training, one example |
|---|---|---|---|
| APACHE II Score | 19 | 14.0 | 0 |
| CHA2DS2-VASc Score for Atrial Fibrillation Stroke Risk | 14 | 15.0 | 10 |
| CURB-65 Score for Pneumonia Severity | 19 | 17.3 | 14 |
| Caprini Score for Venous Thromboembolism (2005) | 18 | 7.0 | 2 |
| Centor Score (Modified/McIsaac) for Strep Pharyngitis | 18 | 16.3 | 6 |
| Charlson Comorbidity Index (CCI) | 13 | 10.7 | 5 |
| Child-Pugh Score for Cirrhosis Mortality | 16 | 12.3 | 9 |
| FeverPAIN Score for Strep Pharyngitis | 5 | 8.7 | 7 |
| Glasgow Coma Score (GCS) | 19 | 12.7 | 6 |
| Glasgow-Blatchford Bleeding Score (GBS) | 15 | 15.3 | 3 |
| HAS-BLED Score for Major Bleeding Risk | 14 | 13.0 | 12 |
| HEART Score for Major Cardiac Events | 9 | 5.7 | 4 |
| PERC Rule for Pulmonary Embolism | 16 | 19.0 | 17 |
| PSI Score: Pneumonia Severity Index for CAP | 20 | 17.3 | 3 |
| Revised Cardiac Risk Index for Pre-Operative Risk | 16 | 13.0 | 15 |
| SIRS Criteria | 19 | 19.0 | 16 |
| Sequential Organ Failure Assessment (SOFA) Score | 7 | 1.3 | 3 |
| Wells' Criteria for DVT | 18 | 14.7 | 5 |
| Wells' Criteria for Pulmonary Embolism | 8 | 13.7 | 12 |

</details>

## Arxo Materia 0.1 standalone: the model answers alone

Here Arxo Materia reads the clinical note and writes the answer itself, without Arxo. There are two variants: in the first the model does the arithmetic on its own, in the second a calculator tool does it.

Both are Arxo Materia 0.1 (4B), based on Qwen3.5-4B, three training seeds each. 1,100 test cases of MedCalc-Bench Verified, the benchmark's own evaluator:

| Setup | Seed 0 | Seed 1 | Seed 2 | Mean |
|---|---|---|---|---|
| Arxo Materia 0.1 standalone, no tools | 900 (81.8%) | 909 (82.6%) | 903 (82.1%) | 82.2 ± 0.4% |
| Arxo Materia 0.1 standalone, calculator tool for arithmetic | 958 (87.1%) | 939 (85.4%) | 904 (82.2%) | 84.9 ± 2.5% |
| Qwen3.5-4B, no training, one example of the same calculator in the prompt, no tools | 723 (65.7%) | | | |
| Qwen3.5-4B, no training, one example of the same calculator in the prompt, calculator tool | 765 (69.5%) | | | |

| Setup | Formulas (720 cases) | Scoring scales (380 cases) |
|---|---|---|
| Arxo Materia 0.1 standalone, no tools, mean of three seeds | 91.4 ± 0.6% | 64.7 ± 1.3% |
| Arxo Materia 0.1 standalone, calculator tool, mean of three seeds | 95.8 ± 2.4% | 64.2 ± 3.5% |
| Qwen3.5-4B, no training, no tools | 574 (79.7%) | 149 (39.2%) |
| Qwen3.5-4B, no training, calculator tool | 632 (87.8%) | 133 (35.0%) |

The untrained rows use the same prompt and the same token limit as the trained ones.

## Arxo Materia 0.1 (0.8B)

Arxo Materia 0.1 (0.8B), `Arxo-Materia-0.8B-MedCalc-0.1`, is the same model built on Qwen3.5-0.8B. It answers alone, without tools. One training run:

| Setup | Accuracy | Formulas (720 cases) | Scoring scales (380 cases) |
|---|---|---|---|
| Arxo Materia 0.1 (0.8B), based on Qwen3.5-0.8B | 780 / 1100 (70.9%) | 589 (81.8%) | 191 (50.3%) |
| Qwen3.5-0.8B, no training, one example of the same calculator in the prompt | 346 / 1100 (31.5%) | 283 (39.3%) | 63 (16.6%) |

## Reference answers that differ from the source formula

Four places where the benchmark's reference answer follows its calculator code and not the source formula:

| Calculator | What differs | Example |
|---|---|---|
| HOMA-IR | `homa_ir.py` converts insulin from pmol/L to µIU/mL by multiplying by 6. With 1 µIU/mL = 6 pmol/L the conversion divides by 6. 32 training rows are affected | Row 5880: insulin 756 pmol/L, glucose 97.3 mg/dL. Reference answer 1089.76; dividing gives 30.27 |
| MELD Na | In `meldna.py` the dialysis check is an `elif` after the `creatinine < 1.0` branch, so it is skipped for patients with two or more dialysis treatments in the past week and creatinine below 1 mg/dL | Rows 10130 and 10174: reference answers 29 and 27; with the dialysis value of 4.0 mg/dL the scores are 38 and 35 |
| Glasgow-Blatchford | `28 <= bun <= 70` gives 4 points at BUN of exactly 70 mg/dL. Taking 70 mg/dL as the rounded start of the urea 25 mmol/L band gives 6 points | Row 9692: reference answer 8; with that convention 10 |
| HEART | The ECG input is "Normal" in all 188 training cases, including notes that report ECG changes | Row 4107: the note reports "ST Elevation and Q waves in II, III, and aVF" |

Row numbers refer to `train_data.csv` of MedCalc-Bench Verified at commit `801592132bfd`. The HOMA-IR and Glasgow-Blatchford cases are also listed in the audit arXiv 2603.02222. The first three were reported to the benchmark authors on 13 November 2026.

## Published results on MedCalc-Bench Verified

Systems that take the clinical note and the question as input.

| System | Trained on the task | Accuracy | Source |
|---|---|---|---|
| **Arxo Materia 0.1 + Arxo** | yes | **90.2%** | this note; Arxo is in early access |
| Qwen3-8B extracts inputs, calculators written by GPT-5.5 | yes | 86.73% | [arXiv 2610.21477](https://arxiv.org/abs/2610.21477), Table II |
| Claude Sonnet 4.6, code agent with Python | no | 86.24% | reported in arXiv 2610.21477 |
| **Arxo Materia 0.1 standalone, calculator tool** | yes | **84.9%** | this repository, three seeds |
| **Arxo Materia 0.1 standalone, no tools** | yes | **82.2%** | this repository, three seeds |
| Nemotron 3.5 Super VL (120B) + LoRA, direct answer | yes | 72.45% | [Fastino model card](https://huggingface.co/fastino/Fastino-Nemotron-3.5-Super-VL-Healthcare) |
| **Arxo Materia 0.1 (0.8B), no tools** | yes | **70.9%** | this repository |
| Qwen3-8B, supervised fine-tuning, direct answer | yes | 69.46% | arXiv 2610.21477, Table II |
| Nemotron 3.5 Super VL (120B), direct answer | no | 65.91% | Fastino model card |
| Qwen3-8B, reinforcement learning, direct answer | yes | 64.2% | [arXiv 2512.19691](https://arxiv.org/abs/2512.19691), Appendix E; December 2025 snapshot of Verified |
| Qwen3-8B, one example in the prompt | no | 62.73% | arXiv 2610.21477, Table II |
| GLM-4.6V, direct answer | no | 51.9% | [arXiv 2603.02222](https://arxiv.org/abs/2603.02222), Table 3 |
| Qwen2.5-32B writes Python from the note alone | no | 44.71% | [arXiv 2609.10728](https://arxiv.org/abs/2609.10728), Table 1 |
| Best direct prompting on the MedHELM leaderboard | no | 34.8% | as cited in arXiv 2603.02222 |
| Qwen2.5-32B, direct answer from the note | no | 28.67% | arXiv 2609.10728, Table 1; first 120 words of the note |

## Results with the calculator supplied in the prompt

A different setting: the model is handed the formula or the calculator description, and in two rows the gold input values as well.

| System | What the prompt adds | Accuracy | Source |
|---|---|---|---|
| Qwen2.5-32B writes Python | formula and gold input values | 90.53% | arXiv 2609.10728, Table 1 |
| GLM-4.7 | calculator specification | 85.5% on 275 cases | arXiv 2603.02222, Table 3 |
| Qwen2.5-32B does the arithmetic | formula and gold input values | 83.47% | arXiv 2609.10728, Table 1 |
| GLM-4.6V | calculator specification | 81.5% | arXiv 2603.02222, Table 3 |

Arxo Materia 0.1 with Arxo reaches the same level from the note alone.

## What we did not do

- The test split stayed sealed until measurement.
- No test note is in the training data: training notes that also appear in the test set were removed.
- No prompt was tuned on test cases, and no score comes from our own grader: every figure uses the benchmark's evaluator.

## Scope of the numbers

The scores refer to the Verified test split. On the 887 relabelled items of the original release from arXiv 2512.19691, the model that answers alone, without tools, scores 68.7%.

## Reproducing the result

Arxo Materia 0.1 standalone runs on its own: the adapters are in the Release, its outputs and per-case scores in `standalone/` and `small/`. The 90.2% result needs Arxo, which is in early access: to run it on your own notes, or to verify the result independently, write to hi@arxo.io.

## What is in this repository

| Path | Contents |
|---|---|
| `standalone/answers/`, `standalone/eval/` | Arxo Materia 0.1 standalone: model outputs for the 1,100 test cases, three seeds without tools and three with the calculator tool, the two untrained runs, and per-case scores |
| `standalone/adapters/` and the Release `v0.1` | Adapter configs and LoRA weights of the six Arxo Materia 0.1 standalone runs (about 300 MB each) |
| `small/` and the Release `v0.1` | Arxo Materia 0.1 (0.8B): answers, per-case scores, adapter config; weights (about 100 MB) in the Release |
| `SHA256SUMS` | Checksums of every file, weights included |

Load an adapter with `AutoModelForImageTextToText`; it does not attach to `AutoModelForCausalLM`. Save a weights file from the Release as `adapter_model.safetensors` next to its `adapter_config.json`.

Licences: the adapters are released under Apache-2.0, the licence of the base models Qwen/Qwen3.5-4B and Qwen/Qwen3.5-0.8B. Answers and evaluation outputs refer to MedCalc-Bench cases and are released under CC-BY-SA 4.0, the licence of the benchmark.
