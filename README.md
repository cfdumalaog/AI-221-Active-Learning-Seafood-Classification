# AI 221 - Active Learning for Imbalanced Seafood Classification

This repository contains Crizepvill F. Dumalaog's AI 221 final project on using active learning to reduce the amount of labeled data needed for a nine-class seafood image-classification task.

## Project focus

- ResNet-18 image classification
- Uncertainty sampling for active learning
- Comparison against a random-sampling baseline
- Repeated experiments and Macro F1 evaluation
- Notebook-based analysis and presentation materials

## Reported result

Using 2.70% of the available data, the active-learning approach reached a mean test Macro F1 of 0.8107 at iteration 8, compared with 0.7371 for the random-sampling baseline.

## Repository contents

- `ai-221-final-project.ipynb` - final analysis notebook
- `written_report.txt` - written project report
- `presentation.pdf` - project presentation
- `presentation.txt` - extracted presentation text
- `presentation_speaker_script.md` - presentation script
- `references/` - reference manifest and source links

The raw seafood image dataset and environment-specific files are not included. The notebook may require updating local data paths before execution.

