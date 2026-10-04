# Model Files

The trained IndoBERT models are not stored directly in this GitHub repository because the model weights are large.

The trained models are stored separately on Google Drive.

## 🤖 Available Models

### 1. Lexicon → IndoBERT

A sentiment classification model trained using sentiment labels generated from the **InSet Indonesian Sentiment Lexicon** and fine-tuned with IndoBERT.

**Evaluation:**

* Accuracy: **89.55%**
* Precision: **89.86%**
* Recall: **89.55%**
* F1 Score: **89.62%**

**Model files:**

```text
config.json
tokenizer.json
tokenizer_config.json
model.safetensors
```

**Google Drive:**

[Download Lexicon → IndoBERT Model](https://drive.google.com/drive/folders/1ASY0LaUhNQIGVN2o8Am8rm1fT2bRWcKe?usp=drive_link)

---

### 2. SmSA → IndoBERT

A sentiment classification model fine-tuned using the **SmSA dataset** for three-class Indonesian sentiment classification.

**Evaluation:**

* Accuracy: **92.00%**
* Precision: **92.41%**
* Recall: **92.00%**
* F1 Score: **91.68%**

**Model files:**

```text
config.json
tokenizer.json
tokenizer_config.json
model.safetensors
```

**Google Drive:**

[Download SmSA → IndoBERT Model](https://drive.google.com/drive/folders/1ASY0LaUhNQIGVN2o8Am8rm1fT2bRWcKe?usp=drive_link)

---

## ⚠️ Important

The Google Drive links point to the **model folder**, not to a single weight file.

The complete model package should contain the model configuration, tokenizer files, and `model.safetensors`. Do not download only `model.safetensors` when the goal is to load the model directly with Hugging Face Transformers.

The repository intentionally excludes model weights and training checkpoints from Git.

## 🔗 Model Architecture

Both models use:

```text
indobenchmark/indobert-base-p1
```

with three sentiment labels:

```text
0 = negatif
1 = netral
2 = positif
```

## 📌 Storage Strategy

```text
GitHub
├── Source notebook
├── Dataset / analysis data
├── Reports
├── Requirements
└── Documentation

Google Drive
├── Lexicon → IndoBERT
└── SmSA → IndoBERT
```

The model files may later be migrated to a dedicated model repository such as Hugging Face Hub when a more permanent model distribution mechanism is required.
