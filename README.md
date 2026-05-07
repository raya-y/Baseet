![Baseet Overview](Bayyin%20%26%20Baseet%20Logo.png)

# Baseet: A Multi-Level Dataset and Hybrid Framework for Arabic Text Simplification

This repository contains the dataset construction pipeline, simplification models, and evaluation code for **Baseet** (بَسيط), a multi-level Arabic text simplification dataset and framework. The work introduces a parallel corpus of 34,294 original Arabic sentences, each paired with three simplified versions, along with a hybrid simplification approach that combines lexical and sequence-to-sequence models.

The aim is to make Arabic text more accessible to readers of varying proficiency levels while preserving the original meaning of the source.

## Overview

Arabic text simplification is still under-resourced compared to English, mostly because there are very few large parallel datasets and most existing models do not handle Arabic morphology and lexical variation well. Baseet addresses both issues. We built a balanced dataset by combining two strong Arabic resources, generated three controlled simplification levels per sentence using a multilingual LLM, and then trained and evaluated five different simplification models on the result.

The five models we explored are:

1. A lexical simplification model based on ArabicBERT
2. AraBART fine-tuned with level-control tokens
3. AraT5v2 fine-tuned with level-control tokens
4. A hybrid pipeline combining AraBART with the lexical model
5. A hybrid pipeline combining AraT5v2 with the lexical model

The hybrid AraT5v2 model performed best across all three simplification levels.

## Repository Structure

```
## Repository Structure

```text
baseet/
├── README.md
├── Bayyin & Baseet Logo.png
├── dataset/
│   ├── bayyin-llm-simplification-part1.ipynb
│   ├── bayyin-llm-simplification-part2.ipynb
│   ├── samer-llm-simplification-part1.ipynb
│   ├── samer-llm-simplification-part2.ipynb
│   └── clean_dataset.ipynb
├── models/
│   ├── Arat5v2_simplification.ipynb
│   ├── arabart-simplification.ipynb
│   ├── lexical-simplification.ipynb
│   ├── hybrid-AraBART-simplification.ipynb
│   └── hybrid-arat5v2-simplification.ipynb
└── evaluation/
    ├── Baseet_Survey_Anonymized.csv
    ├── hybrid-models-simplification-evaluation.ipynb
    ├── lexical-model-simplification-evaluation.ipynb
    └── predictions/
        └── generated model predictions are available through the Google Drive link     └── survey_results.xlsx           # expert annotator ratings
```
## Dataset

Baseet contains **34,294 original sentences** and **102,882 generated samples** (three simplification levels per source). Originals come from the **Bayyin Dataset** (20,131 sentences from BAREC, the Arabic E-Book Corpus, and DARES; only the two hardest readability levels) and the **SAMER Corpus** (14,163 sentences).
 
Three target levels were defined: **L1** (strong, child-friendly), **L2** (moderate, for beginners), and **L3** (mild, keeping formal Arabic).
 
Simplifications were generated with **Aya-8B** via Ollama, then filtered: identical-to-original sentences, outputs longer than 2.5x the source, sentences with three or fewer words, pairs below 0.70 semantic similarity, and duplicates were all removed.
 
## Models
 
- **Lexical (ArabicBERT)** — word-level substitution. Complex words are flagged against an Arabic frequency list, masked, and replaced using ArabicBERT predictions filtered by lemma, POS, and frequency.
- **AraBART / AraT5v2** — fine-tuned with a level-control token (`[L_1]`, `[L_2]`, `[L_3]`) prepended to the input.
- **Hybrid** — seq2seq output passed through the lexical module for a final pass on remaining complex words.

## Results

Automatic evaluation:

| Model | Level | SARI | BLEU | BERT-F1 |
|-------|-------|------|------|---------|
| Lexical (ArabicBERT) | L1 / L2 / L3 | 17.40 / 22.59 / 24.81 | 7.93 / 18.34 / 23.78 | 78.27 / 82.46 / 84.73 |
| AraBART | L1 / L2 / L3 | 41.72 / 44.42 / 45.18 | 13.55 / 24.91 / 30.10 | 81.40 / 85.13 / 86.78 |
| AraT5v2 | L1 / L2 / L3 | 43.84 / 46.13 / 45.96 | 14.79 / 26.26 / 31.13 | 81.79 / 85.47 / 87.01 |
| Hybrid (AraBART + Lexical) | L1 / L2 / L3 | 52.40 / 52.55 / 51.62 | 14.99 / 24.67 / 27.81 | 81.63 / 84.79 / 85.85 |
| **Hybrid (AraT5v2 + Lexical)** | **L1 / L2 / L3** | **54.36 / 54.81 / 53.56** | **16.59 / 26.95 / 29.86** | **82.14 / 85.38 / 86.37** |
 
The best model (Hybrid AraT5v2) was rated by 32 Arabic experts on Meaning Preservation, Grammaticality, and Simplicity. All criteria averaged above 87%, with L1 reaching 89.33% on simplicity.


## Predictions

The generated predictions for each model can be found here: [models predictions](https://drive.google.com/drive/folders/1HynWxeKXdbXMi5amzZE5h3kfaXCXxbOH?usp=drive_link)

## Installation

Tested with Python 3.10.

```bash
git clone https://github.com/<your-username>/baseet.git
cd baseet
pip install -r requirements.txt
```

For the data generation step, you also need [Ollama](https://ollama.com/) running locally with the Aya model pulled:

```bash
ollama pull aya:8b
```

For Arabic morphological analysis, install the CAMeL Tools data:

```bash
camel_data -i all
```



## Acknowledgments

This work builds on several open resources without which it would not have been possible: the Bayyin readability corpus, the SAMER simplification corpus, the BAREC corpus, the DARES corpus, the Arabic E-Book Corpus, the Aya model from Cohere for AI, AraBART, AraT5v2, ArabicBERT, and CAMeL Tools.

## Contributors & contact
This project is submitted for the fulfillment of the requirements for the graduation project at the University of Jeddah. For questions about reproducing results or data access, open an issue on the repository or contact the repository owner.

**Contributors:**
* Sarah F. Alhalees (2219288) 
* Nagham A. Alshbrawi (2219273)
* Raya Y. Abu Aljamal (2310903) 
* Fatimah M. Alsinan (2310303) 
* Feryal E. Jadallah (2311180) 
* Bayan Z. Barmeem (2219206)

## License

The code in this repository is released under the MIT License. The dataset is released under the licenses of its underlying sources and is intended for academic research only.
