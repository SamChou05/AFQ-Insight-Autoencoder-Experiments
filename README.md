# AFQ-Insight Autoencoder Experiments

This repository contains experiments applying convolutional autoencoders and variational autoencoders to diffusion MRI brain connectivity data from the AFQ-Insight HBN dataset. The main focus is on learning brain representations for age prediction and removing site effects using adversarial training.

## What's in this repository

The experiments are organized into several main directories:

- **ConvAE_Experiments/** - The main experimental code
  - **Non_Variational/** - Standard autoencoder experiments
    - **First_tract_data/** - Single tract (tract 0) experiments for validation
    - **Fa_tracts_data/** - FA-only tract experiments  
  - **Variational/** - Variational autoencoder experiments
    - **all_tracts/** - All 48 tracts with multi-task learning
    - **fa_tracts_data/** - FA-only VAE experiments
    - **first_tract_data/** - Single tract VAE experiments
- **FC_AE_Experiments/** - Fully connected autoencoder experiments (less developed)
- **evaluating_tracts/** - Individual tract importance analysis
- **Experiment_Utils/** - Core utilities and model definitions
- **batch_scripts/** - SLURM batch job scripts for HPC systems
- **tests/** - Test files to verify installation

## Getting started
### Installation

First, clone this repository:

```bash
git clone https://github.com/yourusername/AFQ-Insight-Autoencoder-Experiments.git
cd AFQ-Insight-Autoencoder-Experiments
```

Create a virtual environment. I recommend using conda:

```bash
conda create -n afq_experiments python=3.11
conda activate afq_experiments
```

Or if you prefer regular Python virtual environments:

```bash
python -m venv afq_experiments
source afq_experiments/bin/activate  # On Linux/Mac
```

Install the dependencies:

```bash
pip install -r requirements.txt
pip install -e .
```

Test that everything works:

```bash
python tests/test_installation.py
```

See the [AFQ-Insight documentation](https://yeatmanlab.github.io/AFQ-Insight/) for more details on the data format.

## Running experiments

### Using Jupyter notebooks

Start Jupyter and navigate to the experiment folders:

```bash
jupyter notebook
```

The main notebooks to try:
- `ConvAE_Experiments/Non_Variational/First_tract_data/conv_combined_experiment.ipynb`
- `ConvAE_Experiments/Variational/all_tracts/vae_age_site_stages_all.ipynb`
- `ConvAE_Experiments/Variational/fa_tracts_data/vae_age_site_stages.ipynb`

Use the batch scripts mainly to run 

```bash
sbatch batch_scripts/run_notebook_vae_age_site_stages.sbatch
sbatch batch_scripts/run_notebook_vae_age_site_stages_all_tract.sbatch
```

## Types of experiments

### Standard autoencoders

These are in `ConvAE_Experiments/Non_Variational/`. They focus on basic reconstruction tasks and hyperparameter optimization. Good for establishing baselines and validating that the autoencoder approach works.

### Variational autoencoders 

These are more advanced and live in `ConvAE_Experiments/Variational/`. They do multi-task learning: reconstruction, age prediction, and site effect removal using adversarial training. This is where most of the interesting work happens.

### Tract importance analysis

The `evaluating_tracts/` directory contains code to figure out which brain tracts are most important for age prediction. Run it like:

```bash
cd evaluating_tracts
python tract_importance_evaluation.py --output-dir results_fa_only
```

Use the other repository addressed below to plot or visualize experiment results. 

For visualization, you can use the companion repository [AFQ-Insight-Autoencoder-Plotting](https://github.com/SamChou05/AFQ-Insight-Autoencoder-Plotting) to create plots and graphs from the CSV files.

## Customizing experiments

### Changing hyperparameters

Look for sections like this in the notebooks or scripts:

```python
latent_dims = [32, 64, 128]
dropout_values = [0.0, 0.1, 0.2]
w_recon = 1.0      # Reconstruction weight
w_kl = 0.001       # KL divergence weight (VAE)
w_age = 15.0       # Age prediction weight
w_site = 5.0       # Site adversarial weight
```

### Adding new models

1. Define your model architecture in `Experiment_Utils/models.py`
2. Add any new training logic to `Experiment_Utils/utils.py`
3. Create an experiment script based on existing ones

### Modifying data preprocessing

The main data preparation functions are in `Experiment_Utils/utils.py`:
- `prep_fa_dataset()` - For FA-only data
- `prep_first_tract_data()` - For single tract data
- `prep_fa_flattened_remapped_data()` - For site-remapped data

See `requirements.txt` for the complete list.

## Related work

- [AFQ-Insight](https://github.com/yeatmanlab/AFQ-Insight) - The core AFQ analysis framework
- [AFQ-Insight-Autoencoder-Plotting](https://github.com/SamChou05/AFQ-Insight-Autoencoder-Plotting) - Visualization tools for results
- [pyAFQ](https://github.com/yeatmanlab/pyAFQ)

## Acknowledgments

Thanks to the UW Neuroinformatics R&D Group for research support!

## License

This project is licensed under the MIT License:

```
MIT License

Copyright (c) 2024 AFQ-Insight Autoencoder Experiments Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.