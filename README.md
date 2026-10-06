# Scaling Standardized Mathematics Item Generation with Generative AI

Fernando Alarcon, Juan Leon, Pablo Zoido, Magdalena Barros and Jorge Lopez

This repository contains the anonymized data and the code needed to reproduce the results of the paper. The study generated 120 mathematics items with four language models (GPT-5-Nano, Gemini-2.5-Pro, DeepSeek-Chat and Qwen3-Max), reviewed all of them, and administered 45 of them together with 45 items from an established item bank to 209 students in six public schools in Metropolitan Lima, Peru, in February 2026.

## Repository structure

```text
├── data/
│   ├── questionnaire_anonymized.csv
│   ├── test_1_anonymized.csv
│   ├── test_2_anonymized.csv
│   ├── test_3_anonymized.csv
│   ├── item_bank_expert_review.xlsx
│   └── CODEBOOK.md
└── notebook/
    ├── paper_replication.ipynb
    └── outputs/
        ├── tables/     Tables of the paper (CSV) and paper_tables.xlsx
        └── figures/    Figures 1 to 4 (PNG)
```

## Data

| File | Content |
|---|---|
| questionnaire_anonymized.csv | Student questionnaire: sex and background questions |
| test_1_anonymized.csv, test_2_anonymized.csv, test_3_anonymized.csv | Responses to the three test forms. Each form has 30 items, 15 from the item bank and 15 generated with AI |
| item_bank_expert_review.xlsx | Item bank and review of the 120 AI-generated items by a specialist and six teachers |
| CODEBOOK.md | Description of every column and code |

Students are identified by an anonymous code (EST0001, EST0002, ...) that is the same in all files, and schools by Colegio_1 to Colegio_6. The files contain no names, birthdates or ID numbers. Reviewers are identified by a number from 1 to 6.

The pilot was administered in Spanish. The item stems and text fields were translated into English for this repository, keeping every number, operation and unit as in the original.

## What the notebook reproduces

The notebook follows the paper section by section and uses the same numbering.

| Paper section | Results |
|---|---|
| 5.1 Technical quality of the AI-generated items | Table 1 and the teacher review |
| 5.2 Main empirical comparison | Figure 1 |
| 5.3 Probit regression | Table 2 |
| 5.4 Differences within each demand level (RQ1) | Table 3 |
| 5.5 Variation of the difference across levels (RQ2) | Table 4 |
| 5.6 Differences across levels within AI-generated items (RQ3) | Figure 2 and robustness checks |
| 5.7 Secondary psychometric analyses | Tables 5 to 8, Figures 3 and 4 |
| Appendices A to F | Tables A1, B1, C1, D1, E1, E2 and F1 |

The results are already included in notebook/outputs, so they can be consulted without running anything. Each table is saved as a CSV file with the same columns and formatting as in the paper (for example, table_3_predicted_probabilities_rq1.csv), and paper_tables.xlsx contains all of them, one sheet per table. The folder also has item-level files used to build the tables, such as the distractor analysis by item and the IRT parameters. Running the notebook again overwrites these files with identical results.

The main model is a probit regression of correct responses on item origin, intended cognitive demand and their interaction, controlling for the student's score on the other bank items, with standard errors clustered by student. In Stata it corresponds to:

```stata
probit correct c.RestBankScore i.AI##i.demand, vce(cluster student_id)
margins AI#demand
```

## How to run it

1. Install the Python packages:

   ```bash
   pip install pandas numpy matplotlib scipy statsmodels openpyxl factor_analyzer
   pip install sentence-transformers bert-score marginaleffects
   ```

2. Open the notebook and run all cells:

   ```bash
   jupyter notebook notebook/paper_replication.ipynb
   ```

The notebook takes about a minute. The first run takes longer because Table D1 downloads two multilingual language models.

## Notes

Each language model generated 30 items with consecutive identifiers, in three blocks of 10 (high, medium and low demand). The notebook takes the intended demand of each item from its block, which gives 40 items per level in Tables 1 and C1.

Table D1 is computed on the English stems, while the paper used the original Spanish text, so its similarity values differ slightly from the published ones.

Three rows of Table A1 describe settings of the Eval-IA generation tool that are not stored in the data: the 20 reference items given to the models as context, the 0.90 similarity threshold and the software used. The test files do not include the text of the response options, so the options shown in Table F1 come from the test instrument.

## Citation

Alarcon, F., Leon, J., Zoido, P., Barros, M., & Lopez, J. (2026). *Scaling standardized mathematics item generation with generative AI*.
