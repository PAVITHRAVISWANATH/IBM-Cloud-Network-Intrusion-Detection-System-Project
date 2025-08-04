# IBM-Cloud-Network-Intrusion-Detection-System-Project
# Network intrusion detection system using machine learning. The system is capable of analyzing network traffic data to identify and classify various types of cyber-attacks (e.g., DoS) and distinguish them from normal network activity. 

Goal: Use AutoAI to train a model that classifies network traffic (e.g., DoS, Probe, R2L, U2R, Normal) using the Kaggle intrusion detection dataset, then deploy it for predictions—all without writing code.

# Prerequisites
IBM Cloud Lite account (free) — sign in at https://cloud.ibm.com
Watson Studio service created (Lite plan)
Watson Machine Learning included via the Watson Studio project
Dataset downloaded from Kaggle: https://www.kaggle.com/datasets/sampadab17/networkintrusion-detection (e.g., KDDTrain+.csv)

# Step-by-Step No-Code AutoAI Workflow
1. Set Up Project in Watson Studio
Log into IBM Cloud.
Navigate to Watson Studio from the catalog and create a new project.
Choose "Create project from scratch".
Attach Cloud Object Storage (Lite) as the project’s storage.

2. Upload the Dataset
In your Watson Studio project, go to Assets → + Add to project → Data asset.
Upload the KDDTrain+.csv file (or the relevant intrusion detection file).
Once uploaded, you can click it to preview data in the UI.

3. Prepare the Data via the UI
AutoAI can handle many preprocessing steps automatically, but you can improve quality:
Use the data preview to:
Identify missing values (AutoAI can impute them).
Rename or map the label column if needed (ensure the attack/normal label is clearly defined).
If desired, create a simplified label (e.g., group specific attacks into categories like DoS, Probe, etc.) prior to upload using Excel/Google Sheets (still no code).
Check for class imbalance in the target column; AutoAI will apply appropriate handling, but documenting imbalance (e.g., many normals vs few U2R) is good.

4. Launch AutoAI Experiment
In Assets → + Add to project → AutoAI experiment, create a new AutoAI experiment.
Select the uploaded dataset as the source.
Choose the target column — this is the label that indicates attack type (e.g., label or protocol_type depending on your framing).
Specify the prediction type:
Multiclass classification if you want to distinguish DoS, Probe, R2L, U2R, Normal separately.

5. Configure and Run
Accept default settings or:
Choose Optimization Metric (e.g., Accuracy, F1-weighted) depending on priority—F1 is better if classes are imbalanced.
Enable Cross-validation (AutoAI does this automatically for robustness).
Click Run experiment.
Wait for AutoAI to finish: it will automatically do preprocessing, feature engineering, algorithm selection, hyperparameter optimization, and pipeline generation.

6. Review Leaderboard & Pipelines
After completion, AutoAI presents a Leaderboard of pipelines sorted by your chosen metric.
Examine key metrics shown:
Accuracy, Precision, Recall, F1-score (macro/micro/weighted)
Confusion matrix and Log loss in the evaluation view
Use the Pipeline Comparison tab to visually compare models across metrics.
Select the best-performing pipeline (e.g., highest F1 for attack classes or balanced accuracy).

7. Save and Deploy the Model
Click “Save as model” for the chosen pipeline.
Navigate to the Models/Deployments section in the project.
Create an Online Deployment (REST endpoint) using Watson Machine Learning.
Name the deployment.
Deploy it; you’ll get a scoring URL and credentials (API key/token).

8. Test Predictions via UI
In the deployment view, go to the Test tab.
Manually input sample records (matching the feature schema) or upload a small test set.
Observe predicted classes and confidence scores.
Validate that known attack samples are correctly classified.

9. Interpret Metrics for Correctness
High Recall on Attack Classes: Ensures intrusions aren’t missed.
Precision: Low false alarms.
F1-score: Balance between precision and recall (especially important if class distribution is skewed).
Confusion Matrix: See which attack types get confused, if any.
Confidence Scores: Gives trust in individual predictions.


# Deployment Use Cases
Feed real-time network flow summaries into the API for live intrusion detection.
Trigger alerts when the model labels traffic as an attack type with high confidence.
Log predictions for forensic analysis.
