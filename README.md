# Network Intrusion Detection: A Comparative Study of ML and DL Approaches
MSc dissertation project comparing machine learning and deep learning approaches for network intrusion detection systems (NIDS).
## Summary
This project studies machine learning and deep learning approaches for building a network intrusion detection system (IDS) — a system that monitors network traffic and flags anomalies that may indicate an attack. It uses the **CSE-CIC-IDS 2018** dataset, a large-scale, recent benchmark dataset built jointly by the Communications Security Establishment (CSE) and the Canadian Institute for Cybersecurity (CIC), specifically because most prior IDS research relies on older, smaller datasets (NSL-KDD, KDD Cup '99, etc.) that don't reflect modern traffic patterns.
This project evaluates a set of classical machine learning classifiers against deep learning architectures on this dataset to determine which approach offers the best trade-off between detection accuracy and false-positive rate.
## Tech stack
- Python
- Keras / TensorFlow (deep learning models)
- scikit-learn (classical ML models + evaluation metrics)
- pandas / NumPy (data processing)
- Jupyter Notebook
## Methodology
1. Preprocessing and feature engineering: merged the 10 daily CSV files from the CSE-CIC-IDS 2018 dataset, dropped null/duplicate rows and malformed rows (a few records had attribute values equal to the column headers), removed the `Timestamp` column, and dropped 4 extra identifier columns (`Flow ID`, `Src IP`, `Src Port`, `Dst IP`) from the 4th day's file so all 10 dataframes had matching schemas. Records with `NaN`, infinite, or out-of-float64-range values were dropped. Class labels were binarised into `Benign` vs. `Harmful`, and the Benign class was downsampled to match the number of harmful records to avoid class-imbalance bias. Labels were then numerically encoded, and features were reduced from 80 to 48 by dropping any feature pair with correlation ≥ 0.9. Remaining features were scaled with `MinMaxScaler`, and the dataset was split 60/20/20 into training, validation, and test sets.
2. Classical ML models evaluated: Logistic Regression, Decision Tree Classifier, Support Vector Classifier (linear kernel)
3. Deep learning architectures evaluated: Feed Forward Neural Network (FFNN) — 3 hidden Dense layers (128, 64, 32 nodes, ReLU activations) each followed by a Dropout layer (rate 0.2), with a softmax output layer, trained with the Adam optimiser and sparse categorical cross-entropy loss for 5 epochs
4. Evaluated across multiple metrics: accuracy, F1 score, and confusion matrix (true positives, false positives, false negatives, true negatives)
## Key findings
- The FFNN achieved the strongest overall result: 96.64% accuracy and an F1 score of 0.97, with the fewest false positives (1,498) of any model tested.
- The Decision Tree Classifier came very close at 96.06% accuracy and an F1 score of 0.96, with a substantially lower false-positive count (14,644) than Logistic Regression.
- The linear-kernel SVM matched the Decision Tree's accuracy (96.06%) with an F1 score of 0.9579.
- Logistic Regression had both the lowest accuracy (93.13%) and the highest false-positive count (33,054) of the four models, making it the least reliable at avoiding false alarms.
- An earlier experiment — training on a single day's data and testing on the other nine — failed badly (30–80% test accuracy), because each day's file covered different attack types. This motivated the final approach: merging and class-balancing all 10 days into a single dataset before training, which produced the results above.
## Repo contents
- `MSc Project Code - A Comparative Study on Machine Learning and Deep Learning Approaches for Cybersecurity Intrusion Detection System.ipynb` — full implementation and experiments
- `Dissertation Research Paper (MSc Project) - A Comparative Study for Machine Learning and Deep Learning Approaches for Cybersecurity Intrusion Detection System.docx` — full written dissertation with literature review and detailed methodology
> Note: this repo contains the dissertation code and research paper as submitted for an MSc — it's a research comparison, not a deployable system.
## How to explore
\`\`\`bash
git clone https://github.com/semilhalani/dissertation-MLvDL.git
cd dissertation-MLvDL
pip install -r requirements.txt
jupyter notebook
\`\`\`
For the full write-up (literature review, detailed methodology, discussion of results), see the dissertation document included in this repo.
