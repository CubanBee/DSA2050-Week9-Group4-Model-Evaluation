# DSA 2050A — Week 9: Model selection and defensible evaluation

**Group name/number:** GROUP 4 
**Members and student IDs:** Cindy Mamang-Kanga - 675031, Valentine Wachira - 672112, Hassan Kabocha - 670212
**Submission:** One shared GitHub repository link per group  
**Course:** DSA 2050A Data Science Methodology  
**Week:** 9

## Group responsibilities
Complete the lab **in your existing groups**. Work together on one notebook and one repository. Record each member's concrete contribution in the table below. Each member should contribute visibly through Git commits or documented co-authored work.

| Group member | Student ID | Work completed | GitHub username or commit reference |
|---|---|---|---|
| [Cindy Mamang-Kanga] | [675031] | [Data inspection, Subgroup & permutation analysis, Rationale freeze, final test] | [Cindy-mamang] |
| [Valentine Wachira] | [672112] | [Leakage checks, README, Preprocessing pipeline, GitHub & QA] | [CubanBee] |
| [Hassan Kabocha] | [670212] | [5-fold CV, grid search, Confusion matriX] | [k-a-b-o-c-h-a]

## Study objective
Explain the customer churn decision and why model selection must use development data only.
The provider wants to flag customers who are likely to churn so that retention efforts can focus on accounts that are at risk. Model selection must use only 'churn_development.csv' so that 'churn_final_test.csv' stays untouched for an unbiased scanning.

## Data statement
Synthetic fictional customer data created for teaching. No real customer records.

## Setup and reproduction
1. Open this folder in VS Code.
2. Create/select a Python environment (3.10–3.13 recommended).
3. In **Terminal → New Terminal**, run `python -m pip install -r requirements.txt`.
4. Open `notebooks/Week9_Model_Selection_Lab.ipynb` and **Select Kernel**.
5. Run each cell in order. Complete the rationale before the final-test cell.

## Prediction-time features and excluded fields
- Target: `churn` (1 = left; 0 = stayed).
- Excluded: `customer_id` (identifier), `cancellation_confirmed` (post-outcome leakage).
- Allowed features: [Customer attributes available before cancellation happens—numeric fields and categorical fields, preprocessed inside a pipeline so missing values and scaling are fit only within each training fold]

## Experiments and model selection
- Primary metric chosen before comparison: **F1**.
- CV: **5-fold stratified, shuffle=True, seed=42**.
- Include the candidate comparison table from `reports/experiment_log.csv`.
- **Chosen model:** [Logistic Regression]
- **Tuned hyperparameters:** [C = 1]
- **Mean/SD CV F1:** [Mean F1 = 0.470, SD = 0.053]
- **Why this choice:** [We select Logistic Regression over Decision Tree because it achieves a higher 5-fold stratified CV mean F1 of 0.470 compared to the best Decision Tree at 0.399, while also improving mean precision and mean recall. Its 5-fold F1 scores are consistent, and it beats the majority-class baseline of 0.000. Out-of-fold diagnostics show 566 false negatives and 205 false positives, with weaker recall for Established customers than New customers, which is a smaller group. Logistic Regression with C = 1 keeps the model simple, regularised, and easy to interpret without overfitting the pipeline]

## Error and subgroup analysis
- Summarize confusion matrix, false negatives and false positives.
Out-of-fold predictions on development data give 566 false negatives and 205 false positives.
- Compare new versus established customer recall **with subgroup sample sizes**.
New customers (sample size = 452, 261 actual churners) have recall of 0.782, whereas Established customers (sample size = 2,268, 649 actual churners) have much lower recall of 0.216. Treat the New subgroup score cautiously because its sample size is smaller.
- Explain business implications.
Missing 566 churners means that many customers that are at risk to get no retention outreach, while 205 false positives waste outreach effort on customers who would have stayed anyway.

## Permutation importance
- Top two features and approximate importance values.
tenure_months (0.079 mean F1 drop) and support_tickets_90d (0.035 mean F1 drop).
- Explain why association is not causation.
High importance means shuffling the feature hurts predictions because the model relies on that pattern but does not prove that changing the feature directly causes or stops churn.

## Final test (run once only)
- F1: [0.449] ; precision: [0.585] ; recall: [0.364]
- Compare CV vs test and discuss whether the difference is concerning.
Final-test F1 of 0.449 is slightly lower than CV mean F1 of 0.470 by 0.021. That small difference is not alarming for generalisation, though overall recall, 0.364 on test, remains modest and worth improving in future iterations.

## Limitations and ethics
- Provide at least two realistic limitations (dataset is synthetic; hidden customer dynamics; temporal drift, etc.).
1. The dataset is synthetic and may not reflect real customer behaviour.
2. Missing values in key fields and low recall on Established customers mean real-world misses could be uneven across groups.
- Note monitoring / human review needs before real use.
Monitor data drift, missingness, and false negatives over time, and keep human review before acting on churn flags.

## Evidence checklist
- [ ] Executed notebook with explanations
- [ ] `reports/experiment_log.csv`
- [ ] `reports/subgroup_metrics.csv`
- [ ] `reports/permutation_importance.csv`
- [ ] `reports/frozen_selection.json` created before test
- [ ] `reports/final_metrics.json` generated once
- [ ] Two generated PNG charts
- [ ] Meaningful commits or co-authorship evidence for all group members
- [ ] Completed group contribution table with names and student IDs

## Final group review
- [ ] One team member verified the notebook runs from start to finish
- [ ] Group discussed and approved the written model-selection rationale
- [ ] No one used final-test scores to revise the model
- [ ] One representative will submit the shared URL to the course LMS

## GitHub repository URL
[https://github.com/CubanBee/DSA2050-Week9-Group4-Model-Evaluation.git]
