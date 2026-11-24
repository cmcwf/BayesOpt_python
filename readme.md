# Bayesian Optimization for Chemical Reaction Tuning (`bayesopt_python`)

This repository contains Python workflows and experimental datasets for applying **Bayesian Optimization** to chemical reaction optimization. The project leverages **[BayBE](https://github.com/emdgroup/baybe)** as its primary Bayesian optimization framework to conduct an iterative active learning campaign designed to maximize key performance metrics such as **Ultra-Performance Liquid Chromatography (UPLC) peak area** and **chemical yield (%)**.

## 📌 Repository Overview

Optimizing chemical reactions often requires navigating complex parameter spaces comprising continuous variables (e.g., temperature, reaction time, stoichiometry) and discrete or categorical choices (e.g., solvents, catalysts, ligands). This repository provides a structured, step-by-step experimental campaign using **BayBE** to suggest optimal next-round laboratory conditions.

### Highlights

* **Powered by BayBE:** Built using EMD Group's [BayBE](https://github.com/emdgroup/baybe) framework specifically tailored for digital chemistry and experimental design.
* **Iterative Campaign Notebooks:** Step-by-step optimization rounds (`campaign/`) tracking progressive improvements in UPLC area and reaction yield.
* **Experimental Datasets:** Complete tabular records (`data_set/`) in CSV and Excel formats containing input conditions and measured output responses.
* **Quick-Start Example:** A standalone notebook (`optimization_example.ipynb`) demonstrating the core Bayesian Optimization pipeline.

## 📁 Directory Structure

```
bayesopt_python/
├── campaign/                              # Iterative campaign notebooks
│   ├── optimization.ipynb                 # Initial baseline optimization round
│   ├── optimization_2_UPLC_area.ipynb     # Iteration 2: Targeted UPLC peak area
│   ├── optimization_3_yield.ipynb         # Iteration 3: Targeted chemical yield
│   ├── optimization_4_UPLC_area.ipynb     # Iteration 4: Refined UPLC peak area
│   ├── optimization_5_yield.ipynb         # Iteration 5: Refined yield optimization
│   ├── optimization_6_UPLC_area_final.ipynb # Final UPLC area evaluation
│   ├── optimization_7_yield_final.ipynb   # Final yield evaluation
│   └── optimization_8_just_suggestion.ipynb # Next-step experimental candidate generator
│
├── data_set/                              # Experimental datasets (CSV & XLSX)
│   ├── result.xlsx                        # Raw/initial experimental results
│   ├── result_UPLC_area.csv               # Campaign data filtered for UPLC area
│   ├── result_UPLC_area.xlsx              # Excel version of UPLC area dataset
│   ├── result_yield.csv                   # Campaign data filtered for reaction yield
│   ├── result_UPLC_area_yield.csv         # Combined UPLC area & yield dataset
│   ├── result_all.csv / .xlsx             # Complete combined experimental records
│   ├── result_final.csv / .xlsx           # Finalized dataset post-campaign
│   └── result_trimmed.xlsx                # Processed/trimmed parameter subset
│
├── optimization_example.ipynb             # Standalone template & tutorial notebook
└── README.md                              # Repository documentation
```

## ⚙️ Methodology & Optimization Workflow

The optimization process follows an active learning loop implemented with **BayBE**:

1. **Parameter & Target Definition:** Define chemical search spaces (continuous parameters, discrete choices) and optimization objectives within BayBE target spaces.

2. **Surrogate Modeling:** Fit Gaussian Process (GP) surrogate models $f(\mathbf{x}) \sim \mathcal{GP}(m(\mathbf{x}), k(\mathbf{x}, \mathbf{x}'))$ to predict response metrics $y = f(\mathbf{x}) + \epsilon$.

3. **Acquisition Function & Recommendation:** Compute acquisition scores $\alpha(\mathbf{x})$ (such as Expected Improvement $\text{EI}(\mathbf{x})$ or Upper Confidence Bound $\text{UCB}(\mathbf{x})$ ) to select candidate conditions:

   ```math
   \mathbf{x}_{\text{next}} = \arg\max_{\mathbf{x} \in \mathcal{X}} \alpha(\mathbf{x})
   ```

4. **Experimental Execution:** Run laboratory experiments at $\mathbf{x}_{\text{next}}$ and measure output metrics (UPLC peak area, reaction yield).

5. **Iteration:** Feed new data back into the BayBE campaign instance to generate recommendations for subsequent rounds.

## 🚀 Getting Started

### Prerequisites

Install Python 3.10+ along with **BayBE** and standard data science packages:

```bash
pip install baybe pandas numpy matplotlib jupyterlab
```

### Running the Code

1. **Clone the repository:**
   ```bash
   git clone https://github.com/cmcwf/bayesopt_python.git
   cd bayesopt_python
   ```

2. **Launch Jupyter Lab:**
   ```bash
   jupyter lab
   ```

3. **Explore the Quick-Start Notebook:**
   Open `optimization_example.ipynb` to see how BayBE targets, parameters, and campaign instances are configured.

4. **Follow the Campaign Steps:**
   Browse through the `campaign/` directory sequentially to inspect each iteration of the chemical optimization process.

## 📊 Objective Metrics

* **UPLC Peak Area:** Used as a proxy for product formation or relative purity obtained via Ultra-Performance Liquid Chromatography.
* **Reaction Yield (%):** Measured isolated or quantitative reaction yield percentage.

## 📜 License

This project is open-source and available under the [MIT License](LICENSE).

## ✉️ Contact & Acknowledgments

* Primary Bayesian Optimization Framework: [BayBE (EMD Group)](https://github.com/emdgroup/baybe)
* For questions or issues regarding this repository, please open an issue in GitHub.
