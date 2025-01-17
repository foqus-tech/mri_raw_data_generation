# Deep learning based raw data generation

This repository is the code to replicate our experiments presented in the publication "Deep-learning-based image reconstruction with limited data: Generating synthetic raw data using deep learning" (under review).

The trained generation models and two small datasets are provided, such that the following interactive demonstrations can be run without training anything yourself.

Replicating our experiments will require substantial hardware and computational resources, see below!

## Interactive demonstration (no installation required)

You can run an interactive demonstration of the trained raw data generation networks on Google colab here: [Interactive demonstration](https://colab.research.google.com/github/FrankZijlstra/mri_raw_data_generation/blob/main/notebooks/Interactive_demonstration_Google_Colab.ipynb)

On the notebook, simply select 'Runtime -> Run All' and after a little while the visualization will show. Click (do not drag) the sliders to change the latent space coordinates.

Note that a GPU runtime is necessary and may not always be available.

## Interactive demonstration (local)

First go through the installation steps below to set up the environment. Then run the following to start the demonstration:

```
python interactive_demonstration.py
```

This assumes that a graphical interface is available, and a GPU is necessary as well.

The visualization provides two sets of 4 sliders at the bottom which can be used to set the latent space coordinates for phase (top set of sliders) and coil sensitivities (bottom set). The display can be switched between showing raw data, coil sensitivity maps, phase maps, and the source magnitude data. It can also be switched between real and generated data. Finally, the latent space coordinates can be fit to the real data with the 'Fit latent' button.

## Requirements for training

The following are needed to train and replicate our results:
- The FastMRI brain dataset (brain_multicoil_train and brain_multicoil_val). Obtain it here: https://fastmri.med.nyu.edu/. The total unpacked size is around 2.2 TB!
  - If you have access to the individual HDF5 files, the list of files used in our experiments is available in `dataset.txt`.
- ~70 GB disk space for the training dataset.
- A NVIDIA GPU with at least 8GB GPU RAM.
- 20+ GB RAM for the largest experiments

## Installation

First, install Anaconda from here: https://www.anaconda.com/download

Clone this repository, open a terminal/command prompt, and navigate the repository. Run the following command to install environment with the required Python packages:

```
conda env create --file=environment.yml
```

Then activate the environment:

```
conda activate raw_data_generation
```

## Creating the dataset

Edit `create_dataset.py` such that the paths at the start of the file point to the directories where your FastMRI dataset is unpacked.

Then run the following command to create the training dataset in HDF5 format, which will contain raw data, reconstructed complex images, and complex coil sensitivity maps:

```
python create_dataset.py
```

This will take about 2 hours and will place the data in `./data/fastmri_t1post`.

## Training the phase/CSM generation networks

Run the following script to train the phase and coil sensitivity generation networks:

```
./run_generation.sh
```

This will take about 10 hours on an RTX A6000 GPU.

## Running the reconstruction experiments

Run the following script to train and evaluate all reconstruction experiments (20, 50, 100, 160 scans, baseline/synthetic, with/without data augmentation, 16 experiments total):

```
./run_reconstruction.sh
```

Each experiment will take about 11 hours on an RTX A6000 GPU, so the total time will be just over a week! Note that in our study we repeated the experiments 5 times.

## Recreating figures

- Coming soon
