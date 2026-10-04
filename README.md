# YouTube Audience Analytics & Sentiment Tracking

*A machine learning and social media analytics project for analyzing audience behavior and public sentiment around the topic **“Board of Peace”** on YouTube.*

## 📌 Overview

This project analyzes thousands of YouTube comments to identify audience sentiment, discussion patterns, and recurring topics related to the **Board of Peace** discussion.

The workflow combines automated YouTube comment extraction, text preprocessing, InSet Lexicon labeling, IndoBERT transfer learning, SmSA-based supervised training, sentiment prediction, and Latent Dirichlet Allocation (LDA) topic modeling.

The objective is to transform unstructured social media conversations into measurable analytical outputs that can support digital content and audience strategy.

## 🎯 Objectives

- Collect YouTube comments dynamically using keyword-based queries.
- Filter comments according to the configured analysis period.
- Clean and normalize Indonesian text data.
- Generate sentiment labels using the InSet Lexicon.
- Train and evaluate IndoBERT-based sentiment classifiers.
- Compare the Lexicon → IndoBERT and SmSA → IndoBERT approaches.
- Classify comments into **negative, neutral, and positive** sentiment.
- Identify recurring topics within each sentiment group.
- Produce visual reports for model performance, sentiment trends, and audience vocabulary.

## 🔄 Analysis Pipeline

```text
YouTube Data API
      │
      ▼
Comment Extraction
      │
      ▼
Date Filtering & Deduplication
      │
      ▼
Text Preprocessing
      │
      ├───────────────────┐
      ▼                   ▼
InSet Lexicon         SmSA Dataset
      │                   │
      ▼                   ▼
Lexicon Labels       Supervised Training
      │                   │
      └─────────┬─────────┘
                ▼
       IndoBERT Classification
                │
                ▼
       YouTube Sentiment Prediction
                │
        ┌───────┼────────┐
        ▼       ▼        ▼
    Wordcloud  Weekly    LDA Topic
               Trend      Modeling
        │       │        │
        └───────┼────────┘
                ▼
        Analytical Reports
```

## 🧠 Methodology

### 1. YouTube Data Extraction

The notebook uses the **YouTube Data API** to search for videos and retrieve comments based on configurable keywords.

Current analysis keywords include:

- `Board of Peace Indonesia`
- `Dewan Perdamaian`
- `Indonesia gabung dengan Board of Peace`
- `Indonesia penengah konflik AS Iran`
- `Dampak perang Amerika Iran Indonesia`

The current configuration allows up to **30 videos per keyword** and targets up to **2,000 comments per keyword**, subject to API availability and quota.

### 2. Time Filtering

The configured analysis period starts from:

```text
2025-01-01
```

Comments older than the configured start date are excluded from the analysis dataset.

### 3. Text Preprocessing

The preprocessing stage prepares YouTube comments for downstream NLP tasks.

The workflow includes cleaning text noise, removing unwanted formatting and non-alphanumeric content, normalizing text, and removing duplicate records.

### 4. Sentiment Classification

Two IndoBERT-based approaches are evaluated.

#### InSet Lexicon → IndoBERT

The InSet Indonesian sentiment lexicon is used to create sentiment labels from cleaned YouTube comments. These labels are then used to train an IndoBERT sequence classification model.

#### SmSA → IndoBERT

The SmSA dataset is used as the supervised training source for a three-class IndoBERT sentiment classifier.

The sentiment labels are:

```text
0 = negatif
1 = netral
2 = positif
```

### 5. Topic Modeling

The project applies **Latent Dirichlet Allocation (LDA)** separately to the sentiment groups.

Current configuration:

- **3 topics** per sentiment group
- **10 representative words** per topic

### 6. Visualization

The pipeline produces:

- Confusion matrices
- Evaluation metric JSON files
- Weekly sentiment trend charts
- Sentiment word clouds
- LDA topic summaries

## 📊 Dataset Snapshot

The executed pipeline produced the following stages:

| Dataset | Rows |
|---|---:|
| `01_youtube_raw_comments.csv` | 10,920 |
| `02_youtube_filtered_2025plus.csv` | 10,915 |
| `03_youtube_cleaned.csv` | 10,842 |
| `04_youtube_lexicon_labeled.csv` | 10,842 |
| `05_youtube_predicted_by_lexicon_indobert.csv` | 10,842 |
| `06_youtube_predicted_by_smsa_indobert.csv` | 10,842 |

The final cleaned dataset contains **10,842 comments** used for the downstream prediction and reporting workflow.

> Raw and processed datasets can contain user-generated text. Before public distribution, review the data for privacy, platform-policy, and redistribution considerations.

## 📈 Model Evaluation

### Lexicon → IndoBERT

| Metric | Score |
|---|---:|
| Accuracy | **89.55%** |
| Precision | **89.86%** |
| Recall | **89.55%** |
| F1 Score | **89.62%** |

### SmSA → IndoBERT

| Metric | Score |
|---|---:|
| Accuracy | **92.00%** |
| Precision | **92.41%** |
| Recall | **92.00%** |
| F1 Score | **91.68%** |

> The two evaluations use different test contexts. The Lexicon → IndoBERT model is evaluated using lexicon-derived labels, while the SmSA → IndoBERT model is evaluated on the SmSA test set. Therefore, these scores should not be interpreted as a strict apples-to-apples benchmark.

## 📊 YouTube Sentiment Prediction

The executed prediction pipeline produced the following class distributions.

### Lexicon → IndoBERT

| Sentiment | Comments |
|---|---:|
| Negative | 8,943 |
| Neutral | 979 |
| Positive | 920 |

### SmSA → IndoBERT

| Sentiment | Comments |
|---|---:|
| Negative | 7,700 |
| Positive | 1,784 |
| Neutral | 1,358 |

The difference between the two distributions highlights the impact that training-label methodology can have on downstream audience sentiment interpretation.

## 📁 Repository Structure

```text
youtube-audience-sentiment-analytics/
├── README.md
├── .gitignore
├── requirements.txt
│
├── datasets/
│   ├── 01_youtube_raw_comments.csv
│   ├── 02_youtube_filtered_2025plus.csv
│   ├── 03_youtube_cleaned.csv
│   ├── 04_youtube_lexicon_labeled.csv
│   ├── 05_youtube_predicted_by_lexicon_indobert.csv
│   └── 06_youtube_predicted_by_smsa_indobert.csv
│
├── models/
│   └── README.md
│
├── notebooks/
│   └── sentiment_analysis_board_of_peace_youtube.ipynb
│
└── reports/
    ├── lexicon_indobert_confusion_matrix.png
    ├── lexicon_indobert_test_metrics.json
    ├── smsa_indobert_confusion_matrix.png
    ├── smsa_indobert_test_metrics.json
    │
    ├── lexicon_indobert/
    │   ├── lexicon_indobert_lda_topics.txt
    │   ├── lexicon_indobert_weekly_trend.png
    │   ├── lexicon_indobert_wordcloud_negatif.png
    │   ├── lexicon_indobert_wordcloud_netral.png
    │   └── lexicon_indobert_wordcloud_positif.png
    │
    └── smsa_indobert/
        ├── smsa_indobert_lda_topics.txt
        ├── smsa_indobert_weekly_trend.png
        ├── smsa_indobert_wordcloud_negatif.png
        ├── smsa_indobert_wordcloud_netral.png
        └── smsa_indobert_wordcloud_positif.png
```

## 🛠️ Technology Stack

- **Python**
- **Google YouTube Data API**
- **Pandas**
- **NumPy**
- **TQDM**
- **Scikit-learn**
- **Matplotlib**
- **Seaborn**
- **WordCloud**
- **PyTorch**
- **Hugging Face Transformers**
- **Hugging Face Datasets**
- **IndoBERT**
- **InSet Lexicon**
- **SmSA Dataset**
- **Latent Dirichlet Allocation (LDA)**
- **Google Colab / Google Drive**

## 📦 Installation

Create a Python environment, then install the dependencies:

```bash
pip install -r requirements.txt
```

The main analysis notebook is located at:

```text
notebooks/sentiment_analysis_board_of_peace_youtube.ipynb
```

The notebook was developed for a GPU-enabled Google Colab environment and uses Google Drive for intermediate data and model storage during execution.

## 🔑 Configuration

The notebook expects a YouTube Data API key.

Use a placeholder in source code:

```python
API_KEY = "YOUR_YOUTUBE_API_KEY"
```

For actual execution, provide the key through a secure environment or secret-management mechanism rather than committing the real key to Git.

The analysis configuration also defines:

```text
Language           : Indonesian
Start date         : 2025-01-01
Max videos/keyword : 30
Target comments    : 2,000 per keyword
IndoBERT model     : indobenchmark/indobert-base-p1
Maximum sequence   : 128 tokens
Number of labels   : 3
```

## 🤗 Model Storage

The trained IndoBERT checkpoints are intentionally **not stored in this Git repository**.

The repository keeps only:

```text
models/
└── README.md
```

This keeps GitHub lightweight while allowing the trained models to be distributed separately.

Recommended hosting strategy:

```text
GitHub
├── Notebook
├── Datasets / sanitized samples
├── Reports
├── Documentation
└── Configuration

Hugging Face
├── Lexicon → IndoBERT model
└── SmSA → IndoBERT model
```

The final model repository URLs can be added to `models/README.md` after the Hugging Face repositories are created.

## 📊 Reports

The `reports/` directory contains the analytical outputs generated by the notebook.

### Model evaluation

- Confusion matrices
- Accuracy
- Precision
- Recall
- F1 score

### Audience analysis

- Weekly sentiment trends
- Positive word cloud
- Neutral word cloud
- Negative word cloud
- LDA topic summaries

## 💼 Analytical / Business Value

This project demonstrates a workflow for converting large-scale social media conversations into structured audience insights.

Potential uses include:

- Audience sentiment monitoring
- Content strategy evaluation
- Identification of recurring concerns
- Detection of positive and negative discussion themes
- Weekly trend monitoring
- Data-driven digital strategy

The outputs are designed to be readable by both technical and non-technical stakeholders through a combination of evaluation metrics and visual reports.

## ⚠️ Limitations

- YouTube Data API collection is constrained by API quota and availability.
- Sentiment quality depends on the quality and domain relevance of training labels.
- Lexicon-derived labels may inherit limitations from the underlying lexicon.
- LDA topics are statistical topic groupings and still require human interpretation.
- The two model evaluations are based on different datasets.
- Raw YouTube comments should be reviewed before public redistribution.

## 🚀 Future Improvements

- Add domain-specific manual annotation for Board of Peace comments.
- Compare additional Indonesian transformer architectures.
- Automate scheduled data collection and report generation.
- Add experiment tracking and model versioning.
- Separate the notebook into reusable Python modules as the project evolves.
- Build an interactive dashboard for non-technical stakeholders.

## 👤 Project Role

**Machine Learning & Data / Social Media Analyst**

Skills demonstrated:

- Social media data extraction
- Data preprocessing
- Natural Language Processing
- Sentiment analysis
- Transformer fine-tuning
- Topic modeling
- Data visualization
- Audience analytics
- Analytical reporting
- Translating model output into actionable insights

## 📅 Project Context

**Analysis:** April 2026  
**Platform:** YouTube  
**Topic:** Board of Peace  
**Primary Language:** Indonesian