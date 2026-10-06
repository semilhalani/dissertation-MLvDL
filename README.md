# Network Intrusion Detection: A Comparative Study of ML and DL Approaches
MSc dissertation project comparing machine learning and deep learning approaches for network intrusion detection systems (NIDS).
## Summary
This project studies machine learning and deep learning approaches for building a network intrusion detection system (IDS), a system that monitors network traffic and flags anomalies that may indicate an attack. It uses the **CSE-CIC-IDS 2018** dataset, a large-scale, recent benchmark dataset built jointly by the Communications Security Establishment (CSE) and the Canadian Institute for Cybersecurity (CIC). This dataset was chosen specifically because most prior IDS research relies on older, smaller datasets such as NSL-KDD and KDD Cup '99, which don't reflect modern traffic patterns.
This project evaluates a set of classical machine learning classifiers against a deep learning model, a feed forward neural network, on this dataset to determine which approach offers the best trade-off between detection accuracy and false-positive rate.
## Tech stack
- Python
- Keras / TensorFlow (deep learning models)
- scikit-learn (classical ML models + evaluation metrics)
- pandas / NumPy (data processing)
- Jupyter Notebook
## Methodology
1. Preprocessing and feature engineering: merged the 10 daily CSV files from the CSE-CIC-IDS 2018 dataset, dropped null and duplicate rows along with malformed rows where a few records had attribute values equal to the column headers, removed the `Timestamp` column, and dropped 4 extra identifier columns (`Flow ID`, `Src IP`, `Src Port`, `Dst IP`) from the 4th day's file so all 10 dataframes had matching schemas. Records with `NaN`, infinite, or out-of-float64-range values were also dropped. Class labels were binarised into `Benign` vs. `Harmful`, and the Benign class was downsampled to match the number of harmful records to avoid class-imbalance bias. Labels were then numerically encoded, and features were reduced from 80 to 48 by dropping any feature pair with correlation of 0.9 or higher. Remaining features were scaled with `MinMaxScaler`, and the dataset was split 60/20/20 into training, validation, and test sets.
2. Classical ML models evaluated: Logistic Regression, Decision Tree Classifier, and Support Vector Classifier with a linear kernel.
3. Deep learning architecture evaluated: a Feed Forward Neural Network (FFNN) with 3 hidden Dense layers of 128, 64, and 32 nodes, all using ReLU activations, each followed by a Dropout layer with a rate of 0.2, and a softmax output layer. It was trained with the Adam optimiser and sparse categorical cross-entropy loss for 5 epochs.
4. Evaluated across multiple metrics: accuracy, F1 score, and confusion matrix values (true positives, false positives, false negatives, true negatives).
## Key findings
- The FFNN achieved the strongest overall result: 96.64% accuracy and an F1 score of 0.97, with the fewest false positives of any model tested at 1,498.
- The Decision Tree Classifier came very close, with 96.06% accuracy and an F1 score of 0.96, and a much lower false-positive count than Logistic Regression at 14,644.
- The linear-kernel SVM matched the Decision Tree's accuracy of 96.06% with an F1 score of 0.9579.
- Logistic Regression had both the lowest accuracy at 93.13% and the highest false-positive count at 33,054 of the four models, making it the least reliable at avoiding false alarms.
- An earlier experiment training on a single day's data and testing on the other nine failed badly, with test accuracy ranging from 30% to 80%, because each day's file covered different attack types. This is what led to the final approach of merging and class-balancing all 10 days into a single dataset before training, which produced the results above.
## Repo contents
- `MSc Project Code - A Comparative Study on Machine Learning and Deep Learning Approaches for Cybersecurity Intrusion Detection System.ipynb`: full implementation and experiments
- `Dissertation Research Paper (MSc Project) - A Comparative Study for Machine Learning and Deep Learning Approaches for Cybersecurity Intrusion Detection System.pdf`: full written dissertation with literature review and detailed methodology
- `Reflective Essay (MSc Project) - A Comparative Study on Machine Learning and Deep Learning Approaches for Cybersecurity Intrusion Detection System.pdf`: reflective essay on design choices and limitations
> Note: this repo contains the dissertation code and research paper as submitted for an MSc. It's a research comparison, not a deployable system.
## How to explore
```bash
git clone https://github.com/semilhalani/dissertation-MLvDL.git
cd dissertation-MLvDL
pip install -r requirements.txt
jupyter notebook
```
For the full write-up (literature review, detailed methodology, discussion of results), see the dissertation paper (PDF) included in this repo.
