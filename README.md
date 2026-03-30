# membership-inference-attack-distilbert-agnews

Membership Inference Attack (DistilBERT / AG News)

This repository implements Membership Inference Attacks (MIA) against a provided victim text-classification model (victim_model_distilbert_agnews).
You are given:
	•	A victim DistilBERT model trained on AG News (4-class)
	•	A shadow pool dataset (sampled.csv)
	•	A validation set (validation_samples.csv + validation_results.txt)
	•	A notebook (Membership_Inference_Attack.ipynb) containing all code used for the attack

The goal is to produce member / non-member predictions for the evaluation set in:

mia_lm_results.txt

in the required <id> <member> format.

⸻

1. Project Structure

|-- victim_model_distilbert_agnews/      # Provided Hugging Face model + tokenizer

|-- sampled.csv                          # Shadow pool for optional shadow modeling

|-- validation_samples.csv               

|-- validation_results.txt               

|-- Membership_Inference_Attack.ipynb    # All code (shadow + loss attack)

|-- mia_lm_results.txt                   # Final predictions (loss-threshold attack only)

|__ Latimore_Proj_report.pdf                           # Final report


⸻

2. Important Notes for the Grader

We attempted two attack methods:
	1.	Loss-threshold attack (WORKED, final results use this)
	2.	Shadow-model attack (included but DID NOT perform well)

The final output file mia_lm_results.txt is generated exclusively using the loss-threshold attack, because:
	•	It gave stable and reproducible results
	•	The shadow attack performed poorly and was not used in the final evaluation pipeline

In the notebook, the shadow-model section is left in intentionally

We keep it as documentation of attempted methods, but it should not be used to reproduce our submitted results.

There is a comment in the notebook above the shadow model section saying:

“Shadow model attack included for demonstration; not used for final results. Loss-threshold attack is the method generating mia_lm_results.txt.”

⸻

3. How to Run

Prerequisites

Install dependencies (same environment used in development):

pip install torch transformers numpy pandas scikit-learn tqdm

Any Python 3.9+ environment is fine.

⸻

4. Running the Attack (VS Code Instructions)

You only need to run the notebook:
1.	Open VS Code
2.	Install the Python + Jupyter extensions (if not already)
3.	Open the file:

Membership_Inference_Attack.ipynb


4.	Run the notebook top to bottom
	• It loads the victim model
	• Computes per-sample losses
	• Calibrates the threshold
	• Generates predictions
	• Saves: mia_lm_results.txt

No external scripts or command-line tools are required.
The grader only needs to run the notebook.

⸻

5. Output Format

Your predictions file must be named:

mia_lm_results.txt

Format (no header):

<id> <member>

Where:
	•	<id> = zero-based index in evaluation dataset
	•	<member> = 1 means “member”, 0 means “non-member”

Example:

0 1
1 0
2 0

The notebook saves the results in the correct format.

⸻

6. Methods Summary

Loss Threshold Attack (Final Method)

Uses the victim model’s per-sample loss:
	•	Members typically have lower loss
	•	Non-members have higher loss
	•	A threshold is chosen using the validation set (not used in training)

This produced stable F1 score and is the method used for mia_lm_results.txt.

Shadow Model Attack (Attempted, Not Used)

We attempted to train DistilBERT shadow models using sampled.csv.
However:
	•	The shadow data distribution was too small/noisy
	•	The attack classifier overfit
	•	Final performance was significantly worse than the loss-based attack

Still included in the notebook for completeness.

⸻

7. Reproducibility

If the grader runs the notebook in VS Code:

✔ The loss attack will reproduce the results
✔ The output file will match the expected format
✔ No additional paths, flags, or scripts are needed

Shadow-model attack may be run but is not required for grading.

⸻

8. Results (Reproducibility Summary)

The notebook prints a summary after running the loss-threshold attack.
These values confirm the attack executed correctly:

Device: cpu
Validation size: 1000
Member counts: {0: 808, 1: 192}

Loss statistics (validation):
  min:    0.017
  25%:    0.028
  median: 0.048
  75%:    0.116
  max:    5.162

Threshold selection:
  Selected τ (loss threshold): 0.17167251400157224
  Rule used: lower loss => member

Validation performance (thresholded):
  TP: 166   FP: 643   TN: 165   FN: 26
  Precision: 0.205
  Recall:    0.865
  F1:        0.332
  ROC-AUC:   0.521

Evaluation set:
  Size: 10000 samples

Output saved to:
  mia_lm_results.txt

Important Note

These results correspond to the loss-threshold attack only, which is the method used to generate the final submission file.
The shadow-model attack is included only as an attempted approach and is not part of the final results.
