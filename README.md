# IMDb-Model

Movie genre classification from titles and plot descriptions using pretrained DistilBERT. Includes single-label and multi-label TensorFlow experiments and a separate PyTorch experiment.

**Repository maintainer:** Aniket Tiwari

Originally developed as a collaborative ITCS 6156 course project. This repository presents the genre-classification code; the unrelated virtual try-on project and original course presentation are outside its scope.

## Notebooks

| File | Purpose | Status |
| --- | --- | --- |
| `notebooks/Code_IMDb.ipynb` | Fine-tune DistilBERT across 27 single-label movie genres; save predictions and model | Completed run recorded in the source project |
| `notebooks/Code_Multilabel.ipynb` | Predict multiple genres using binary cross-entropy; generate per-label confusion matrices and classification metrics | Completed run recorded in the source project |
| `notebooks/experimental/predicting_genre.ipynb` | PyTorch Trainer workflow with class weighting, a stratified split, and cosine scheduling | Experimental; source contains path errors and no completed training results |
| `notebooks/experimental/distilbert_genre_prediction_draft.ipynb` | Earlier data-loading draft | Incomplete |

The notebook code is preserved. Execution outputs, private Colab account metadata, and widget state have been cleared for publication. Historical results are summarized below rather than presented as a fresh execution.

## Setup and data

The notebooks were written for Google Colab and Google Drive. Open a notebook in Colab, install its dependencies, mount Drive, and adjust `main_dir` to the project directory. The original path is `/content/drive/MyDrive/ITCS6156_BERT`; experimental notebooks also contain `/gdrive` paths that need adjustment.

The completed TensorFlow notebooks use pandas, TensorFlow, Hugging Face Transformers, scikit-learn, matplotlib, and seaborn. The PyTorch experiment additionally uses torch, datasets, NumPy, and NLTK. Source notebook logs identify Transformers 4.46.x; a complete environment lockfile was not supplied. Compatibility with current library releases has not been established.

Provide the original datasets in this layout:

```text
ITCS6156_BERT/
  Data/
    train_data.txt
    test_data.txt
    test_data_solution.txt
  Data_Multilabel/
    movies.csv
  Results/
```

Create `Results/` before running. Data, pretrained weights, generated predictions, and TensorBoard logs are excluded from Git. See `data/README.md` for the expected schemas.

## Results from saved project artifacts

The single-label dataset contains 54,214 training records and 54,200 separate test records across 27 genres. The TensorFlow notebook splits the training records 80/20 and fine-tunes for three epochs with batch size 16, maximum sequence length 512, and Adam at learning rate 7e-5.

| Single-label metric | Value | Evidence |
| --- | --- | --- |
| Best recorded validation accuracy | 63.83% | Epoch 1 notebook output |
| Final recorded validation accuracy | 63.32% | Epoch 3 notebook output |
| Saved prediction CSV test accuracy | 63.21% | Independently calculated against supplied test labels |
| Saved prediction CSV weighted F1 | 62.55% | Independently calculated against supplied test labels |
| Saved prediction CSV macro F1 | 44.57% | Independently calculated against supplied test labels |

The CSV evaluation aligned all 54,200 IDs and titles. The saved CSV and notebook preview contain different predictions, so the CSV metrics should not be attributed to a specific checkpoint without further verification. No training was rerun for this repository publication.

The multi-label notebook records a 6,929/1,733 training/validation split, maximum sequence length 256, batch size 16, three epochs, and Adam at learning rate 7e-5.

| Recorded multi-label validation metric | Value |
| --- | --- |
| Weighted precision | 0.77 |
| Weighted recall | 0.56 |
| Weighted F1 | 0.62 |
| Micro F1 | 0.65 |
| Macro F1 | 0.41 |
| Exact-match accuracy | 0.31 |

## Limitations and experiment notes

- Training accuracy rises while validation loss worsens in the single-label run, indicating overfitting after the first epoch.
- Rare genres perform poorly; weighted F1 should be read alongside macro F1 and individual-label metrics.
- The multi-label code thresholds raw logits at 0.5, equivalent to approximately 0.622 probability. A probability threshold of 0.5 corresponds to a logit threshold of 0. This implementation is preserved, and the reported metrics reflect its original rule.
- The current multi-label CSV produces a different cleaned record count from the saved notebook output. The historical dataset version must be reconciled before attempting to reproduce those scores.
- The datasets and task formulations differ. These experiments do not constitute a controlled comparison of single-label versus multi-label learning or a benchmark against BERT or traditional classifiers.
- Class-weighted loss and scheduler code exist in the PyTorch experiment, but completed results are unavailable. Synonym augmentation is not implemented in the supplied notebooks.

## Model and data provenance

The notebooks use Hugging Face's [`distilbert-base-uncased`](https://huggingface.co/distilbert/distilbert-base-uncased) checkpoint. DistilBERT is fine-tuned here; teacher-student distillation is not implemented by this project. The original single-label data description identifies the IMDb text database as its source. Dataset files are not redistributed in this repository.
