# 🛍️ Mall Customer Segmentation using Unsupervised Learning

**K-Means · Agglomerative Hierarchical Clustering · DBSCAN**

Practical Report 1 (PR 1) — Unsupervised Learning, *Red & White Skill Education*
**Author:** Herit Tanna

---

## 📌 Project Overview

A shopping mall has data about 200 customers but does not know *who* its customers really are. In this project I use **three unsupervised learning (clustering) algorithms** to group similar customers together, compare the algorithms, and give simple marketing advice for every customer group.

Everything is done in one Jupyter Notebook (`UL_PR1.ipynb`) that is divided into 7 tasks with Markdown explanations for every step.

## 🎥 Video

**Video explanation (face + screen):** [PASTE YOUR GOOGLE DRIVE / YOUTUBE (UNLISTED) LINK HERE](https://your-video-link-here)

## 📊 Dataset

| Item | Detail |
|---|---|
| Name | Mall Customer Segmentation Data |
| Source | [Kaggle — Customer Segmentation Tutorial in Python](https://www.kaggle.com/datasets/vjchoudhary7/customer-segmentation-tutorial-in-python) |
| File | `Mall_Customers.csv` |
| Size | 200 rows × 5 columns (no missing values, no duplicates) |
| License | CC0: Public Domain |

| Column | Description |
|---|---|
| `CustomerID` | Unique ID (dropped before modelling) |
| `Gender` | Male / Female (encoded to 1 / 0 with `LabelEncoder`) |
| `Age` | Age in years (18–70) |
| `Annual_Income` | Yearly income in thousand USD (15–137) — renamed from `Annual Income (k$)` |
| `Spending_Score` | Score from 1 to 100 given by the mall — renamed from `Spending Score (1-100)` |

## 📁 Repository Structure

```
├── UL_PR1.ipynb          # Main notebook (all 7 tasks, executed)
├── UL_PR1.html           # HTML export of the notebook
├── Mall_Customers.csv    # Dataset
├── requirements.txt      # Libraries with version numbers
├── README.md             # This file
└── screenshots/          # All key plots (used below)
```

## 🧭 What Was Done (Task by Task)

| Task | Description |
|---|---|
| **1. Data Loading & EDA** | Loaded the CSV, dropped `CustomerID`, renamed columns, encoded Gender, plotted histograms + KDE, pairplot and correlation heatmap, and wrote an EDA summary. |
| **2. Scaling & Feature Selection** | Applied `StandardScaler` to Age, Annual_Income and Spending_Score (`df_scaled`), created `df_2f` (Annual_Income + Spending_Score), and explained why scaling is needed. |
| **3. K-Means** | Elbow method + Silhouette score → **k = 5**. Fitted the model, plotted clusters with centroids, and built a profile table with segment names. |
| **4. Hierarchical** | Ward dendrogram with cut line → 5 clusters. Fitted `AgglomerativeClustering`, compared with K-Means. |
| **5. DBSCAN** | 4-NN distance plot to estimate `eps`, grid search on `eps × min_samples`, final model `eps = 0.4`, `min_samples = 5`, noise points shown separately. |
| **6. Comparison & Insights** | Three-panel comparison figure, metrics table (Silhouette, Davies-Bouldin, Calinski-Harabasz) and a business insight report. |
| **7. Notebook Quality** | Task-wise Markdown, consistent light plot style, Restart & Run All with zero errors, HTML export. |

## 🤖 Algorithms Used

### 1. K-Means Clustering
* **Idea:** Choose `k` cluster centres (centroids). Every customer joins the nearest centroid; centroids are then moved to the average of their customers. This repeats until nothing changes.
* **Needs:** the number of clusters `k` (found with the Elbow method and Silhouette score).
* **Strength:** fast, simple and easy to explain. **Weakness:** assumes round, similar-sized clusters and forces every point into a cluster.
* **My result:** `k = 5` (Elbow and Silhouette both agree; Silhouette = 0.555).

### 2. Agglomerative Hierarchical Clustering
* **Idea:** Start with every customer as its own cluster and repeatedly merge the two closest clusters (**Ward linkage** merges the pair that increases the within-cluster variance the least). The merge history is drawn as a **dendrogram**.
* **Needs:** the number of clusters, chosen by cutting the dendrogram at its longest vertical gap.
* **Strength:** no centroids needed and the dendrogram shows how groups are related. **Weakness:** slow on big data and cannot undo a merge.
* **My result:** 5 clusters (Silhouette = 0.554). Only 4 of 200 customers are placed differently from K-Means.

### 3. DBSCAN (Density-Based Clustering)
* **Idea:** A cluster is a dense area of points. `eps` is the neighbourhood radius and `min_samples` is the number of points needed to call an area dense. Points in no dense area become **noise**.
* **Needs:** `eps` and `min_samples` (not the number of clusters).
* **Strength:** finds clusters of any shape and detects outliers. **Weakness:** sensitive to `eps`, and struggles when groups touch each other.
* **My result:** `eps = 0.4`, `min_samples = 5` → 4 clusters + 15 noise points (Silhouette = 0.478, noise excluded).

## 📈 Results

| Algorithm | Clusters | Noise points | Silhouette ↑ | Davies-Bouldin ↓ | Calinski-Harabasz ↑ |
|---|---|---|---|---|---|
| **K-Means** | 5 | 0 | **0.555** | **0.572** | **248.6** |
| Hierarchical (Ward) | 5 | 0 | 0.554 | 0.578 | 244.4 |
| DBSCAN | 4 | 15 | 0.478 | 0.591 | 146.9 |

**Best algorithm: K-Means** — best scores on all three metrics and the easiest to interpret.

### Customer Segments (from K-Means) and Marketing Actions

| Segment | Customers | Marketing action |
|---|---|---|
| High Income, High Spenders | 39 | Loyalty / VIP programme with early access and private events |
| Low Income, High Spenders | 22 | Budget promotions: youth discounts, flash sales, instalment offers |
| Average Income, Average Spenders | 81 | Bundle offers and seasonal sales to increase spending |
| High Income, Low Spenders | 35 | Premium personal invitations, luxury showcases and a short survey |
| Low Income, Low Spenders | 23 | Low-cost engagement: coupons on essentials and free events |

## 🖼️ Screenshots

**1. Distributions of Age, Annual Income and Spending Score**
![Histograms](screenshots/01_histograms.png)

**2. Pairplot — the Income vs Spending panel shows clear groups**
![Pairplot](screenshots/02_pairplot.png)

**3. Correlation heatmap — all features have weak correlations**
![Correlation heatmap](screenshots/03_correlation_heatmap.png)

**4. Elbow Method — the elbow is at k = 5**
![Elbow method](screenshots/04_elbow_method.png)

**5. Silhouette Score — highest at k = 5 (0.555)**
![Silhouette scores](screenshots/05_silhouette_scores.png)

**6. K-Means clusters with centroids (black X)**
![K-Means clusters](screenshots/06_kmeans_clusters.png)

**7. Ward dendrogram — the cut line gives 5 clusters**
![Dendrogram](screenshots/07_dendrogram.png)

**8. Agglomerative Hierarchical clusters**
![Hierarchical clusters](screenshots/08_hierarchical_clusters.png)

**9. K-Means vs Hierarchical — almost identical results**
![K-Means vs Hierarchical](screenshots/09_kmeans_vs_hierarchical.png)

**10. 4-NN distance plot — the knee suggests eps ≈ 0.4**
![k-distance plot](screenshots/10_k_distance_plot.png)

**11. DBSCAN clusters with noise points (black x)**
![DBSCAN clusters](screenshots/11_dbscan_clusters.png)

**12. Three-panel comparison: K-Means | Hierarchical | DBSCAN**
![Three-panel comparison](screenshots/12_three_panel_comparison.png)

## 🛠️ Tools Used

* **Python 3** — programming language
* **Jupyter Notebook** — for writing and running the code
* **Pandas & NumPy** — data handling
* **Matplotlib & Seaborn** — plots
* **Scikit-learn** — StandardScaler, LabelEncoder, KMeans, AgglomerativeClustering, DBSCAN, NearestNeighbors and the evaluation metrics
* **SciPy** — linkage and dendrogram
* **Git & GitHub** — version control

## ⚙️ How to Run

```bash
# 1. Clone the repository
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>

# 2. Install the required libraries
pip install -r requirements.txt

# 3. Open the notebook
jupyter notebook UL_PR1.ipynb
```

Then click **Kernel → Restart & Run All**. `Mall_Customers.csv` must be in the same folder as the notebook. The plots are saved automatically in the `screenshots` folder.

### 📦 Requirements (`requirements.txt`)

```
pandas==3.0.2
numpy==2.4.4
matplotlib==3.10.8
seaborn==0.13.2
scikit-learn==1.8.0
scipy==1.17.1
jupyter
```

## ✅ Key Learnings

* Distance-based algorithms need **scaled features**, otherwise the feature with the bigger range dominates.
* The **Elbow method** and **Silhouette score** should be used together to choose `k`.
* K-Means and Hierarchical clustering agree on this data, while DBSCAN is useful for spotting **outliers** but merges groups that touch each other.
* Clustering is only useful when the groups are turned into **clear business actions**.

---

*Dataset: Kaggle — CC0 Public Domain. Project submitted for Red & White Skill Education, Unsupervised Learning (PR 1).*
