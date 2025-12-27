# 🌊 Flash Flood Early-Warning System (Israel)
> A Deep Learning approach using Entity-Aware LSTM (EA-LSTM) for flood forecasting in arid regions.

## 📌 Overview & The Challenge
This project implements an early-warning system for **flash floods in the Judean Desert and Negev regions of Israel**.
We focus specifically on arid catchments, which present unique hydrological challenges:
* **Rapid Response:** Floods are flashy and short-lived (hours), making daily averages insufficient.
* **Extreme Imbalance:** Flood events occur on <1% of days.
* **Sensor Noise:** Our Exploratory Data Analysis (EDA) revealed critical data quality issues—specifically days with significant rainfall where streamflow data was either **missing (`NaN`)** or **suspiciously zero** (despite >20mm rain), indicating sensor failure rather than actual dry conditions.

To address these, we developed a robust pipeline that prioritizes physical consistency over naive accuracy.

## 🧠 Key Features
* **Architecture:** Dual-Head Entity-Aware LSTM (EA-LSTM) combining static catchment attributes with dynamic meteorological time-series.
* **Smart Masking:** A physics-based mechanism to filter out "poisonous" training samples (both missing values and suspicious zeros during rain), preventing the model from learning incorrect rainfall-runoff relationships.
* **Leakage Prevention:** Strict temporal splitting (Train/Val/Test) and global normalization to avoid look-ahead bias.
* **Metric Selection:** Optimized for **Recall and Precision (F1-Score)** to ensure safety and trust, avoiding the "Accuracy Paradox" of imbalanced datasets.

## 📂 Repository Structure
* `notebook.ipynb`: The complete training pipeline, EDA (including log-scale tail analysis), and evaluation.
* `project_report.pdf`: Detailed concept note, literature review, and architectural decisions.
* `data/`: (Not included in repo).
  > **Data Source:** This project uses the **Caravan - Israel** dataset.
  > You can download the full dataset from [Zenodo](https://zenodo.org/records/15181680) or use the specific subset provided in the assignment resources.

## 🚀 How to Run

### Option A: Google Colab (Recommended)
The easiest way to run the notebook without local installation.
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/dorbenit/flood-warning-system-israel-/blob/main/notebook.ipynb)

1.  Click the badge above to open the notebook.
2.  **Important:** Since the raw data is not stored in this repository, you must upload the relevant basin CSV files (e.g., `il_48125.csv`) and attribute files to the Colab environment (drag & drop into the 'Files' sidebar).
3.  Run all cells.

### Option B: Local Machine
1.  Clone the repository:
    ```bash
    git clone [https://github.com/dorbenit/flood-warning-system-israel.git](https://github.com/dorbenit/flood-warning-system-israel.git)
    cd flood-warning-system-israel
    ```
2.  Install dependencies:
    ```bash
    pip install -r requirements.txt
    ```
3.  Launch Jupyter:
    ```bash
    jupyter notebook notebook.ipynb

## 🔍 Insights & Future Roadmap
While the model achieves a significant improvement in Recall (~60%) over baselines, several limitations remain due to the sparse historical record (post-1980) and daily aggregation:

1.  **Temporal Resolution:** Transitioning to **Hourly Data** is critical. Daily averages smooth out peak discharge, hiding the extreme signal of desert flash floods.
2.  **Transfer Learning:** Given the small sample size of gauged Israeli basins, future iterations should leverage models pre-trained on large datasets (like CAMELS-US) to learn general hydrological physics, followed by fine-tuning on local Israeli data.
3.  **Data Balance:** Further work is needed to handle the extreme class imbalance, potentially via synthetic data augmentation or specialized loss functions beyond weighted BCE.

