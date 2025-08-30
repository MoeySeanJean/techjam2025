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

We considered all metadata of the reviews. However, it has no correlation to the relevancy of the reviews. For example, a 5 star review has no indication about the relevancy of the review. Thus, we decided to drop them.
<br><br>

3. **Extracting Features**

We considered all metadata of the reviews. However, it has no correlation to the relevancy of the reviews. For example, a 5 star review has no indication about the relevancy of the review. Thus, we decided to drop them.
<br><br>

4. **Modeling**

Our solution consists of BERT classification. We classify the reviews into clean or flagged. We train the model based on GPT-5 labelled data from UCSD. The training data is cleaned by having equal amounts of clean and flagged reviews.
<br><br>

5. **Policy Module**

Our solution will consider other languages as input. We will call Google Translate API to translate before feeding into BERT for classification. If the reviews have no text, we consider it as flagged.
<br><br>

6. **Testing**

We test our solution on both kaggle dataset (see [setting up](#setting-up)) as well as manually scrapped and labelled data from Google Maps which is based in Singapore. This is to test our model for global context.
<br><br>

7. **Pipeline**

We provide a full pipeline of our solution (see [pipeline](#pipeline)).
<br><br>

## Evaluation

### Findings

1. Kaggle dataset
```
Accuracy:  0.9127
Precision: 0.9828
Recall:    0.9127
F1 Score:  0.9448
Class-wise metrics:
'clean' -> Precision: 0.9930, Recall: 0.9182, F1: 0.9542, Support: 1088
'flagged' -> Precision: 0.0532, Recall: 0.4167, F1: 0.0943, Support: 12
```
This dataset contains only 12 flagged reviews, but our model can accurately tagged clean reviews as 'clean'. Some 'flagged' reviews are clean, but as false positive is less costly than false negative in real world scenarios, this is acceptable.

2. Singapore dataset
```
Accuracy:  0.6476
Precision: 0.9742
Recall:    0.6476
F1 Score:  0.7662
Class-wise metrics:
clean -> Precision: 0.9949, Recall: 0.6430, F1: 0.7812, Support: 3622
flagged -> Precision: 0.0507, Recall: 0.8519, F1: 0.0956, Support: 81
```
This dataset contains 81 flagged reviews. Same reason as above, but due to higher number of flagged reviews, accuracy is lower. However, as discussed above, this is acceptable.

### Reasonings

To push our model to its limits, we deliberately scraped Google Reviews from a wide range of Singapore locations, using keywords such as hawker centre, food court, coffee shope, bar, nightclub, karaoke, etc.

This diverse dataset naturally intorudced noise like slang, short forms, irrelevant rants, and inconsistent review styles. By testing under these challenging conditions, we wanted to evaluate how well our model handles the messiness of real-world reviews.

Despite the complexity, our model achieved over ~65% accuracy and ~77% F1. It shows very high precision (97%) for clean reviews, ensuring trustworthy recommendations, but tends to over-flag reviews, trading precision for flagged reviews in favor of higher recall. This highlights both robustness in filtering spam and the challenge of imbalanced data.

### Limitations

1.  **Short Forms & Abbreviations**: Users often write in shorthand or local slang (e.g., “ugwim”), which makes interpretation difficult.

2. **Spam & Promotional Noise**: Repeated ads or copy-paste reviews still appear and distort credibility.

3. **Location-Specific Context**: Some reviews only make sense within a local or cultural context, which complicates classification.

4. **Emoji-Only Reviews**: Posts with only emojis don’t convey meaningful feedback and are flagged as irrelevant by our model.

5. **Non-text reviews**: Posts without text are flagged by our solution, as text should be main source of reviews and reviews without text are not representative of the actual reviews.

### Recommendations

To improve the performance, we can implement multi-model solution to tackle the multimodality of reviews. We can also get a better and larger training data to improve model performance.

## Pipeline

Our pipeline is in `pipeline.ipynb`. Given a list of any real-world review, our pipeline will output 'clean' or 'flagged'.

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
2. `labelling.ipynb`
   - Calls OpenAI API for GPT-5 labelling task
   - Labels our training data
3. `data_cleaning.ipynb`
   - Prepare the data for training
   - Match the amount of flagged and clean reviews in dataset
4. `training.ipynb`
   - Train the model
   - Test the model with few examples
5. `testing.ipynb`
   - Evaluate the model on kaggle or manual datasets
6. `pipeline.ipynb`
   - Final solution pipeline
7. `./data`
   - Cleaned and labeled data used for testing and training
   - Testing data scrapped and labelled manually
   - Trained model

## Contributions

1. Moey Sean Jean
2. Lim Fang Jun
3. Angel Bu Tong Mei
4. Gan Wei Siang
5. Dexter Tan Yi Jia
