
# Efficient NLP: Carbon and Accuracy Trade-offs between BERT and TF-IDF Models

Academic code and results for our term-paper study on accuracy vs. energy in NLP.
We compare TF-IDF baselines to BERT across three text classification datasets, track energy with CodeCarbon, and report kWh, CO₂, kWh per correct prediction, and joules per +1% accuracy.

## Abstract
This project asks a practical question: How close can lightweight TF-IDF models get to BERT while using far less energy?

We price accuracy rather than just rank models. This project introduces a model-agnostic evaluation method that combines CodeCarbon energy logs with two decision metrics: kWh per correct prediction and joules per +1% accuracy. On AG News, Amazon Polarity, and DBPedia, TF-IDF baselines often sit within a few accuracy points of BERT while using 10–100× less energy. BERT still leads on peak accuracy, but each extra point now has a clear energy and CO₂ price tag.

In this study, BERT is a proxy for any larger LM: swap in DistilBERT, MiniLM, DeBERTa-v3 small, adapters, or quantized variants—the accounting stays the same. This reframes “Which model is best?” as “Which model is best for your carbon and budget constraints,” which is what matters in production, on edge devices, and for sustainability reporting.

## Motivation & Research Questions
- Can simple, classical pipelines offer good enough accuracy for common NLP tasks at a fraction of the energy?
- How should we quantify “accuracy at what cost” so choices are transparent and reproducible?
- When (dataset, setting) is BERT’s extra accuracy worth its energy and CO₂ overhead?## Methods (Brief)
### Pipelines
- TF-IDF + Logistic Regression
- TF-IDF + Linear SVM
- TF-IDF + Complement Naïve Bayes
- BERT-base (fine-tuned per dataset)
### Tracking
CodeCarbon wraps training + inference and logs energy from CPU, GPU, and memory (Wh → kWh), then estimates CO₂ via regional carbon intensity.
### Hardware
Deepnote cloud; NVIDIA T4 (16 GB VRAM), 32 GB RAM, CUDA enabled. Same instance type for all runs.
### Determinism
Fixed seeds where libraries allow; identical preprocessing across TF-IDF baselines; standard train/test splits.

## Metrics
- Accuracy (test)
- Energy (kWh) and CO₂ (g/kg) from CodeCarbon
- kWh per correct prediction = kWh / #correct
- Joules per +1% accuracy = ((ΔkWh * 3.6e6) / (ΔAcc * 100)) comparing BERT vs. best TF-IDF on the same dataset
- BERT–baseline deltas: ΔAccuracy, ΔEnergy

## Datasets
- AG News - 4-way news topic classification
- Amazon Polarity - binary sentiment on product reviews
- DBPedia - ontology/topic classification

We use the standard train/test splits, minimal text cleaning, and no label changes. The goal is a clean comparison of accuracy vs. energy, not leaderboard tuning.

## Results

We didn’t just benchmark models, we priced accuracy. Using CodeCarbon and two simple ratios kWh per correct and joules per +1% accuracy we show when a heavier transformer actually earns its keep. BERT here is a stand-in for any large LM: swap in DistilBERT, a quantized adapter, or tomorrow’s model and the method still holds. The striking result is practical: on our tasks, TF-IDF delivers 10–100× more correct predictions per kWh, while BERT’s extra accuracy often comes with a steep energy bill. This turns “which model is best?” into “which model is best for your carbon and budget constraints,” a question that matters for production, edge deployment, and sustainability reporting.
## Limitations
- One hardware profile (T4, 32 GB RAM) and one cloud region; carbon intensity varies elsewhere.
- Conservative hyperparameters; stronger tuning may change absolute results.
- Three datasets; extending to other tasks will test generality.
- We report total run energy; a finer split of training vs. inference is possible.

## Future Research Direction
- Carbon-aware hyperparameter search (optimize accuracy and kWh jointly).
- Dynamic model routing under energy/latency budgets.
- Token-normalized energy (per-token Joules) to explain sequence-length effects.
- Standardized “green” benchmarks: publish accuracy + kWh/correct as a requirement.
## Authors


- Abdullah Orzan — s2aborza@uni-trier.de
- Bartuğ Hacır — s2bahacr@uni-trier.de
- İpek Nur Dolu — s2ipdolu@uni-trier.de