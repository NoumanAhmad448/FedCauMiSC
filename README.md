# FedCauMiSC: Federated Causal Missingness-aware method based on Star-graph Cumulant

This repository contains the official implementation of **FedCauMiSC**, a framework for federated causal discovery designed to handle two major challenges in real-world data: **variable heterogeneity** (clients observing non-identical variable sets) and **missing data**.

The methodology is based on using Inverse Probability Weighting (IPW) to correct for missingness bias and higher-order cumulants to identify causal direction, all without sharing raw client data.

## 📂 Repository Structure

The repository includes two primary Jupyter Notebooks, each designed for a specific experimental evaluation:

1.  **`notebook8503f9757c.ipynb` (Alpha Evaluation on Sachs):**
    *   **Purpose:** Evaluates the performance of FedCauMiSC on the Sachs dataset under varying levels of **variable heterogeneity**.
    *   **Methodology:** It splits the Sachs dataset into 6 clients. The `alpha` parameter (from 1/11 to 6/11) is used to simulate the proportion of variables each client observes. This notebook is used to generate the comparison results against the baselines from the Information Sciences paper (e.g., FedRFCI, FedFCI).

2.  **`notebooka76ae517d3.ipynb` (K-Client Evaluation on Sachs & Asia):**
    *   **Purpose:** Evaluates the performance and robustness of FedCauMiSC across different numbers of clients (\(K = 1\) to \(6\)).
    *   **Methodology:** It runs the full pipeline on both the **Sachs** and **Asia** datasets. For a fixed number of clients, it injects missingness, performs the local IPW correction, aggregates cumulants, and evaluates the final graph. This notebook generates the results for the detailed \(K\)-client performance table.

## 🧠 Core Methodology

The FedCauMiSC pipeline operates in two main phases:

**Phase 1: Client-Side (Local Computation)**
*   **Variable Classification:** Each client classifies its observed variables into fully observed (\(V_o\)) and partially observed (\(V_m\)) sets.
*   **Missingness Model:** For each variable in \(V_m\), a logistic regression model is fitted to predict the probability of missingness based on its parents.
*   **IPW & Rank Transformation:** Inverse Probability Weights are computed to correct for selection bias. A rank transformation is applied to the data to prevent the higher-order cumulants from collapsing to zero under extreme sparsity.
*   **Cumulant Extraction:** The local causal skeleton is decomposed into star graphs. For each center-peripheral pair, four higher-order cumulants are computed (\(C_3, C_{1,2}, C_{2,1}\)).

**Phase 2: Server-Side (Global Recovery)**
*   **Aggregation:** The server aggregates the local cumulants from all clients using an effective sample size (\(n_{\text{eff}}\)) weighting scheme.
*   **Recursive Source Recovery:** The server computes the asymmetry measure (\(\tau_{ij}\)) to identify source variables and recursively eliminates their influence (using Lemma 2) to recover the global causal order.
*   **Graph Construction:** The final causal graph is assembled from the recovered order.

## 🚀 How to Run

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/NoumanAhmad448/FedCauMiSC.git
    cd FedCauMiSC
2. Install dependencies:
Ensure you have Python 3.8+ and the following libraries installed:
```
pip install pandas numpy scikit-learn networkx matplotlib scipy
```
3. Run the Notebooks:
Open the notebooks in Jupyter Lab or Kaggle. The notebooks are self-contained and will automatically download the required datasets (Sachs and Asia) if they are not found locally. Execute the cells in order to reproduce the results.


