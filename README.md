# 🧬 GeneCell3: Multi-Label Protein Function Prediction

This project focuses on predicting protein functions (specifically **cellular components**) using **multi-label deep learning models**. The data is sourced from [UniProt](https://www.uniprot.org/), a leading protein sequence and functional information database.

We developed two models:

- 🧪 **Base Model** – uses only protein sequences.
- 🌐 **Enhanced Model** – uses both sequences and subcellular location descriptions.

---

## 📁 Dataset

The dataset was manually curated from [UniProt](https://www.uniprot.org/) and contains the following features:

- **Sequence**: Amino acid sequence of the protein.
- **Subcellular location [CC]**: Natural language description of where the protein is located in the cell.
- **Gene Ontology (cellular component)**: Target labels (multi-label) indicating where the protein functions within a cell.

---

## 🧼 Preprocessing Steps

1. **Reproducibility**: Set random seeds for consistency.
2. **Sequence Cleaning**: Standardize amino acid sequences and replace invalid characters.
3. **Sequence Encoding**: Label encode and pad sequences.
4. **Location Processing**: Clean natural language text using regex.
5. **TF-IDF Vectorization**: Turn location descriptions into numeric features.
6. **Label Encoding**: Convert GO terms to binary vectors using `MultiLabelBinarizer`.

---

## 🧠 Models

### 🔹 Base Model

- Input: Amino acid sequence
- Architecture:
  - Embedding → BiLSTM → Conv1D → BiLSTM → Dense
- Output: Multi-label sigmoid classifier

### 🔸 Enhanced Model

- Inputs: Amino acid sequence + TF-IDF vector of subcellular location
- Architecture:
  - Embedding → BiLSTM → Attention → GlobalMaxPool
  - Dense layers on location input
  - Concatenate → Dense → Output

---

## ⚙️ Training Details

- Loss: Binary Crossentropy
- Optimizer: Adam with gradient clipping
- Class imbalance: Handled using class weights
- Early stopping and learning rate reduction on plateau

---

## 📊 Evaluation Metrics

| Metric               | Base Model | Enhanced Model |
|----------------------|------------|----------------|
| Exact Match Ratio    | 0.00%      | 0.00%          |
| Hamming Loss         | 0.3824     | 0.2273         |
| F1 Micro             | 2.44%      | 2.74%          |
| F1 Macro             | 1.61%      | 2.42%          |
| Jaccard Score        | 1.99%      | 1.70%          |

> **Note**: Protein function prediction is a complex task, especially with sparse and overlapping multi-labels. The enhanced model still shows a consistent improvement across most metrics.

---

## 🧠 Interpretation Guide

- **Hamming Loss**: Lower is better — how many labels were incorrectly predicted.
- **F1 Micro**: Weighted average, good for class imbalance.
- **F1 Macro**: Treats all classes equally — lower for imbalanced data.
- **Jaccard Score**: Measures overlap between prediction and true labels.
- **Exact Match Ratio**: How often the entire label set was predicted correctly (very strict).

---

## 📚 How to Run

1. Install dependencies:
```bash
pip install -r requirements.txt
```

2. Add your dataset as `Small_Data.xlsx`.

3. Run the notebook or script containing the model training code.

---

## 🔐 Licensing and Credits

- **Dataset Source**: [UniProt](https://www.uniprot.org/)
- This project is intended for **educational and research purposes only**.
