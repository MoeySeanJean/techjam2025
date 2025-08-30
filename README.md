# TechJam 2025


## Problem Overview
The goal of this project is to build an **ML-powered system** to evaluate **Google location reviews**. Our system focuses on three key aspects:

1. **Gauge review Quality**
   - Detect and filter out spam, advertisements, and irrelevant content.
   - Identify low-quality reviews such as rants or complaints from users who likely never visited the location.
<br><br>

2. **Review Relevancy**
   - Determine whether the content of a review is genuinely related to the location being reviewed.
<br><br>

3. **Enforce Policy** 
   - Automatically flagging or filtering reviews that violate platform guidelines:
      - No advertisements or promotional content.
      - No irrelevant content (e.g., reviews about unrelated topics).
      - No unverifiable rants or complaints from non-visitors.
<br><br>   

This system improves the **trustworthiness of reviews** for three main stakeholders:

- **Users**: More reliable reviews enable better decision-making.
<br><br>

- **Businesses**: Fairer representation with reduced impact of malicious or irrelevant reviews.
<br><br>

- **Platforms**: Automated moderation lowers manual workload and enhances credibility.
<br><br>

## Our Solution
We developed a structured pipeline that integrates **pseudo-labeling**, **supervised training** and **multilingual support**. This design ensures scalability and adaptability while aligning with platform guidelines.

1. **Data collection**

We used data from kaggle, UCSD and Google Maps reviews. Kaggle and UCSD dataset is mentioned in [setting up](#setting-up). These data is labelled using GPT-5 from OpenAI. Google Maps reviews are scrapped and labelled manually.
<br><br>

2. **Data analysis**

As Google Maps reviews are multimodal (text, video, pictures), we chose to only filter reviews with text reviews as a good reviews often contain text and a irrelevant review often contains no text. Due to the nature of video and pictures being supplementary to the text, we will only classify the reviews based on purely text.
<br><br>

3. **Extracting Features**

We considered all metadata of the reviews. However, it has no correlation to the relevancy of the reviews. For example, a 5 star review has no indication about the relevancy of the review.
Thus, we decided to drop them.
<br><br>

4. **Modeling**

Our solution consists of BERT classification. We classify the reviews into clean or flagged. We train the model based on GPT-5 labelled data from UCSD.
<br><br>

5. **Policy Module**

Our solution will consider other languages as input. We will call Google Translate API to translate before feeding into BERT for classification.
<br><br>

6. **Testing**

We test our solution on manually scrapped and labelled data from Google Maps. Location is based in Singapore. This is to test our model for global context.
<br><br>

## Setting up
1. Install [conda](https://www.anaconda.com/download).
2. Create and activate conda environment.
```bash
conda create -n techjam2025
conda activate techjam2025
```
3. Install CUDA (**required for training and evaluating model**).
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
<br><br>
2. `labelling.ipynb`
   - Calls OpenAI API for GPT-5 labelling task
   - Labels our training data
<br><br>
3. `data_cleaning.ipynb`
   - Prepare the data for training
   - Match the amount of flagged and clean reviews in dataset

4. `bert.ipynb`
   - Train the model
   - Test the model with few examples
<br><br>
5. `pipeline.ipynb`
   - Final solution pipeline
   - Evaluation of solution
<br><br>
6. `./data`
   - Cleaned and labeled data used for testing and training
   - Testing data scrapped and labelled manually
   - Trained model
<br><br>

## Contributions

1. Moey Sean Jean
2. Lim Fang Jun
3. Angel Bu Tong Mei
4. Gan Wei Siang
5. Dexter Tan Yi Jia