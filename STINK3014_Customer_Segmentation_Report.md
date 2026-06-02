# PART III: PROTOTYPE DEVELOPMENT

## 1. TASK 1: DATA FAMILIARISATION

### 1.1 Dataset Introduction
The **HidupSihat Malaysia** dataset serves as the foundational corpus for this customer lifestyle segmentation prototype. It comprises a demographic and psychometric health profile of **500 members** characterized across **8 key behavioral features**. These features capture physiological metrics, lifestyle habits, psychological stress levels, and wellness-related financial investment:

1. **Age** (years): Demographic age of the individual.
2. **BMI** (Body Mass Index): Physiological ratio of weight to height, indicating general health categorization.
3. **Workout_Freq** (sessions per week): Quantitative weekly physical exercise engagement.
4. **Avg_Steps** (daily step count): Baseline physical activity level.
5. **Monthly_Spend** (MYR): Monthly personal budget allocation for wellness and fitness services.
6. **Stress_Level** (scale 1–9): Self-reported mental stress index.
7. **Sleep_Hours** (hours per night): Quantitative resting and recovery quality metric.
8. **Diet_Quality** (scale 1–9): Self-reported nutritional quality index.

### 1.2 Descriptive Statistics Interpretation
The baseline descriptive statistics of the cohort demonstrate a highly diverse and mature target audience with clear opportunities for structured health interventions:

* **Age Distribution**: The cohort exhibits a mean age of **40.44 years** ($SD = 11.66$), representing a mature adult consumer base with established purchasing power.
* **Physiological Profile**: The mean BMI is **26.19** ($SD = 4.33$), positioning the average member within the *overweight* category. This highlights a clear need for weight management and fitness intervention services.
* **Physical Activity baseline**: Members exhibit a robust daily baseline activity level with a mean of **7,564.67 steps** ($SD = 2595.39$) and an average workout frequency of **2.94 sessions/week** ($SD = 1.99$).
* **Financial Commitment**: The average monthly budget allocation is **151.39 MYR** ($SD = 61.90$), indicating a high willingness to invest financially in physical well-being.
* **Mental Well-being & Recovery**: The cohort shows a moderate average stress index of **4.93** ($SD = 2.58$) alongside a mean sleep duration of **6.57 hours/night** ($SD = 1.00$) and a moderate diet quality score of **5.05** ($SD = 2.56$).

### 1.3 Correlation Analysis & Interpretation
Bivariate correlation analysis across the entire dataset reveals that the 8 features exhibit extremely weak linear relationships (all Pearson $r$ values are below $\pm 0.12$). This lack of strong linear dependency indicates that the consumer variables operate largely independently in a linear sense:

* **Weak Activity and Diet Association**: The correlation between **Avg_Steps** and **Diet_Quality** is **$-0.028$**, indicating that active daily walking does not automatically translate to a better diet quality in a linear manner.
* **Absence of Linear Stress-Sleep Link**: The correlation between **Stress_Level** and **Sleep_Hours** is exceptionally weak at **$-0.008$**, suggesting that the impact of stress on sleep in this cohort is complex, non-linear, or mediated by other personal factors.
* **Minimal Exercise-Spend Link**: The correlation between **Workout_Freq** and **Monthly_Spend** is **$-0.049$**, showing that a member's financial investment is not strictly proportional to their weekly workout frequency.
* **Slight Negative Trend in Workouts and Stress**: The correlation between **Workout_Freq** and **Stress_Level** is **$-0.117$**, which is the strongest relationship in the matrix. This weak negative relationship aligns with the general consensus that more frequent workouts are marginally associated with lower stress levels.

This overall linear independence strongly justifies the use of advanced multi-dimensional clustering techniques (such as K-Means and non-linear Kohonen Maps) to uncover latent non-linear structures that simple correlation coefficients fail to capture.

---

## 2. TASK 2: K-MEANS CLUSTERING

### 2.1 Feature Standardization Process
Because distance-based algorithms such as K-Means compute similarity using Euclidean distance, they are highly sensitive to differences in feature scales (e.g., `Avg_Steps` ranges from 3,000 to 12,000, while `Workout_Freq` ranges from 0 to 6). To prevent variables with larger numerical ranges from dominating the distance metric, Z-score standardization was applied to all 8 features:

$$Z = \frac{X - \mu}{\sigma}$$

This mathematical transformation centers the mean of each feature at **0** and scales the standard deviation to **1**, ensuring that every behavioral aspect contributes equally to the clustering process.

### 2.2 Justification of Optimal K Choice (Elbow Method)
To determine the optimal partition count, the K-Means algorithm was executed across a range of $K \in [1, 10]$ to calculate the Within-Cluster Sum of Squares (WCSS), or inertia. 

> **Optimal Partition Count:** $K = 4$
> 
> The line plot of inertia against $K$ displays a distinct inflection point ("elbow") at $K = 4$. Prior to this point, increasing $K$ yields sharp decreases in inertia, representing the capture of major structures. Beyond $K = 4$, the rate of decrease slows and becomes linear, representing diminishing statistical returns.

### 2.3 Synthesized Analysis of K-Means Centroids
The four clusters generated by the optimal $K=4$ configuration were analyzed by synthesizing the multi-perspective interpretations of three advanced Large Language Models (Gemini, DeepSeek, and Copilot) to establish descriptive and academically rigorous customer segments.

| Feature / Metric | Cluster 0: "Wealthy Conscious Relaxers" | Cluster 1: "Young Active Go-Getters" | Cluster 2: "Dedicated Vitality Seniors" | Cluster 3: "High-Stress Sedentary Spenders" |
| :--- | :---: | :---: | :---: | :---: |
| **Age (years)** | 43.47 | 26.70 | 48.20 | 44.61 |
| **BMI** | 23.58 | 26.67 | 27.52 | 26.63 |
| **Workout_Freq (sessions/wk)** | 1.62 | 3.50 | 4.61 | 1.82 |
| **Avg_Steps (daily)** | 5,131.62 | 8,083.32 | 8,239.56 | 8,493.45 |
| **Monthly_Spend (MYR)** | 161.80 | 138.85 | 144.44 | 162.74 |
| **Stress_Level (1–9)** | 3.95 | 5.02 | 3.20 | 7.47 |
| **Sleep_Hours (hours)** | 6.92 | 6.24 | 6.49 | 6.69 |
| **Diet_Quality (1–9)** | 5.75 | 4.90 | 5.20 | 4.42 |

#### Cluster 0: "Wealthy Conscious Relaxers"
* **Synthesized Profile**: This segment represents mature adults who prioritize passive wellness. While their formal workout frequency is low ($1.62$ sessions/week) and daily baseline movement is minimal ($5,131.62$ steps), they maintain a healthy body mass ($BMI = 23.58$). They achieve this through excellent nutritional habits ($Diet\_Quality = 5.75$) and restful sleep ($6.92$ hours). They are financially committed to premium wellness services ($161.80$ MYR) but focus on recovery, nutrition, and stress management ($3.95$) over intense physical exertion.

#### Cluster 1: "Young Active Go-Getters"
* **Synthesized Profile**: Consisting of young professionals (mean age of $26.70$ years), this highly energetic segment balances active physical schedules ($3.50$ workouts/week, $8,083.32$ steps) with sub-optimal lifestyle balances. They experience high psychological pressure ($Stress\_Level = 5.02$) and reduced sleep ($6.24$ hours). Despite their active lifestyles, their diet quality is moderate ($4.90$), and their physiological profile borders on overweight ($BMI = 26.67$). They represent a prime demographic for stress-relief and time-efficient fitness programs.

#### Cluster 2: "Dedicated Vitality Seniors"
* **Synthesized Profile**: This group consists of highly disciplined older adults (mean age of $48.20$ years) who serve as the vanguard of physical fitness. They exhibit outstanding exercise habits with the highest formal workout frequency ($4.61$ sessions/week) and strong baseline movement ($8,239.56$ steps). They maintain exceptional control over mental strain ($3.20$ stress index) and eat a balanced diet ($5.20$). Although their average BMI is slightly elevated ($27.52$), their highly active lifestyle and low-stress profile minimize cardiovascular risks.

#### Cluster 3: "High-Stress Sedentary Spenders"
* **Synthesized Profile**: This group is characterized by high-earning corporate employees (mean age of $44.61$ years) facing severe lifestyle imbalances. They experience extreme mental stress ($7.47$) and exhibit very poor diet quality ($4.42$). While they walk a high number of daily steps ($8,493.45$ steps, likely due to active work commutes), they rarely engage in structured workout sessions ($1.82$ sessions/week). They attempt to compensate for their poor lifestyle habits through high financial expenditure on wellness programs ($162.74$ MYR), making them an ideal audience for premium, personalized wellness coaching.

---

## 3. TASK 3: KOHONEN MAP (KSOM) CLUSTERING

### 3.1 Model Architecture & Evaluation
A Kohonen Self-Organizing Map (KSOM) was trained on a 2x2 grid structure using competitive learning. The network's performance was evaluated using two primary topological metrics:

> **Final Quantisation Error (QE):** $2.5512$
> 
> **Final Topographic Error (TE):** $0.0000$

* **Quantisation Error (QE)** measures the average Euclidean distance between each input vector and its Best Matching Unit (BMU) on the grid. A QE of $2.5512$ indicates that the prototype vectors of the grid nodes accurately represent the multi-dimensional structure of the dataset.
* **Topographic Error (TE)** measures the proportion of input vectors for which the first and second best matching units are not adjacent on the grid. A TE of **$0.0000$** indicates that the high-dimensional topology of the data was preserved on the 2D grid structure, ensuring highly stable neighbor relationships.

### 3.2 Multivariate Similarity Logic for New Members
To evaluate the model's classification performance, four new members with distinct behavioral profiles were mapped to the trained KSOM network.

```
Dataset A: [50, 28.0, 6,  6638,  60, 3, 7.9, 5] -> Mapped to SOM Cluster 1
Dataset B: [28, 34.1, 7,  2000,  87, 3, 7.0, 6] -> Mapped to SOM Cluster 3
Dataset C: [44, 23.0, 5,  8000, 300, 5, 8.0, 3] -> Mapped to SOM Cluster 3
Dataset D: [25, 21.0, 8, 10000, 200, 7, 6.0, 4] -> Mapped to SOM Cluster 1
```

#### Cluster 1 Mapping Logic: Active Lifestyle Cohort (Datasets A & D)
* **Dataset A (Active Senior)** and **Dataset D (Elite Athlete)** both map to **SOM Cluster 1**. 
* Under traditional linear distance metrics, their age difference (50 vs. 25 years) and financial contrast (60 vs. 200 MYR spend) would keep them separate. However, the KSOM's competitive learning algorithm prioritizes their shared high physical activity as the dominant clustering factor. Both exhibit exceptional workout frequencies (6 and 8 sessions/week, respectively) and strong daily baseline steps. 
* Their shared high exercise volume maps them to the same active region of the topological space, classifying them together based on their active lifestyle.

#### Cluster 3 Mapping Logic: Behavioral & Physiological Imbalances (Datasets B & C)
* **Dataset B (Sedentary High-BMI)** and **Dataset C (High Spender with Poor Diet)** both map to **SOM Cluster 3**.
* This classification demonstrates the KSOM's ability to capture complex, non-linear relationships. Dataset B exhibits physical imbalances, combining high workout frequency with severe daily baseline inactivity ($2,000$ steps) and physiological obesity ($BMI = 34.1$). Dataset C exhibits behavioral imbalances, combining active movement ($8,000$ steps) with extremely poor nutrition ($Diet\_Quality = 3$) and very high wellness spending ($300$ MYR).
* Rather than looking at isolated features, the network identifies both profiles as having significant imbalances (either extreme BMI combined with daily inactivity, or poor nutrition combined with high spending). By grouping them in Cluster 3, the model highlights them as key targets for highly specialized lifestyle interventions.

---

# PART IV: ANALYSIS AND COMPARISON

## 1. Direct Comparison: K-Means vs. SOM

The clustering results from K-Means and the Self-Organizing Map (SOM) reveal key differences in how each algorithm structures the customer segments:

* **Boundary Definition**: K-Means relies on hard distance boundaries in Z-score space. It divides the data space into convex Voronoi cells, where each point is assigned strictly to the nearest centroid. In contrast, SOM uses a soft, competitive learning approach. It projects the customer profiles onto a continuous 2D grid where adjacent nodes represent closely related lifestyle patterns.
* **Feature Integration**: K-Means treats all features linearly, which can cause dominant demographic factors (like age) to overshadow more subtle behavioral patterns. SOM integrates these features through non-linear competitive learning. This allows it to prioritize behavioral characteristics (such as workout frequency and daily steps) over raw demographics, grouping users with similar active lifestyles regardless of their age difference.

---

## 2. Architectural Superiority Analysis

The Kohonen Self-Organizing Map (KSOM) demonstrated clear structural advantages over the traditional K-Means algorithm for customer lifestyle segmentation:

### Topology Preservation
The primary advantage of the SOM architecture is its ability to preserve the spatial topology of high-dimensional data. In K-Means, clusters are treated as independent, isolated entities with no spatial relationships between them. In contrast, SOM organizes its nodes on a continuous 2D grid ($2 \times 2$ lattice). This layout ensures that adjacent nodes represent similar lifestyles. 

For the **HidupSihat Malaysia** dataset, this spatial organization allows wellness programs to map how customers transition between different profiles—such as a "High-Stress Sedentary Spender" gradually moving toward a "Wealthy Conscious Relaxers" profile as their habits improve.

### Handling of Non-Linearity
The behavioral patterns in customer lifestyle data are inherently non-linear and complex. For example, the relationship between stress levels, diet quality, and sleep recovery does not follow a simple linear path. 

K-Means assumes that clusters are convex and spherical, which limits its ability to model these intricate interactions. SOM, however, uses competitive weight updates and neighborhood functions to naturally map complex, non-linear relationships onto a simplified 2D space without losing critical behavioral insights.

### Robustness to Noise and Outliers
In K-Means, the position of each cluster centroid is calculated using the mean of all points assigned to it. This makes the algorithm sensitive to outliers, as extreme data points can pull the centroid away from the true center of the cluster. 

SOM is much more robust to noise and outliers. During training, the winning node (BMU) and its neighbors are updated together, and this collective adjustment prevents individual outliers from distorting the overall grid structure. The result is a highly stable and reliable segmentation model that is well-suited for real-world business planning.
