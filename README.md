# Phishing-Website-Detection-Using-Machine-Learning

## Team Members
Sambisha Godi, Adam Schuler, Anton Falk, Pin-Chen Kuo, Sarah Dsouza

## Overview
This project uses the PhiUSIIL Phishing URL Dataset (Prasad & Chandra, 2024), publicly available through the UCI Machine Learning Repository. The dataset was developed as part of the PhiUSIIL framework, which aims to establish a benchmark dataset for phishing detection research. The data was collected using automated web crawlers and feature extraction pipelines that analyzed URL structure, domain information, and webpage HTML content in 2024.

The original dataset was collected from Open PageRank Initiative Anon for legitimate URLs, and PhishTank, OpenPhish, and MalwareWorld for phishing URLs by downloading the webpages.

- Downloading phishing webpage: Because phishing sites are quickly detected and taken down, the authors continuously track feeds from anti-phishing platforms to obtain the latest phishing URLs. Each URL is then programmatically downloaded in a secure environment and stored locally as a text file for later processing.

- Downloading Legitimate websites : The authors continuously monitored anti-phishing sites and downloaded over 100000 phishing webpages from October 2022 to May 2023. The above-discussed approach is used to download over 100000 legitimate webpages used to construct the dataset.


Our dataset contains 235,795 total samples, consisting of:

*  134,850 legitimate URLs
*  100,945 phishing URLs

Each URL is processed into structured features capturing lexical characteristics, domain behavior, website metadata, and HTML content patterns. The dataset supports supervised machine learning and cybersecurity research and serves as a modern, high-quality benchmark for phishing detection.

Dataset source link: https://archive.ics.uci.edu/dataset/967/phiusiil+phishing+url+dataset

## EDA
### Types of threats
![Test Image 7](https://github.com/SarahJDsouza/Phishing-Website-Detection-Using-Machine-Learning/blob/main/threat.png)
### Top Phishing TLDs
![Alt text](https://github.com/SarahJDsouza/Phishing-Website-Detection-Using-Machine-Learning/blob/0e6bf65cd96d9dc4869155798bcaedc016555bcf/tld.png)
### Feature Selection
![Alt text](https://github.com/SarahJDsouza/Phishing-Website-Detection-Using-Machine-Learning/blob/0e6bf65cd96d9dc4869155798bcaedc016555bcf/top15.png)

## Model Comparison Table (After Threshold Tuning)

| Model              | Accuracy | ROC AUC | Precision | Recall | F1-Score | Balanced Accuracy | Best Threshold | Total Misclassification Cost | Notes / Insights |
|--------------------|----------|---------|-----------|--------|----------|-------------------|----------------|-----------------------------|------------------|
| Logistic Regression| 0.9990   | 1.0000  | 0.999     | 0.999  | 0.999    | 0.9986            | 0.30           | 76                          | Excellent baseline; very low FN; threshold tuning reduces cost. |
| Naive Bayes        | 0.9903   | 0.9975  | 0.987     | 0.999  | 0.993    | 0.9986            | 0.95           | 627                         | Good probabilistic baseline; slightly lower precision for legitimate URLs. |
| Decision Tree      | 0.9987   | 0.9987  | 0.999     | 0.999  | 0.999    | 0.9985            | 0.10           | 250                         | High accuracy; interpretable; threshold tuning reduces FN. |
| SVM (Linear)       | 0.9980   | 0.9982  | 0.997     | 0.999  | 0.998    | 0.9986            | -0.25          | 107                         | Strong separation; very low misclassification; threshold tuning slightly reduces FN. |
| Random Forest      | 0.9993   | 0.999998| 0.999     | 0.999  | 0.999    | 0.9987            | 0.35           | 48                          | Ensemble captures complex patterns; highest overall performance; cost-sensitive ready. |
| Ensemble (Voting)  | 0.9998   | 1.0000  | 0.999     | 0.999  | 0.999    | 0.9990            | 0.55           | 18                          | Best overall model; combines strengths of individual models; extremely low misclassification cost. |



### Key Learnings and Outcomes

- **Understanding Phishing Behavior**
  - Scammers manipulate **URL structures** with long domains, raw IP addresses, and multiple subdomains.
  - **Visual deception** tactics such as homograph attacks, obfuscation with special characters, and repeated patterns drive high similarity scores.
  - **Content-based tricks** include hidden fields, external form submissions, and password inputs placed in unusual contexts.
  - **Keyword baits** like “Bank,” “Pay,” and “Crypto” create urgency and lure users into disclosing sensitive information.However, it not an essential predictor.

- **Feature Selection and Importance**
  - Statistical tests (KS Statistic, Mutual Information) identified the most discriminative features, including `URLSimilarityIndex`, `NoOfExternalRef`, and `LineOfCode`.
  - Grouping features into behavioral categories (Trust Signals, External Resource Risk, Credential Capture, Obfuscation/Stealth) revealed which tactics carry the strongest predictive power.
  - Feature importance analysis confirmed that structural and behavioral URL traits are the backbone of phishing detection.

- **Model Evaluation**
  - Multiple models were trained: Logistic Regression, Naive Bayes, Decision Tree, SVM, Random Forest, and an Ensemble Voting Classifier.
  - **Random Forest** achieved the highest ROC-AUC among individual models, while the **Ensemble Classifier** delivered superior accuracy and minimized misclassification cost.
  - Threshold tuning demonstrated the importance of cost-sensitive evaluation, especially in reducing false negatives.

- **Data Quality and Accuracy Investigation**
  - The unusually high accuracy prompted deeper validation: checks for data leakage, correlation bias, and label distribution.
  - Dataset quality was confirmed, with sources like PhishTank, OpenPhish, and MalwareWorld providing reliable, well-structured data.
  - This reinforced the lesson that **high accuracy is not always a win — it is a signal to investigate deeper** and understand the provenance of the dataset.

- **Broader Implications**
  - URL-based detection models can serve as lightweight, scalable defenses against phishing.
  - The project highlights the importance of combining **technical rigor** with **critical validation** to ensure trustworthy results.
  - Future directions include integrating the model into browser extensions, real-time monitoring systems, or combining supervised learning with unsupervised clustering to detect novel phishing patterns.

