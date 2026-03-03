# **Discovering and Predicting Research Topics in Computer Science (DBLP Dataset)**

## **Author**
* Duyen Thanh Thao Nguyen (myUH ID: 2329157)
* * An Tran (myUH ID: 2261820)

## **Overview**

This project analyzes millions of research papers from the DBLP dataset to uncover underlying research topics, identify long-term trends in computer science, and predict the venue of new papers. By combining unsupervised learning (clustering, dimensionality reduction, anomaly detection) with supervised learning (classification), the project provides a large-scale view of how computer science research evolves over time.

---

## **Project Goals**
* Discover **topic structures** across millions of DBLP papers
* Predict the **topic or venue** of new papers
* Identify **emerging research trends**
* Detect **anomalous or niche papers**
* Explore **patterns in publication venues, citations, and collaborations**

---

## **Dataset Attributes**

* **Title** – for keyword extraction and TF-IDF
* **Abstract** – main textual source for feature modeling
* **Authors** – used for productivity and collaboration statistics
* **Venue** – used as labels for supervised classification
* **Year** – supports trend and temporal research analysis
* **Citation Count** – identifies high-impact papers
* **References** – enables network analysis
* **Unique ID** – ensures stable linking across records

---

## **Workflow Overview**

### **1. Preprocessing**
* Load data in 500-row chunks to reduce memory usage
* Remove incomplete entries (missing title/abstract)
* Deduplicate using *title + abstract*
* Apply text normalization: lowercasing, whitespace cleanup, removal of non-alphabetic characters
* Generate TF-IDF representations (5,000 features, no stopwords, unigrams)

### **Dimensionality Reduction**
* Apply **Truncated SVD** → 50-dimensional semantic embeddings
* Use PCA/t-SNE for visualization


## **2. Exploratory Data Analysis**
Includes both numerical summaries and visualizations:
* Papers per **year**, **venue**, and **author**
* Citation statistics over time
* Keyword frequency trends
* PCA and t-SNE visualizations of paper embeddings

### **Trend Detection**
* Built yearly keyword frequency matrix
* Applied linear regression to detect rising research topics
* Found strong growth in:
  * *Machine learning*
  * *Data science*
  * *Neural networks*
  * *Algorithms for large datasets*


## **3. Clustering Analysis**
Aimed to group papers into meaningful research topics.
### **K-Means (k = 5, 10, 15)**
* Chose **k = 10** based on inertia and elbow method
* Identified 10 major topic areas, including:
  * Networking
  * Data Mining
  * ML/Modeling
  * Robotics
  * Computer Vision
  * Algorithms
  * Software Engineering
  * Signal Processing
  * Education Technology
  * Fuzzy Systems

### **Agglomerative Clustering**
* Average linkage performed best
* Higher Silhouette score but still overlapping clusters

### **DBSCAN**
* Best results at eps = 0.15
* One major cluster + multiple small dense groups + noise
* Useful for discovering niche topic pockets


## **4. Supervised Classification**
Evaluated multiple models for predicting **venue** from embeddings:

| Model                   | Accuracy   | Macro-F1   |
| ----------------------- | ---------- | ---------- |
| Logistic Regression     | 0.9966     | 0.9942     |
| SVM                     | 0.9508     | 0.9547     |
| Random Forest           | 0.9470     | 0.9481     |
| LightGBM                | 0.9686     | 0.9676     |

**Logistic Regression** was the top performer, achieving ~99.7% accuracy.
The confusion matrix shows extremely few mistakes, confirming that SVD features capture strong venue-specific patterns.


## **5. Anomaly Detection**
Used One-Class SVM (nu = 0.05, RBF) on a sampled subset (5,000 papers):
* Identified 255 anomalous papers
* Validated by inspecting top keywords, title, and venue
* Visual confirmation via 2D scatter plot
* Outliers represent niche, unusual, or cross-disciplinary topics

---

## **Conclusion**
The project successfully achieved its goals by:
* Revealing clear research topic structures across DBLP
* Showing long-term topic evolution, especially the rise of ML and data-driven research
* Predicting publication venues with extremely high accuracy
* Highlighting niche or unusual papers using anomaly detection
* Demonstrating that large-scale text embeddings and machine learning can meaningfully organize scientific literature

---

## **Challenges**

* **High Dimensionality:** Preserving meaning while reducing TF-IDF dimensions
* **Choosing k:** Balancing interpretability and cluster coherence
* **Data Noise:** Variations in terminology and writing styles
* **Computational Cost:** Processing millions of papers required optimization
* **Interpretability:** Translating model output into human-readable topic labels
* **Integrating methods:** Combining clustering (unsupervised) with classification (supervised)

---
## **Contents of the ZIP File**
1. **README.md**: Documentation and execution instructions
2. **dblp-ref**: Contains the raw DBLP dataset (3 million papers across four JSON files), each including title, abstract, authors, venue, year, citations, and references. These records serve as the raw input for all tasks in the reports. 
3. **project.ipynb**: Python notebook with all code for preprocessing, TF-IDF, SVD, EDA, clustering, classification and anomaly detection.
4. **tsne.py**: Helper script used for generating t-SNE visualizations. 
5. **Output Files.zip**: Contains CSV result files generated during preprocessing and analysis (paper statistics, citation summaries, keyword trends, sampled subsets, venue productivity and TF-IDF vocabulary)
6. **Group 8_Project Report.pdf**: Final project report in narrative style including the introduction, overview of dataset, preprocessing, EDA, clustering, classification, anomaly detection, key findings, challenges, members contribution and conclusion.

---

## **How to Run the Project**
1. Place `dblp-ref` folder and `tsne.py` in the same folder as the notebook or scripts.
2. Open and run the Jupyter notebook:
```bash
jupyter notebook project.ipynb
```
---

## **Tools**
This project requires the following dependencies:
- Python 3.x
- pandas
- numpy
- scikit-learn
- matplotlib
- seaborn
- scipy
- nltk
- lightgbm
- shap
- tsne (if installed as an external package)

Built-in Python modules:
- gzip
- json
- glob
- re
- collections
- time
