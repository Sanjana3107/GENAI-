SELFIES-Based Molecular Generation using Masked Language Modeling (MLM) with BERT

1. Objective
The present study aims to generate new, valid, and diverse molecules using SELFIES-based encoding and Transformer generative modeling. The process involves converting SMILES strings into SELFIES to ensure molecular validity, followed by introducing random masking and fine-tuning a pretrained BERT model with a Masked Language Modeling (MLM) objective to reconstruct and predict chemically meaningful tokens.

Goals
SELFIES-based molecular encoding
Random token masking (15%)
BERT fine-tuning with MLM objective
Prediction-based molecule reconstruction

2. Background
SMILES (Simplified Molecular Input Line Entry System) is widely used to represent molecular structures but is syntactically fragile — small changes can render molecules invalid.
SELFIES (SELF-referencing Embedded Strings) overcome this limitation by ensuring every sequence represents a valid molecule.
Recent advances in Natural Language Processing (NLP), particularly Transformer architectures like BERT, have enabled learning complex chemical grammars. By adapting Masked Language Modeling (MLM) to SELFIES, a model can predict missing tokens and thus synthesize valid and chemically meaningful molecules.

3. Masked Language Modeling (MLM)
MLM is a self-supervised learning technique introduced in BERT.
A fixed percentage (15%) of tokens are replaced by a special [MASK] token.
The model learns to predict these masked tokens from the surrounding context.
In this study, MLM is applied to SELFIES sequences, ensuring that predicted tokens always correspond to chemically valid molecular substructures. This allows the model to learn chemical patterns and relationships among building blocks of molecules.

4. Pretrained BERT

A pretrained BERT model was fine-tuned on masked SELFIES sequences.
Model Configuration
Architecture: Multi-layer self-attention encoder (Transformer)
Input: Tokenized SELFIES with [CLS], [SEP], and [MASK]
Output: Probability distributions for predicting masked tokens
Loss Function: Cross-entropy loss
Objective: Accurately reconstruct masked tokens from molecular context

5. Data Preprocessing and Collection
5.1 Source of Data
The dataset contains ~250,000 SMILES strings sourced from the ZINC database, a well-known repository of commercially available and biologically relevant small molecules.
Molecules were filtered according to Lipinski’s Rule of Five to ensure drug-likeness and structural validity.

5.2 Preprocessing Steps

5.2.1 SMILES Validation
Loaded SMILES strings from CSV.
Verified validity using RDKit by parsing into molecular graphs.
Removed invalid SMILES (syntax or valence errors).

5.2.2 Conversion to SELFIES
Converted valid SMILES into SELFIES representations.
Ensured all sequences correspond to chemically valid molecules.

5.2.3 Tokenization and Vocabulary Construction
Tokenized SELFIES strings into tokens.
Constructed vocabulary including special tokens:
[PAD]: Padding
[MASK]: Masking
[CLS]: Classification token
[SEP]: Segment separation

5.2.4 Random Masking (MLM Input Preparation)
Randomly masked 15% of SELFIES tokens with [MASK].
Original tokens were used as labels for MLM supervision.

5.2.5 Padding and Attention Masking
Padded all sequences to a fixed maximum length.
Created attention masks (1 = token, 0 = padding).
Padded labels to match input length.

5.2.6 Data Storage
Stored processed sequences, labels, and attention masks in CSV and JSON formats:
CSV for analysis/debugging

JSON for model training/inference

6. Model Development

6.1 Model and Training Setup
Model: BertForMaskedLM from HuggingFace Transformers
Base Model: bert-base-uncased
Tokenizer: BertTokenizerFast (customized for SELFIES if required)
Framework: HuggingFace Transformers (PyTorch backend)

Training Details
Dataset Split: 80% Train | 10% Validation | 10% Test
Batch Size: 64
Epochs: 1
Precision: Mixed (fp16=True)
Checkpoint Directory: ./bert_selfies_model
Logging Interval: 500 steps
Checkpoint Interval: 1000 steps
Evaluation Strategy: None (evaluation_strategy="no")

7. Results and Evaluation

7.1 Molecule Generation
Masked SELFIES sequences were fed to the fine-tuned BERT model.
Top-k predictions were used to fill [MASK] positions.
Predicted SELFIES were decoded back into SMILES strings.

7.2 SELFIES to SMILES Conversion
Generated SELFIES sequences were successfully converted to valid SMILES molecules.

7.3 Validation of New Molecules
Generated SMILES were checked for novelty, validity, and uniqueness.



8. Conclusion

This study demonstrates that BERT with Masked Language Modeling (MLM) on SELFIES representations can successfully generate chemically valid and novel molecules.
By combining:
SMILES validation
SELFIES conversion
Tokenization and masking
BERT-based fine-tuning
…the model learns the chemical syntax required for generating realistic molecular structures.
