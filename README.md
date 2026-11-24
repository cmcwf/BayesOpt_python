Experimental Optimization Campaign WorkflowThis repository contains the setup for running iterative data-driven experimental optimization campaigns (e.g., Bayesian Optimization, Active Learning, or Design of Experiments).📁 Repository Structure.
├── optimisation_example.ipynb   # Master template and benchmark notebook for optimization algorithms
├── data_set/                    # Directory for experimental datasets, measurements, and metadata
│   ├── raw_experiments.csv      # Initial experimental results / baseline measurements
│   └── campaign_history.csv    # Consolidated dataset updated after each experimental round
└── campaign/                    # Execution notebooks for active optimization campaigns
    ├── 01_initial_sampling.ipynb # Notebook for initial batch selection (e.g., LHS, Sobol)
    ├── 02_iteration_1.ipynb     # Iteration 1 model training & next experiment suggestions
    └── ...
🚀 Optimization Workflow Steps1. Benchmark & Setup (optimisation_example.ipynb)Use optimisation_example.ipynb as the baseline standard for:Defining the target objective functions $f(\mathbf{x})$ to maximize or minimize.Setting parameter bounds $\mathbf{x} \in [\mathbf{x}_{\text{lower}}, \mathbf{x}_{\text{upper}}]$.Configuring Gaussian Process (GP) surrogates, Random Forests, or Optuna/Ax frameworks.Setting acquisition functions such as Expected Improvement ($\text{EI}(\mathbf{x})$) or Upper Confidence Bound ($\text{UCB}(\mathbf{x})$).2. Experimental Data Management (data_set/)Store all observed inputs $\mathbf{X} = \{ \mathbf{x}_1, \mathbf{x}_2, \dots, \mathbf{x}_n \}$ and corresponding outputs $\mathbf{y} = \{ y_1, y_2, \dots, y_n \}$.Ensure standard column formatting across datasets (e.g., parameter names, run IDs, timestamp, measured metrics, noisy/replicate flags).3. Executing Campaign Iterations (campaign/)Each round of experimentation follows a standard execution loop inside a dedicated notebook in campaign/:Load Data: Import updated experimental results from data_set/.Train Surrogate Model: Fit model $M: \mathbf{x} \mapsto y$ using current observations.Optimize Acquisition: Evaluate acquisition function $\alpha(\mathbf{x})$ over the decision space.Propose Next Batch: Output the top $k$ recommended experimental conditions $\mathbf{x}_{n+1}, \dots, \mathbf{x}_{n+k}$.Log & Export: Save proposed experiments for lab execution and log model metrics.💻 Standard Campaign Code Template (Python)Below is the standard workflow logic to include in campaign iteration notebooks:import pandas as pd
import numpy as np

# 1. Load current dataset
df = pd.read_csv('../data_set/campaign_history.csv')
X_train = df[['param_1', 'param_2', 'param_3']].values
y_train = df['target_metric'].values

# 2. Fit Surrogate (Example with Gaussian Process / Bayesian Optimization)
# (Refer to optimisation_example.ipynb for hyperparameter tuning)
from sklearn.gaussian_process import GaussianProcessRegressor
from sklearn.gaussian_process.kernels import Matern

kernel = Matern(nu=2.5)
gp = GaussianProcessRegressor(kernel=kernel, n_restarts_optimizer=10, random_state=42)
gp.fit(X_train, y_train)

# 3. Suggest Next Batch via Acquisition Function
# Output suggested candidates dataframe to send to lab/experiment pipeline
📌 Best PracticesReproducibility: Fix random seeds (random_state) in campaign notebooks.Versioning: Keep raw experimental data immutable and append new round results cleanly.Checkpointing: Export candidate suggestions as CSV files with unique identifiers for tracking back from physical experiments.
