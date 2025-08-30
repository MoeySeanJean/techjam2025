# TechJam 2025

## Problem Statement
The goal of this project is to build an ML-powered system for evaluating Google location reviews. The system focuses on three main aspects:

1. **Review Quality**
   - Detect and filter out spam, advertisements, and irrelevant content.
   - Identify low-quality reviews such as rants or complaints from users who likely never visited the location.
2. **Review Relevancy**
   - Assess whether a review’s content is genuinely related to the specific location being reviewed.
3. **Enforce Policy**
   - Automatically flag reviews that violate platform guidelines, such as:
     - Containing advertisements or promotional material.
     - Discussing unrelated topics.
     - Posting irrelevant rants without evidence of a real visit.

This system ensures that reviews remain trustworthy, relevant, and useful for both businesses and users. This system is important for 3 stakeholders:

1. **Users**
   - Increases trust in location-based reviews, leading to better decision-making.
2. **Businesses**
   - Ensures fair representation and reduces the impact of malicious or irrelevant reviews.
3. **Platforms**
   - Automates moderation, reduces manual workload, and enhances platform credibility.

## Solution
Our proposed system addresses the challenge by constructing a structured pipeline that integrates pseudo-labeling, supervised model training, and policy enforcement. The approach is designed to be scalable and adaptable, ensuring that reviews are evaluated not only for their linguistic content but also for their compliance with platform guidelines.

1. **Data collection**

We used data from kaggle, UCSD and Google Maps reviews. Kaggle and UCSD dataset is mentioned in [setting up](#setting-up). These data is labelled using GPT-5 from OpenAI. Google Maps reviews are scrapped and labelled manually.

2. **Data analysis**

As Google Maps reviews are multimodal (text, video, pictures), we chose to only filter reviews with text reviews as a good reviews often contain text and a irrelevant review often contains no text. Due to the nature of video and pictures being supplementary to the text, we will only classify the reviews based on purely text.

3. **Extracting Features**

We considered all metadata of the reviews. However, it has no correlation to the relevancy of the reviews. For example, a 5 star review has no indication about the relevancy of the review.
Thus, we decided to drop them.

4. **Modeling**

Our solution consists of BERT classification. We classify the reviews into clean or flagged. We train the model based on GPT-5 labelled data from UCSD.

5. **Policy Module**

Our solution will consider other languages as input. We will call Google Translate API to translate before feeding into BERT for classification.

6. **Testing**

We test our solution on manually scrapped and labelled data from Google Maps. Location is based in Singapore. This is to test our model for global context.

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

## Using this repo

1. `llm.ipynb`
- Contains an alternative solution with LLM
- Discarded due to expensive and slow inference

2. `labelling.ipynb`
- Calls OpenAI API for GPT-5 labelling task
- Labels our training data

3. `data_cleaning.ipynb`
- Prepare the data for training
- Match the amount of flagged and clean reviews in dataset

4. `bert.ipynb`
- Train the model
- Test the model with few examples

5. `pipeline.ipynb`
- Final solution pipeline
- Evaluation of solution

6. `./data`
- Contains cleaned and labeled data used for testing and training

## Contributions

1. Moey Sean Jean
2. Lim Fang Jun
3. Angel Bu Tong Mei
4. Gan Wei Siang
5. Dexter Tan Yi Jia