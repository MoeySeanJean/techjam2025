# TechJam 2025
## Setting up
1. Install [conda](https://www.anaconda.com/download)
2. Create and activate conda environment
```bash
conda create -n techjam2025
conda activate techjam2025
```
3. Create HuggingFace access token and OpenAI API Key in `.env` file
```
HF_TOKEN=[your_token_here]
OPENAI_API_KEY=[your_key_here]
```
4. Download data from [Kaggle](https://www.kaggle.com/datasets/denizbilginn/google-maps-restaurant-reviews) and [UCSD](https://mcauleylab.ucsd.edu/public_datasets/gdrive/googlelocal/)

## Solution
Our proposed system addresses the challenge by constructing a structured pipeline that integrates pseudo-labeling, supervised model training, and policy enforcement. The approach is designed to be scalable and adaptable, ensuring that reviews are evaluated not only for their linguistic content but also for their compliance with platform guidelines.