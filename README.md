# INFERNOIR — Implicit Assumption Analyzer

INFERNOIR is a student NLP research prototype that explores generating possible implicit assumptions from written Reddit arguments. It is intended to support critical thinking, not to determine whether an argument is true, valid, or sound.

## Project overview

Given a Reddit-style argument, the project generates candidate assumptions that may be underlying the argument. The notebook also compares a text-retrieval approach with a fine-tuned text-generation model.

### Approaches compared

- **TF-IDF retrieval baseline:** represents arguments using weighted word features, finds a similar training argument, and reuses its annotated assumptions.
- **Sentence-embedding retrieval baseline:** represents arguments with `sentence-transformers/all-MiniLM-L6-v2`, retrieves a semantically similar training argument, and reuses its assumptions.
- **T5-small:** a fine-tuned text-to-text transformer that generates candidate assumptions.

### Dataset

The project uses [`agentlans/reddit-logic`](https://huggingface.co/datasets/agentlans/reddit-logic), a dataset of Reddit `r/ChangeMyView` arguments with structured annotations, including assumptions.

The annotations are largely model-assisted and should be treated as reference labels rather than verified ground truth.

## Repository contents

- `INFERNOIR_Notebook.ipynb` — Colab/Jupyter notebook for data preparation, baselines, model training/evaluation, auditing, and the Gradio interface.
- `INFERNOIR_Presentation.pptx` — project presentation.

## Run the project

1. Open [Google Colab](https://colab.research.google.com/).
2. Select **File → Upload notebook** and upload `INFERNOIR_Notebook.ipynb`.
3. Select **Runtime → Change runtime type → T4 GPU** if available.
4. Run the notebook cells in order. Training and evaluation can take several minutes.
5. The final Gradio cell may create a temporary public demo link while the Colab runtime remains active.

The notebook may install dependencies as part of its setup. If running locally, install packages listed in `requirements.txt` first.

## Reported evaluation

On 994 held-out test examples, the reported ROUGE scores were:

| Approach | ROUGE-1 F1 | ROUGE-L F1 |
|---|---:|---:|
| T5-small | 0.2643 | 0.2230 |
| Sentence-embedding retrieval | 0.2464 | 0.1952 |
| TF-IDF retrieval | 0.2405 | 0.1917 |

ROUGE measures overlap with reference text, not logical correctness. T5-small achieved the highest reported overlap, but qualitative tests showed that outputs can be repetitive, irrelevant, or logically weak. These scores should not be interpreted as proof that the generated assumptions are correct.

## Limitations and responsible use

- Generated assumptions are suggestions for human review, not authoritative interpretations.
- Dataset annotations may contain errors or reflect the biases of their generation process.
- The data comes from a single Reddit community and may not generalize to all writing styles or topics.
- The validity-label audit is not a demographic fairness evaluation.
- Do not use INFERNOIR as an automated judge of truth, validity, soundness, or a person's intent.

## Tech stack

Python, pandas, NumPy, Hugging Face Datasets and Transformers, scikit-learn, Sentence Transformers, ROUGE, and Gradio.

## Citation and dataset

Dataset: `agentlans/reddit-logic` on Hugging Face. Check the dataset page for its license and citation details before redistributing dataset content.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
