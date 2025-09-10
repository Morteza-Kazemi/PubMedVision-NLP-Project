# PubMedVision NLP Analysis

**NLP Course Project - Politecnico di Milano**

By: Morteza Kazemi (Preliminary Analysis, Clustering, Learning Embeddings), Hamidreza Saffari (LLM Evaluation), Seyed Taha Mousavi Jafarkolaei (ML Models), Mohadeseh Heravi (PubMedBERT Fine-Tuning)

---

## Overview

This project analyzes the **[PubMedVision Medical VQA dataset](https://huggingface.co/datasets/FreedomIntelligence/PubMedVision)** from a **natural language perspective**, focusing on the *questions and answers* rather than the images.  

We explore linguistic structure, semantic patterns, and modality classification using both **traditional NLP methods** and **modern large language models (LLMs)**. Our goal is to better understand how language in medical VQA tasks reflects modality-specific knowledge and to evaluate the effectiveness of different modeling approaches.  

---

## Project Structure

### 1. Preliminary Analysis
- Dataset exploration (size, statistics, text length distributions).  
- Vocabulary study: frequency, stopword removal, per-document vocab size.  
- Text cleaning: keep Greek & Latin characters, remove rare Chinese words.  

#### 1.2 Clustering with K-Means
- Cluster question vectors to recover semantic groupings.  
- Findings: clusters align with imaging modalities (CT, MRI, X-ray) and linguistic functions (concern, inquiry, interpretation).  

#### 1.3 Word Embeddings
- Trained **Word2Vec** on answer text.  
- Explored semantic structure:  
  - PCA & 3D visualizations of embeddings.  
  - Medical analogies (e.g., *tumor + therapy – cancer → radiosurgical / chemotherapy*).  
  - Observed meaningful clustering of both medical and general terms.  

---

### 2. Modality Classification
Task: Predict imaging modality (e.g., CT, MRI, Endoscopy) from the **answer text**.  

- **Classical models such as LinearSVC**  
- **Deep models such as LSTMs**  
- **Transformers: fine-tuning PubMedBERT** 

---

### 3. LLM Evaluation
Evaluated 4 LLMs using the Hugging Face Inference API:  
- **Mistral-7B-Instruct-v0.3**  
- **Llama-3.1-8B-Instruct**  
- **Qwen2.5-7B-Instruct**  
- **Claude-3-7-Sonnet-20250219**  

#### Methodology
- **Zero-shot**: classify without examples.  
- **Few-shot**: classify with provided exemplars.  
- Includes error handling, reproducibility, and rate limiting.
- The system takes the **answer text** as input and asks the models to identify the imaging modality from a predefined set of 11 categories.

#### Key Findings
- **Claude-3 Sonnet**: consistently best (accuracy 0.68, F1 0.66).  
- **PubMedBERT**: competitive with Claude, highlighting strength of domain-specific fine-tuning.  
- **Mistral**: improved with few-shot prompting.  
- **Llama-3.1**: potential position bias in zero-shot → major improvements in few-shot.  
- **Qwen2.5**: stable but underperforms in few-shot compared to zero-shot.  

---

## Results


| Model                        | Prompt Type | Accuracy | F1-score (Macro) | F1-score (Weighted) |
| ---------------------------- | --------- | -------- | ---------------- | ------------------- |
| **claude-3-7-sonnet** | Few Shot  | **0.68** | **0.66**         | **0.66**            |
| **claude-3-7-sonnet** | Zero Shot | **0.68** | 0.65             | 0.65                |
| **PubMedBERT**               | N/A       | **0.68** | 0.64             | 0.65                |
| **LinearSVC**                | N/A       | 0.60     | 0.54             | 0.56                |
| **Mistral-7B-Instruct-v0.3** | Few Shot  | 0.56     | 0.52             | 0.52                |
| **Mistral-7B-Instruct-v0.3** | Zero Shot | 0.54     | 0.49             | 0.49                |
| **Qwen2.5-7B-Instruct**      | Zero Shot | 0.52     | 0.49             | 0.48                |
| **Llama-3.1-8B-Instruct**    | Few Shot  | 0.51     | 0.46             | 0.46                |
| **Qwen2.5-7B-Instruct**      | Few Shot  | 0.48     | 0.43             | 0.44                |
| **Llama-3.1-8B-Instruct**    | Zero Shot | 0.10     | 0.09             | 0.10                |

![Confusion Matrix](https://drive.google.com/uc?export=view&id=1r5SaPti5oT2OrxNQTRw7OLHWqncOB_s0)
