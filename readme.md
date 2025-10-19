# Search and Information Retrieval Project (Fall 2024)

This repository contains the implementation of a **Search and Information Retrieval (SIR)** system using Python.  
It is structured according to the assignment requirements and demonstrates multiple retrieval models, query expansion, and evaluation metrics on a subset of the **20 Newsgroups dataset**.
---
## 📂 Project Structure

```

├── notebooks/
│   └── ITE_SIR_HW.ipynb        # Main Jupyter Notebook (all parts implemented step by step)
├── data/                        # Data automatically fetched from scikit-learn (20 Newsgroups subset)
├── README.md                    # This file


## ⚙️ Requirements

To run this project, you need:

- Python 3.8+
- Jupyter Notebook (if running interactively)
- Required packages:
  ```bash
  pip install nltk scikit-learn tabulate matplotlib numpy
````

Additionally, you must download NLTK resources:

```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
nltk.download('wordnet')
nltk.download('omw-1.4')
```

---

## 📊 Dataset

We use the **20 Newsgroups dataset** provided by scikit-learn, restricted to 3 categories:

* `sci.space`
* `rec.sport.baseball`
* `comp.graphics`

This dataset contains raw newsgroup posts that serve as our **document collection**.
Each document has an associated category label, which we use for **evaluation ground truth**.

---

## 🧩 Project Parts

### Part 1: Preprocessing

* Loaded raw text documents from the dataset.
* Applied preprocessing pipeline:

  * Lowercasing
  * Removal of punctuation and digits
  * Tokenization
  * Stopword removal
  * Removal of short words (length < 2)
  * Stemming (Porter Stemmer)
  * Lemmatization
* Verified preprocessing with example text.
* Produced a cleaned list of documents (`preprocessed_docs`).

### Part 2: Boolean Model

* Built an **Inverted Index** mapping terms → document IDs.
* Implemented Boolean retrieval supporting:

  * `AND`, `OR`, `NOT`
  * Implicit AND between terms
* Ran example queries:

  * `space AND shuttle AND mission`
  * `baseball AND stadium AND team`
  * `graphics OR computer`
  * `NOT space`
  * `nasa`
* Displayed results in tabular form.

### Part 3: Extended Boolean Model (p-norm model)

* Implemented the **p-norm model**:

  * AND operator:
    [
    S_{AND}(d) = \left(\frac{\sum w_{i,d}^p}{n}\right)^{1/p}
    ]
  * OR operator:
    [
    S_{OR}(d) = 1 - \left(\frac{\sum (1-w_{i,d})^p}{n}\right)^{1/p}
    ]
* Compared Boolean vs Extended Boolean retrieval.
* Visualized how document scores change for different values of `p` (1, 2, 10, 50).
* Verified that Boolean retrieval is recovered in the limit as `p → ∞`.

### Part 4: Indexing and TF–IDF

* Constructed the **term-document matrix**:

  * Term Frequency (TF)
  * Document Frequency (DF)
  * Inverse Document Frequency (IDF, using log base 2)
  * TF–IDF weighting
* Displayed detailed tables:

  * Each row shows: raw term, processed term, DF, IDF, and per-document (TF, TF–IDF).
* Provided both **term-centric** and **document-centric** views.

### Part 5: Vector Space Model (VSM)

* Represented documents and queries as **TF–IDF vectors**.
* Computed **cosine similarity** between query vectors and document vectors.
* Retrieved and ranked top-k documents for queries:

  * `space mission shuttle`
  * `baseball team stadium`
  * `computer graphics image`
* Visualizations:

  * Bar charts of top-k similarities for each query.
  * Side-by-side plots showing ranked documents and their cosine scores.
* Displayed query vector TF–IDF weights to show how queries are mapped into the vector space.

### Part 6: Query Expansion (Pseudo Relevance Feedback)

* Implemented pseudo-relevance feedback:

  * Took top-N retrieved documents from VSM.
  * Extracted highest-weighted TF–IDF terms not in the original query.
  * Expanded the query with top-m terms.
* Compared retrieval results **before vs after expansion**.
* Visualized score differences using **side-by-side bar charts**.
* Applied expansion to all three example queries.

### Part 7: Evaluation

* Evaluated retrieval performance using **ground truth categories**.
* Metrics per query and model:

  * Precision@k
  * Recall@k
  * F1-score@k
  * Average Precision (AP@k)
* Computed **Mean Average Precision (MAP@k)** across all queries.
* Models evaluated:

  * Boolean
  * Extended Boolean
  * VSM
  * VSM + Expansion
* Visualizations:

  * Grouped bar charts (Precision, Recall, F1 per query per model).
  * Precision–Recall curves for queries (VSM vs VSM+Expansion).

---

## 📈 Example Outputs

* **TF–IDF Table (excerpt):**

| Raw Term | Processed Term | DF  | IDF (log2) | Doc0             | Doc1             | ... |
| -------- | -------------- | --- | ---------- | ---------------- | ---------------- | --- |
| space    | space          | 120 | 1.651      | TF=4, TF-IDF=6.6 | TF=2, TF-IDF=3.3 | ... |

* **Evaluation Results (Precision/Recall/F1/AP@10):**

| Query                 | Category  | Boolean            | Extended Boolean   | VSM                | VSM+Expansion      |
| --------------------- | --------- | ------------------ | ------------------ | ------------------ | ------------------ |
| space mission shuttle | sci.space | 0.7/0.11/0.19/0.22 | 0.8/0.12/0.21/0.24 | 0.8/0.13/0.22/0.26 | 0.9/0.14/0.24/0.29 |

* **MAP@10 Summary:**

| Model            | MAP@10 |
| ---------------- | ------ |
| Boolean          | 0.20   |
| Extended Boolean | 0.23   |
| VSM              | 0.27   |
| VSM+Expansion    | 0.30   |

---

## 🚀 How to Run

1. Clone the repository:

   ```bash
   git clone https://github.com/khaledkassem98/Information_Retrieval_Project.git
   cd ITE_SIR_HW
   ```

2. Open the Jupyter Notebook:

   ```bash
   jupyter notebook notebooks/ITE_SIR_HW.ipynb
   ```

3. Run all cells in order:

   * Part 1 → Part 7
     Each part builds on the previous one.

---

## 📌 Key Insights

* Preprocessing reduces vocabulary size and normalizes terms.
* Boolean retrieval is strict but lacks ranking.
* Extended Boolean provides soft matching, controlled by parameter `p`.
* TF–IDF weighting improves discrimination of terms.
* VSM with cosine similarity provides effective ranked retrieval.
* Query expansion improves recall at the cost of some precision.
* MAP and PR-curves confirm VSM + expansion achieves best overall performance.

---

## 👩‍💻 Authors

* Developed as part of **Search and Information Retrieval (Fall 2024)** course project.
* Team: *[khaled kassem]*

---

## 📜 License

This project is provided under the MIT License.
Feel free to use, adapt, and extend with attribution.

```

---

