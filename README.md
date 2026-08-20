# 🛒 Customer Segmentation with RFM Analysis

> Segmenting 4,000+ customers into actionable marketing groups using Recency, Frequency, and Monetary analysis — with K-Means clustering to validate the segments.

---

## 🎯 Business Problem

The marketing team was spending budget treating all customers identically — sending the same emails, the same discounts, the same cadence. This wastes money on customers who would buy anyway (Champions) and ignores the customers who are quietly drifting away (At Risk).

**Objective:** Identify distinct customer segments so marketing can tailor its actions to each group and maximize revenue per marketing dollar spent.

---

## 📁 Project Structure

```
rfm-analysis/
│
├── rfm_analysis.ipynb      ← Main notebook (start here)
├── rfm_output.csv          ← Final customer-level RFM scores & segments
├── README.md
│
└── plots/
    ├── 01_rfm_distributions.png
    ├── 02_segment_analysis.png
    ├── 03_optimal_k.png
    ├── 04_pca_clusters.png
    └── 05_rfm_heatmap.png
```

---

## 📊 Dataset

**UCI Online Retail II** — Real transaction data from a UK-based online retailer (2010–2011)

- **Source:** [archive.ics.uci.edu/dataset/502/online+retail+ii](https://archive.ics.uci.edu/dataset/502/online+retail+ii)
- **Size:** ~500K transactions, ~4,300 unique customers after cleaning
- **Key columns:** `Invoice`, `Customer ID`, `InvoiceDate`, `Quantity`, `Price`

---

## 🔬 Methodology

### Step 1 — Data Cleaning
- Removed rows with missing `Customer ID`
- Filtered out cancelled orders (Invoice starting with `'C'`)
- Removed invalid quantities and negative prices
- Created `TotalPrice = Quantity × Price`

### Step 2 — RFM Calculation

| Metric | Formula | Meaning |
|--------|---------|---------|
| **Recency** | Days since last purchase (from reference date) | How recently did they buy? |
| **Frequency** | Number of unique invoices | How often do they buy? |
| **Monetary** | Sum of TotalPrice | How much do they spend? |

### Step 3 — RFM Scoring
Each metric was split into 5 quantile bins (scores 1–5):
- **Recency:** Lower days = higher score (score is reversed)
- **Frequency & Monetary:** Higher value = higher score

### Step 4 — Segment Mapping
Scores were mapped to 8 business segments based on R/F/M combinations:

| Segment | Description |
|---------|-------------|
| 🏆 Champions | Recent, frequent, high-spend |
| 💙 Loyal Customers | Regular buyers with good spend |
| 🌱 Potential Loyalists | Recent but low frequency |
| 🆕 New Customers | Very recent, first-time buyers |
| ⚠️ At Risk | Used to buy often, now quiet |
| 🚨 Cannot Lose Them | High-frequency buyers going cold |
| 😴 Hibernating | Low recency and frequency |
| ❌ Lost | Lowest R, F, and M |

### Step 5 — K-Means Clustering
Applied `StandardScaler` + `KMeans` on RFM values. Used the Elbow Method and Silhouette Score to select optimal K. Visualised clusters with PCA reduction to 2D.

---

## 📈 Key Findings

- **Top 20% of customers generate ~65% of total revenue** — classic Pareto pattern
- **"At Risk" segment** accounts for ~15% of customers with historically above-average spend — high urgency for win-back campaigns
- **K-Means (K=4)** naturally discovered clusters that aligned with manual segments, validating the scoring logic
- **New Customers have high recency but low frequency** — the first 30 days after acquisition are critical

---

## 💡 Business Recommendations

| Segment | Recommended Action | Priority |
|---------|-------------------|----------|
| Champions | Loyalty rewards, referral programs | 🟢 Maintain |
| Loyal Customers | Upsell, premium product push | 🟢 Grow |
| At Risk | Personalised win-back email + discount | 🔴 Urgent |
| Cannot Lose Them | Direct outreach, VIP retention offer | 🔴 Critical |
| New Customers | Onboarding flow, second-purchase incentive | 🟡 Activate |
| Lost | One-time deep discount; suppress if no response | ⚫ Minimal spend |

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| `pandas` | Data loading, cleaning, aggregation |
| `scikit-learn` | StandardScaler, KMeans, PCA, silhouette_score |
| `matplotlib` | Static visualisations |
| `seaborn` | Heatmap visualisation |
| `numpy` | Numerical operations |

### Install dependencies
```bash
pip install pandas scikit-learn matplotlib seaborn numpy openpyxl
```

---

## 🚀 How to Run

```bash
# 1. Clone the repo
git clone https://github.com/yourusername/rfm-customer-segmentation
cd rfm-customer-segmentation

# 2. Install dependencies
pip install -r requirements.txt

# 3. Download the dataset
# → https://archive.ics.uci.edu/dataset/502/online+retail+ii
# → Place 'online_retail_II.xlsx' in the project root

# 4. Launch the notebook
jupyter notebook rfm_analysis.ipynb
```

---

## 📊 Visual Outputs

| Plot | Description |
|------|-------------|
| `01_rfm_distributions.png` | Histogram of R, F, M values |
| `02_segment_analysis.png` | 4-panel: customer count, revenue share, avg spend, scatter |
| `03_optimal_k.png` | Elbow method + silhouette score for K selection |
| `04_pca_clusters.png` | K-Means vs manual segments in PCA space |
| `05_rfm_heatmap.png` | Revenue heatmap by R-score × F-score |

---

## 🤝 Author

Made by **Anggito Ardiansyah** as part of a Business Analytics portfolio.  
[LinkedIn](https://www.linkedin.com/in/anggito-setiawan-ardiansyah-2971101b4/) · [GitHub](https://github.com/anggitoardiansyah)

---

*Dataset credit: Dr. Daqing Chen, London South Bank University. Licensed under CC BY 4.0.*
