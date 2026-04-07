# Electronic Sales Data — Advanced Analytics

A collection of four Jupyter notebooks that apply advanced analytics and machine-learning techniques to an electronic-products sales dataset.

---

## Repository Structure

```
.
├── Individual Assignment Data File Electronic_sales.xlsx   # Source dataset
├── cluster.ipynb             # RFM customer segmentation & loyalty analysis
├── iso.ipynb                 # Anomaly / fraud detection with Isolation Forest
├── ped.ipynb                 # Price Elasticity of Demand (OLS regression)
├── shippingoptimization.ipynb # Multi-objective shipping optimisation (NSGA-II)
└── advanced analytics individual assignment.docx.pdf       # Assignment brief
```

---

## Dataset

`Individual Assignment Data File Electronic_sales.xlsx` contains completed and cancelled orders for electronic products. Key columns include:

| Column | Description |
|---|---|
| `Customer ID` | Unique customer identifier |
| `Purchase Date` | Date the order was placed |
| `Product Type` | Category of product (e.g. Smartphone, Laptop) |
| `Unit Price` | Price per unit |
| `Quantity` | Units purchased |
| `Total Price` | `Unit Price × Quantity` |
| `Add-on Total` | Value of accessories / add-ons |
| `Rating` | Customer satisfaction rating |
| `Loyalty Member` | `Yes` / `No` |
| `Gender` | Customer gender |
| `Payment Method` | Payment method used |
| `Shipping Type` | Standard / Expedited / Express / Overnight / Same Day |
| `Order Status` | `Completed` or `Cancelled` |

---

## Notebooks

### 1. `cluster.ipynb` — Customer Micro-Segmentation & Loyalty Analysis

**Goal:** Segment customers by purchasing behaviour and test whether loyalty-programme membership drives higher add-on spending.

**Approach:**
- Computes **RFM (Recency, Frequency, Monetary)** metrics per customer.
- Standardises features with `StandardScaler` and applies **K-Means clustering** (`k = 4`) to identify cohorts (e.g. Champions, Loyal, At-Risk, Hibernating).
- Performs an independent **Welch's t-test** to compare mean add-on totals between loyalty members and non-members.

**Key outputs:**
- 3-D scatter plot of RFM clusters.
- Boxplot comparing add-on spend across loyalty tiers.
- Statistical conclusion on loyalty-programme effectiveness.

---

### 2. `iso.ipynb` — Anomaly Detection with Isolation Forest

**Goal:** Identify statistically unusual orders that may signal fraud, data-entry errors, or other edge cases, and evaluate whether anomalies correlate with order cancellations.

**Approach:**
- Selects behavioural and transactional features (`Age`, `Total Price`, `Quantity`, `Rating`, categorical columns).
- Encodes categorical variables with `LabelEncoder`.
- Trains an **Isolation Forest** (`contamination = 0.05`) to flag the top 5 % most anomalous orders.
- Compares cancellation rates between normal and anomalous order groups.

**Key outputs:**
- Anomaly counts and cancellation-rate comparison table.

---

### 3. `ped.ipynb` — Price Elasticity of Demand

**Goal:** Estimate how sensitive demand (quantity sold) is to price changes for each product category.

**Approach:**
- Filters to `Completed` orders only.
- Log-transforms `Quantity` and `Unit Price` to enable direct interpretation of regression coefficients as elasticities.
- Fits a separate **OLS model with HC3 robust standard errors** for each `Product Type`, controlling for loyalty status, customer rating, age, and monthly seasonality dummies.

**Key outputs:**
- Elasticity coefficient (β) and p-value per product category.
- Indication of which product types show statistically significant price sensitivity.

---

### 4. `shippingoptimization.ipynb` — Multi-Objective Shipping Optimisation

**Goal:** Find the set of shipping assignments that optimally balances **total cost** against **total customer satisfaction**, subject to daily capacity constraints on premium shipping tiers.

**Approach:**
- Models the assignment problem with two objectives (minimise cost, maximise satisfaction) and four capacity constraints (Expedited ≤ 100, Express ≤ 60, Overnight ≤ 30, Same Day ≤ 20).
- Loyalty multipliers boost satisfaction scores for loyalty members.
- Solves the problem with **NSGA-II** (Non-dominated Sorting Genetic Algorithm II) via the `pymoo` library using integer decision variables.
- Selects a *balanced compromise* solution as the point on the Pareto front closest to the ideal (minimum cost, maximum satisfaction).

**Key outputs:**
- Pareto front plot comparing NSGA-II solutions against random feasible baselines.
- Constraint-validation table confirming all capacity limits are respected.
- Best compromise cost and satisfaction values.

---

## Requirements

Install dependencies with pip:

```bash
pip install pandas numpy openpyxl scikit-learn scipy statsmodels linearmodels matplotlib seaborn pymoo
```

Python 3.8 or later is recommended.

---

## Usage

1. Ensure the dataset file is in the same directory as the notebooks.
2. Open any notebook in JupyterLab or Jupyter Notebook.
3. Run all cells (`Kernel → Restart & Run All`).

Each notebook is self-contained and can be executed independently.

---

## Results Summary

| Analysis | Technique | Primary Finding |
|---|---|---|
| Customer Segmentation | K-Means RFM | 4 distinct customer cohorts identified |
| Loyalty Add-on Spend | Welch's t-test | Loyalty members spend significantly more on add-ons |
| Anomaly Detection | Isolation Forest | Anomalous orders show elevated cancellation rates |
| Price Elasticity | OLS (HC3) | Elasticity varies meaningfully across product categories |
| Shipping Optimisation | NSGA-II | Pareto-optimal assignments balance cost vs. satisfaction within all capacity constraints |
