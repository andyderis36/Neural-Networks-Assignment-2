# Customer Lifestyle Segmentation using K-Means and Kohonen Self-Organizing Maps (KSOM)

This repository contains the analysis and implementation of customer lifestyle segmentation for the group project **STINK3014 Neural Networks (Assignment 2, Semester A252)** at Universiti Utara Malaysia (UUM). 

The project evaluates and compares two primary clustering approaches—a linear distance-based approach (**K-Means**) and an unsupervised artificial neural network approach (**Kohonen Self-Organizing Map / KSOM**)—using the **HidupSihat Malaysia** dataset.

---

## Table of Contents
- [Customer Lifestyle Segmentation using K-Means and Kohonen Self-Organizing Maps (KSOM)](#customer-lifestyle-segmentation-using-k-means-and-kohonen-self-organizing-maps-ksom)
  - [Table of Contents](#table-of-contents)
  - [Project Description](#project-description)
  - [Repository Structure](#repository-structure)
  - [HidupSihat Malaysia Dataset](#hidupsihat-malaysia-dataset)
  - [Implementation Workflow](#implementation-workflow)
    - [1. Task 1: Data Familiarisation](#1-task-1-data-familiarisation)
    - [2. Task 2: K-Means Clustering](#2-task-2-k-means-clustering)
    - [3. Task 3: Kohonen Map (KSOM) Clustering](#3-task-3-kohonen-map-ksom-clustering)
  - [Clustering Results](#clustering-results)
    - [Customer Segment Profiles (K-Means Results)](#customer-segment-profiles-k-means-results)
  - [Methodology Comparison](#methodology-comparison)
  - [Installation and Execution](#installation-and-execution)
    - [Prerequisites](#prerequisites)
    - [Steps:](#steps)

---

## Project Description

Customer segmentation in the fitness and health industry is essential for designing personalized intervention programs. Using the profiles of 500 members with 8 lifestyle features, this project aims to:
1. Perform data familiarization and linear correlation analysis between customers.
2. Build a **K-Means** clustering model to group customers into optimal segments.
3. Train a **Kohonen Self-Organizing Map (KSOM)** network to map customers to a topology-preserving 2D grid.
4. Analyze the mechanical differences between the two methods in processing non-linear relationships and handling new customer data.

---

## Repository Structure

The repository is structured as follows:

```text
├── output/
│   ├── correlation_heatmap.png         # Correlation matrix heatmap of features
│   ├── elbow_method.png                # Elbow Method graph for K-Means
│   ├── kmeans_centroids.csv            # Coordinate points of K-Means cluster centroids
│   └── customer_lifestyle_segmented.csv# Original dataset with appended cluster labels
├── (preview)NN_A_2.html                # HTML preview of the executed notebook
├── NN_A_2.ipynb                        # Main Jupyter Notebook containing code
├── requirements.txt                    # List of required Python dependencies
├── STINK3014-A252-Assignment02.pdf     # Academic assignment instructions document
├── STINK3014_Assignment02_Customer_Lifestyle.csv # Raw dataset file
├── STINK3014_Customer_Segmentation_Report.md     # Complete analysis report
└── README.md                           # Repository guide (this file)
```

---

## HidupSihat Malaysia Dataset

The dataset **STINK3014_Assignment02_Customer_Lifestyle.csv** contains demographic, psychometric, and lifestyle profile information for **500 members** with **8 key features**:

1. **Age** (years): Age of the respondent.
2. **BMI** (Body Mass Index): Weight-to-height ratio.
3. **Workout_Freq** (sessions/week): Frequency of physical exercise per week.
4. **Avg_Steps** (steps/day): Average daily steps of the respondent.
5. **Monthly_Spend** (MYR): Monthly budget allocation for health and fitness services.
6. **Stress_Level** (scale 1–9): Self-reported psychological stress level.
7. **Sleep_Hours** (hours/night): Nightly sleep duration.
8. **Diet_Quality** (scale 1–9): Self-reported daily diet quality.

---

## Implementation Workflow

The analysis process in [NN_A_2.ipynb] is divided into three main tasks:

### 1. Task 1: Data Familiarisation
* Read the dataset and calculate descriptive statistics of the customer cohort (e.g., mean age of 40.44 years, mean BMI of 26.19 which falls into the overweight category).
* Perform Pearson correlation analysis. All features show very low linear correlation (all values $r < \pm0.12$). This confirms the necessity of a non-linear clustering approach.
* Save the correlation heatmap to [output/correlation_heatmap.png].

### 2. Task 2: K-Means Clustering
* Apply Z-score standardization to normalize varying scales (e.g., thousands of steps daily vs. a 1–9 stress scale).
* Determine the optimal number of clusters using the **Elbow Method** over $K \in [1, 10]$. The Within-Cluster Sum of Squares (WCSS) graph shows a distinct elbow at **$K = 4$**.
* Train the K-Means model with $K=4$, save the centroid coordinates to [output/kmeans_centroids.csv], and export the segmented dataset to [output/customer_lifestyle_segmented.csv].

### 3. Task 3: Kohonen Map (KSOM) Clustering
* Train the Kohonen Map model using the `minisom` library configured with a $2 \times 2$ grid (4 nodes) under competitive learning.
* Evaluate the model using two topological indicators:
  * **Quantisation Error (QE):** $2.5512$ (average distance of inputs to their Best Matching Unit).
  * **Topographic Error (TE):** $0.0000$ (indicating that high-dimensional topology is mapped onto the 2D grid without distortion).
* Test the classification of new customer profiles using 4 test datasets (Dataset A, B, C, D) to analyze the non-linear cluster assignment logic of KSOM.

---

## Clustering Results

### Customer Segment Profiles (K-Means Results)

Based on centroid characteristics, customers are segmented into four consistent behavioral groups:

| Feature / Metric | Cluster 0: "Wealthy Conscious Relaxers" | Cluster 1: "Young Active Go-Getters" | Cluster 2: "Dedicated Vitality Seniors" | Cluster 3: "High-Stress Sedentary Spenders" |
| :--- | :---: | :---: | :---: | :---: |
| **Age** | 43.47 | 26.70 | 48.20 | 44.61 |
| **BMI** | 23.58 | 26.67 | 27.52 | 26.63 |
| **Workout_Freq** | 1.62 | 3.50 | 4.61 | 1.82 |
| **Avg_Steps** | 5,131.62 | 8,083.32 | 8,239.56 | 8,493.45 |
| **Monthly_Spend** | 161.80 MYR | 138.85 MYR | 144.44 MYR | 162.74 MYR |
| **Stress_Level** | 3.95 | 5.02 | 3.20 | 7.47 |
| **Sleep_Hours** | 6.92 | 6.24 | 6.49 | 6.69 |
| **Diet_Quality** | 5.75 | 4.90 | 5.20 | 4.42 |

* **Cluster 0 ("Wealthy Conscious Relaxers")**: Mature customers focusing on diet quality (5.75) and adequate sleep (6.92 hours). Their formal exercise frequency is low (1.62 sessions/week), yet they maintain a healthy BMI (23.58) alongside high monthly spend on wellness products/services (161.80 MYR).
* **Cluster 1 ("Young Active Go-Getters")**: Young professionals (average age 26.70) who are highly active (3.50 workout sessions, 8,083 steps) but experience poorer life balance, with moderate-to-high stress levels (5.02), less sleep (6.24 hours), and moderate diet quality.
* **Cluster 2 ("Dedicated Vitality Seniors")**: Middle-aged group (average age 48.20) with the highest workout frequency (4.61 sessions/week) and the lowest stress level (3.20). They demonstrate high fitness dedication, maintaining active lifestyles despite a slightly overweight average BMI (27.52).
* **Cluster 3 ("High-Stress Sedentary Spenders")**: High-income, middle-aged customers with extreme stress levels (7.47) and the poorest diet quality (4.42). They rarely engage in formal workouts (1.82 sessions/week) but register the highest monthly spend (162.74 MYR) on health/fitness services to compensate for their sedentary habits.

---

## Methodology Comparison

Based on the analysis in [STINK3014_Customer_Segmentation_Report.md], KSOM exhibits several structural advantages over K-Means for lifestyle segmentation:

1. **Topology Preservation**: K-Means treats clusters as isolated entities with no spatial relationships. KSOM structures clusters on an interconnected 2D grid, allowing clear visualization of customer lifestyle transitions between adjacent nodes.
2. **Handling Non-Linearity**: Lifestyle feature relationships are often non-linear (e.g., higher spending does not linearly correspond to higher workout frequency). KSOM utilizes competitive non-linear learning to map these complex interactions into a lower-dimensional space.
3. **Outlier Robustness**: K-Means centroids are highly sensitive to outliers due to simple geometric averaging. KSOM updates winning weights along with their neighborhood functions, offering greater stability against noisy data points.

---

## Installation and Execution

### Prerequisites
Ensure you have **Python 3.8+** installed.

### Steps:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/andyderis36/Neural-Networks-Assignment-2.git
   cd Neural-Networks-Assignment-2
   ```

2. **Create and activate a virtual environment (optional but recommended):**
   * **Windows:**
     ```powershell
     python -m venv .venv
     .venv\Scripts\Activate.ps1
     ```
   * **Linux/macOS:**
     ```bash
     python3 -m venv .venv
     source .venv/bin/activate
     ```

3. **Install the required dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Launch Jupyter Notebook:**
   ```bash
   jupyter notebook
   ```
   Open [NN_A_2.ipynb] to view and execute the analysis code.

5. **Read the Full Report:**
   To access the complete theoretical framework and detailed discussion, read [STINK3014_Customer_Segmentation_Report.md].
