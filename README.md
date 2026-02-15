# CBS Media Monitoring Pipeline

Modernizing the legacy CBS article-linking system by replacing lexical heuristics with Dutch semantic vectors. I rebuilt this to fix "lazy" date-based matching and English-stemming bias found in the original code.

* `preprocess.ipynb` - The data engineering stage. Handles Dutch vectorization using spaCy (`nl_core_news_lg`) to generate 300-dimensional embeddings. Includes the logic for stitching 350,000+ raw files into a unified corpus. Results in `trainset_reconstructed.csv` (the master training set).

* `rf_models.ipynb` - The machine learning stage. Runs A/B tests between the legacy character-matching and the new semantic model. Implements **"Hard Negative" sampling** (same-day unrelated pairs) to force the model to learn actual text features rather than just looking at the calendar. Results in `model_rf_fixed.pkl` and classification diagnostics.

* `network_graph.ipynb` - The auditing stage. Uses a Barnes-Hut physics engine to map how news articles cluster around official CBS reports. Allows for microscopic verification of "family tree" connections to ensure the model isn't just matching on random noise. Results in `family_cluster.html` (interactive network map).

## Professional Disclaimer
To comply with confidentiality agreements, the raw article corpus and internal CSV datasets have been excluded from this public repository. The provided notebooks demonstrate the preprocessing logic, model architecture, and visualization pipelines.

## Reproducibility
* **Python Version:** 3.13
* **Random Seed:** `random_state=42`
```bash
pip install -r requirements.txt
python -m spacy download nl_core_news_lg
```
