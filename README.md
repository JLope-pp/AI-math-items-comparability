# Scaling Standardized Mathematics Item Generation with Generative AI

Public, reproducible companion to the paper of the same title.

## Contents

```text
paper_ia_items_matematica_github/
├── data/                 Anonymized pilot data, fully in English
└── notebook/
    └── paper_replication.ipynb   Every table and figure in the Results section and the appendices
```

## Data (`data/`)

All files are anonymized and translated to English. The pilot involved 209 students in six public schools in Metropolitan Lima (February 2026). In this data:

- Each student is identified only by `student_id` (e.g. `EST0001`), consistent across all four student-level files. There are no names, no birthdates, no national ID numbers.
- Each school is identified only by `school_id` (`Colegio_1`...`Colegio_6`). Real school names are not included.
- `questionnaire_anonymized.csv`: student questionnaire (sex, background questions).
- `test_1_anonymized.csv`, `test_2_anonymized.csv`, `test_3_anonymized.csv`: item-level responses to the three test forms (30 items each: 15 from the item bank, 15 AI-generated).
- `item_bank_expert_review.xlsx`: the item bank plus the specialist and teacher technical review of all 120 AI-generated items. Reviewer identity is a numeric code (1-6), not a name.

See **`data/CODEBOOK.md`** for the full column-by-column dictionary, including what each questionnaire code (`p1`...`p13h`) means.

The math item stems (`question_text_{N}` in the test files) were translated from the original Spanish pilot instrument to English, preserving every number, operation, unit, and quantity exactly. Only the wording changed. A couple of fields (`item_type_code`, `content_topic`) are kept as their original numeric or letter codes, since no text label for them exists in the codebook.

## Notebook (`notebook/paper_replication.ipynb`)

One notebook, reading only from `../data/`, that follows the paper section by section and uses the same numbering:

- **5.1** Table 1, technical quality of the 120 AI-generated items by intended cognitive demand, and the teacher review
- **5.2** Figure 1, observed proportion of correct responses by item origin and intended demand
- **5.3** Table 2, probit estimates for the probability of a correct response, controlling for the student's score on the bank items other than the target item (`RestBankScore`), with standard errors clustered by student
- **5.4** Table 3, average predicted probabilities and AI-bank differences within each demand level (RQ1)
- **5.5** Table 4, differences between AI-bank gaps across demand levels (RQ2)
- **5.6** Figure 2, predicted probabilities for AI-generated items across demand levels (RQ3), an independent check of every predicted probability and contrast with the `marginaleffects` package, and the robustness checks (standard errors clustered by student and item, the control score as a proportion, and test-form fixed effects)
- **5.7.1** Table 5, item-rest correlations
- **5.7.2** Table 6, Cronbach's alpha and McDonald's omega of each test form
- **5.7.3** Table 7, empirical distractor functioning
- **5.7.4** Table 8, exploratory sex-related differential performance
- **5.7.5** Figures 3 and 4, item characteristic curves and test information from a one-parameter IRT model estimated with all 30 items of each form
- **Appendices A to F**: Tables A1, B1, C1, D1, E1, E2 and F1

The probit model is equivalent to Stata's `probit correct c.RestBankScore i.AI##i.demand, vce(cluster student_id)` followed by `margins AI#demand` and `lincom`. Each table is written to `notebook/outputs/tables/` and each figure to `notebook/outputs/figures/` when the notebook runs.

## How to run it

```bash
pip install pandas numpy matplotlib scipy statsmodels openpyxl
pip install factor_analyzer          # Table 6 (McDonald's omega)
pip install sentence-transformers    # Table D1
pip install bert-score               # Table D1
pip install marginaleffects          # optional, independent check in Section 5.6

jupyter notebook notebook/paper_replication.ipynb
```

The notebook runs in under a minute, except Table D1, which downloads a multilingual sentence-embedding model and a multilingual BERT model the first time it runs.

## Notes

- **Intended demand in Tables 1 and C1.** Each language model generated 30 items with consecutive identifiers, in three blocks of 10 (high, medium and low intended demand). The notebook takes the intended level of each item from its generation block, which gives 40 items per level. Three items (229, 256 and 260) have a review-form code that differs from their block.
- **Table D1.** Semantic similarity is computed on the English item stems in this repository, while the paper's values were computed on the original Spanish stems. The number of items compared is the same, and the similarity values differ slightly.
- **Table A1.** Three rows describe settings of the Eval-IA generation tool that are not stored in the data: the 20 reference items used as generation context, the 0.90 similarity threshold, and the software implementation. They come from Eval-IA's technical report (AISIDE) and the paper's Methods section.
- **Table F1.** The test files store the item stems but not the text of the response options. The notebook checks the stems and keyed answers of the illustrative items against the data; the options shown in the paper come from the test instrument.
