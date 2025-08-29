# TechJam 2025
## Setting up
1. Install [conda](https://www.anaconda.com/download).
2. Create and activate conda environment.
```bash
conda create -n techjam2025
conda activate techjam2025
```
3. Install CUDA (**required for training model**).
```bash
conda install conda install cuda -c nvidia/label/cuda-12.9.0 -c nvidia/label/cuda-12.9.1
```
4. Create HuggingFace access token and OpenAI API Key in `.env` file.
```
HF_TOKEN=[your_token_here]
OPENAI_API_KEY=[your_key_here]
```
5. Download data from [Kaggle](https://www.kaggle.com/datasets/denizbilginn/google-maps-restaurant-reviews) and [UCSD](https://mcauleylab.ucsd.edu/public_datasets/gdrive/googlelocal/). Place the dwonloaded dataset in `./data` directory.