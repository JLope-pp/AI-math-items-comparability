# Data Codebook

This codebook describes the columns and codes of the anonymized files that are not self-explanatory.

## Files

- questionnaire_anonymized.csv: student questionnaire (background and sex), one row per student.
- test_1_anonymized.csv, test_2_anonymized.csv, test_3_anonymized.csv: responses to the three test forms, one row per student.
- item_bank_expert_review.xlsx (sheet consolidated): the item bank and the specialist and teacher review of the 120 AI-generated items.

## Shared identifiers

- student_id: anonymized student code (e.g. EST0001), consistent across all four student-level files.
- school_id: anonymized school code (Colegio_1...Colegio_6), one per real school in the pilot. Real school names are not included.

## questionnaire_anonymized.csv

| Column | Meaning | Values |
|---|---|---|
| sex | Student sex | Female, Male |
| p1 | How old are you? | age in years |
| p2 | Do you live in the same district as your school? | YES, NO, DON'T KNOW |
| p3 | First language(s) learned in the first five years of life | numeric code |
| p41 | Did you repeat any grade/year in PRIMARY school? | DID NOT REPEAT, FIRST, SECOND, THIRD, FOURTH, FIFTH, SIXTH, or a combination |
| p42 | Did you repeat any grade/year in SECONDARY school? | same scale as p41 |
| p5 | Highest level of education completed by mother/main female caregiver | NO FORMAL EDUCATION, INCOMPLETE PRIMARY, COMPLETE PRIMARY, INCOMPLETE SECONDARY, COMPLETE SECONDARY, TECHNICAL HIGHER EDUCATION, UNIVERSITY EDUCATION, DON'T KNOW |
| p6 | Same as p5, for father/main male caregiver | same scale |
| p7 | At home you have... (multi-select, raw encoding) | a number whose digits 1-7 indicate which items are checked: 1=own study space, 2=computer/laptop, 3=tablet, 4=mobile internet, 5=home internet, 6=desk/study table, 7=quiet place to study. Example: 1457 means options 1, 4, 5, 7 are checked. |
| p81 | How often do you have mobile internet access? | NEVER, SOMETIMES, FREQUENTLY, or -777/-888 (not answered / double-marked) |
| p82 | How often do you have home internet access (not mobile)? | same scale as p81 |
| p9 | Main device used to connect to the internet | MOBILE PHONE, COMPUTER/LAPTOP, TABLET |
| p10a-p10h | Attention / self-regulation while studying (8 statements) | DISAGREE, PARTIALLY AGREE, AGREE |
| p11a-p11d | Beliefs about intelligence (growth mindset, 4 statements) | same 3-point scale |
| p12a-p12h | Perceived characteristics of the test questions (8 statements) | NO QUESTIONS, VERY FEW QUESTIONS, MOST QUESTIONS, ALL QUESTIONS, or -777 (not answered) |
| p13a-p13h | Perceived usability of the testing app (8 statements) | same 3-point scale as p10 |

The question codes (p1 to p13h) are the ones used in the original instrument. The wording of each statement comes from the instrument's variable labels, which are not included here because the same file documents personal data.

## test_1/2/3_anonymized.csv

| Column pattern | Meaning |
|---|---|
| grade | Student's grade: 1st grade...4th grade |
| item_correct_{N} | 1 = correct, 0 = incorrect, for item N (N = the instrument's global item number, e.g. 227-316) |
| option_selected_{N} | Which option (a/b/c/d) the student selected for item N |
| question_text_{N} | The full text of math item N |

The item stems in question_text_{N} were translated from Spanish, the language of the pilot, into English. Numbers, operations, units and quantities were kept exactly as in the original. Table D1 in the notebook is computed on this English text, so its similarity values differ slightly from those in the paper, which used the Spanish text.

## item_bank_expert_review.xlsx (sheet consolidated)

| Column | Meaning | Values |
|---|---|---|
| item_id | Item identifier | integer |
| item_type_code | Item type code from the original file | O, C (meaning not documented) |
| reviewer_id | Numeric code for the teacher who reviewed the item | 1-6 (not a name) |
| origin | Where the item came from | Item bank, AI-generated |
| model_or_source | For item-bank rows, the original source batch/assessment; for AI-generated rows, the language model | Batch 1 - Basic operations, Batch 2 - Math questions, Batch 3 - Math word problems, TIMSS, ENLA, Desafia-T, NdM, gemini-2.5-pro, qwen3-max, deepseek-chat, gpt-5-nano |
| content_alignment_teacher / content_alignment_expert | Content-alignment rating (1-3 scale, teacher / specialist) | 1-3 |
| distractor_quality_teacher / distractor_quality_expert | Distractor-quality rating (1-3 scale) | 1-3 |
| difficulty_level, item_grade_level, item_classroom_group | Classification codes from the original file | numeric (categories not documented) |
| cognitive_demand_teachers / cognitive_demand_expert / intended_cognitive_demand | Cognitive demand as rated by the teachers, by the specialist, and as intended at generation (recorded on the review form) | 1 = low, 2 = medium, 3 = high |
| content_topic_teachers / content_topic | Content-topic code | 1-7 (no text labels available) |
| expert_answer_correct | Whether the specialist judged the keyed answer correct | correct, incorrect |
| distractor_1_rationale, distractor_2_rationale, distractor_3_rationale | Reviewer's explanation of the error each incorrect option represents | free text |
| teacher_comments, expert_comments | Review comments | free text |

Each language model generated 30 items with consecutive item_id values, in three blocks of 10 (high, medium and low demand). The notebook takes the intended demand of each item from its block. Items 229, 256 and 260 have a different code in intended_cognitive_demand on at least one review form.
