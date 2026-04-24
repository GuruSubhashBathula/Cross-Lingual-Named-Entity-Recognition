# Cross-lingual Named Entity Recognition: English → Telugu

This repository contains code and data for evaluating cross-lingual transfer of NER models (DistilBERT, mBERT, MuRIL) from English (CoNLL-2003) to Telugu, with out-of-domain evaluation on WNUT-17 and a manually annotated Telugu-250 test set.

The code reproduces all results reported in the accompanying project report:
- English in-domain (CoNLL-2003) performance
- English out-of-domain (WNUT-17) performance
- Telugu supervised evaluation on the Telugu-250 test set (with inter-annotator agreement)
## Environment and Installation

Tested with:
- Python 3.10 / 3.11
- Ubuntu / Google Colab environment
## To Clone Repository
``` bash
git clone https://github.com/GuruSubhashBathula/Cross-Lingual-Named-Entity-Recognition.git
cd Cross-Lingual-Named-Entity-Recognition

# Option A: pure venv + pip
python -m venv .venv
source .venv/bin/activate      # on Windows: .venv\Scripts\activate
pip install -r requirements.txt

# Option B: conda env
conda env create -f environment.yml
conda activate cross-lingual-ner
```

Key dependencies:
- `transformers`
- `datasets`
- `seqeval`
- `evaluate`
- `pandas`
- `numpy`
- `matplotlib`
- `scikit-learn`
- `jupyter or use Google Colab`

  ## How to Run and Reproduce Results

There is a single main notebook:

- `Cross-lingual-Named-Entity-Recognition.ipynb`

This notebook is organized into the following sections:

1. **Setup and model loading**  
   - Loads DistilBERT, mBERT, and MuRIL token classification models from Hugging Face.
2. **CoNLL-2003 (English in-domain) evaluation**  
   - Parses the CoNLL test set.
   - Evaluates all three models with `seqeval` F1.
3. **WNUT-17 (English out-of-domain) evaluation**  
   - Downloads the WNUT-17 test file via `wget`.
   - Converts WNUT labels to CoNLL-style PER/ORG/LOC/OTH.
   - Evaluates all three models.
4. **Telugu datasets and evaluation**  
   - Loads the manually annotated `telugu_annotation_sheet_250.csv` and builds Telugu-250 test data.
   - Evaluates DistilBERT, mBERT, and MuRIL on Telugu-250, including:
     - Exact match F1 (seqeval)
     - Token-level partial F1
     - MCC (entity vs O)
     - Per-class F1 (PER/ORG/LOC)
5. **Extended metrics and plots**  
   - Creates result tables and matplotlib plots for:
     - CoNLL vs WNUT vs Telugu F1
     - Per-entity F1 per model on Telugu-250
     - Annotator agreement (Cohen’s kappa, entity count comparison).

### Recommended run order

1. Open `Cross-lingual-Named-Entity-Recognition.ipynb` in Jupyter or Google Colab.
2. Run all cells in order, from top to bottom, **without skipping**.
3. Make sure you have internet access for downloading:
   - Pretrained models from Hugging Face.
   - WNUT-17  test sets from GitHub.
4. The final cells will print:
   - The **final results table** with CoNLL, WNUT-17, and Telugu F1.
   - Domain shift and cross-lingual F1 drops.
   - Extended Telugu metrics and plots.
## File Manifest

- `Cross-lingual-Named-Entity-Recognition.ipynb`  
  Main notebook; runs all experiments and reproduces the reported metrics and plots.

- `data/`
  - `telugu_annotation_sheet_250.csv`  
    Manually annotated Telugu NER test set (250 sentences) with PER/ORG/LOC labels.
  - `telugu_news_text.txt`  
    Source Telugu news text used for examples and qualitative analysis.

- `models/`
  - `README.md`  
    Notes on which Hugging Face model IDs are used; models are downloaded at runtime.

- `requirements.txt` / `environment.yml`  
  Python environment and dependency specification.

- `LICENSE`  
  License for this repository (see below).

- `README.md`  
  This file.

  ## Copyright and Licensing

© 2026 Your Name. All rights reserved.

The code in this repository is released under the MIT License.  
See the `LICENSE` file for details.

Note: External datasets (CoNLL-2003, WNUT-17) are distributed under their respective licenses and are **not** included in this repository. This repo only provides download links and parsing scripts.

## Contact

Author: Bathula Guru Subhash, Ambati Sai Kiran  
Institution: IIT Palakkad, M.Tech Data Science  
Email: 142502014.smail@iitpkd.ac.in, 142502003@smail.iitpkd.ac.in

Feel free to open an issue or contact me by email for questions about the code or results.

## Known Bugs and Issues

- **Internet access required**:  
  The notebook downloads pretrained models and some datasets (e.g., WNUT-17) at runtime. Running offline will fail.

- **Hugging Face token warning**:  
  Colab may show a warning about missing `HFTOKEN`. This is safe to ignore for public models.

- **Seqeval undefined metric warnings**:  
  On some datasets/models (e.g., DistilBERT on Telugu), some entity labels have no predictions or no true samples. `seqeval` will print `UndefinedMetricWarning` and set precision/recall/F1 to 0.0. This is expected and does not affect the final summary table.

- **Dataset availability**:  
  If a dataset URL changes (404), the corresponding evaluation cell will fail. In that case, update the URL in the notebook to the new location (see Troubleshooting below).

## Troubleshooting

- **HTTPError 404 when downloading a dataset**  
  - Make sure the URL in the notebook is still valid.  
  - If it has moved, copy the latest raw URL from the dataset’s GitHub/Hugging Face page into the corresponding `wget` or `read_parquet` call.

- **CUDA / GPU errors**  
  - The notebook forces models to run on CPU to avoid CUDA issues.  
  - If you want to use GPU, modify `DEVICE = torch.device("cpu")` to `"cuda"` and ensure a GPU runtime is available.

- **Seqeval warnings about undefined metrics**  
  - These occur when a label has no predicted or true samples (e.g., a model never predicts ORG in Telugu).  
  - They can safely be ignored; the script already handles this and reports F1 as 0.0.

- **Out-of-memory errors**  
  - If you hit memory limits, reduce `max_length` or batch size in the evaluation loops in the notebook.
 
## Credits and Acknowledgments

This project builds on:

- **Pretrained models**  
  - `distilbert-base-cased` fine-tuned on CoNLL-2003 (Hugging Face)  
  - `bert-base-multilingual-cased` (mBERT)  
  - `google/muril-base-cased`

- **Datasets**  
  - CoNLL-2003 English NER  
  - WNUT-17 Emerging / Rare Entities  
  - Manually annotated Telugu-250 test set created as part of this project.

Thanks to the course instructors and teaching staff at IIT Palakkad for guidance, and to the authors/maintainers of the above datasets and models.

## Datasets and External Repositories

Due to licensing, datasets are **not** included directly. Please obtain them from their official sources:

- **CoNLL-2003 (English NER)**  
  - Official source: (add link to LDC / course mirror as appropriate)

- **WNUT-17 (Emerging and Rare Entities)**  
  - GitHub: https://github.com/leondz/emerging_entities_17

- **Pretrained models on Hugging Face**    
  - mBERT: `bert-base-multilingual-cased`  
  - MuRIL: `google/muril-base-cased`

