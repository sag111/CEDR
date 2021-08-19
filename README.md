# CEDR
**C**orpus for **E**motions **D**etecting in **R**ussian-language text sentences of different social sources.

This repository provides dataset and additional materials of the paper: "[Data-Driven Model for Emotion Detection in Russian Texts](https://www.sciencedirect.com/science/article/pii/S1877050921013247)".

The dataset was prepared for the five basic emotions (joy, sadness, anger, fear, and surprise) using a crowdsourcing platform and a home-grown procedure for collecting and controlling annotators markup.

Version 0.1.1 (actual version)

coming soon

Data
---
The data is in Russian.

Data was collected from several sources: posts of the Live Journal social network, texts of the online news agency Lenta.ru, and Twitter microblog posts.

In total, 3107 sentences were selected from LiveJournal posts, 3131 sentences from Lenta.Ru, and 3430 sentencesfrom Twitter. After selection, sentences were offered to annotators for labeling.

The dataset includes a set of train/test splits, with 7528, and 1882 examples respectively.

The dataset are uploaded to our cloud storage and are available [here](https://cloud.mail.ru/public/f89f/W4o4PtU9U).

The number of labels by data source is presented in Table 1.

#### Table #1. The number of emotion labels in different subsets of our dataset.

coming soon

Annotation procedure
---
Annotating sentences with labels of their emotions was performed with the help of a crowdsourcing platform [Yandex Toloka](https://yandex.ru/support/toloka/index.html?lang=en).

Only those of the 30% of the best-performing active users (by the platform’s internal rating) who spoke Russian and were over 18 years old were allowed into the annotation process.

The annotators’ task was: “What emotions did the author express in the sentence?”. The annotators were allowed to put an arbitrary number of the following emotion labels: "joy", "sadness", "anger", "fear", and "surprise".
Sentences which the annotators believed to express no author’s emotion were to be assigned "No emotion" label.

Sentences were split into tasks and assigned to annotators so that each sentence was annotated at least three times.

The final label for the sentence was "No emotion" if less than 60% annotators put any emotion labels on it (regard less of their types). A label of a specific emotion was assigned to a sentence if put by more than half of the annotators (except those who put "No emotion").

Baselines
---

While evaluating the proposed solution, we compare it against several basic estimates:
1. Random: The emotion label is chosen randomly for each sentence;
2. SVM (TF-IDF): The classifier based on SVM with linear kernel, where the input sentence is encoded with the TF-IDF frequency method using 4-to-8-long character n-grams;
3. Lexicon: The classifier based on dictionaries of emotive vocabulary. The emotion label is determined by thepresence of words from the emotive vocabulary for the corresponding emotion.
4. Our ensembel: A set of classification models and encoding a sentence with the pretrained [ELMo model](docs.deeppavlov.ai/en/master/features/pretrainedvectors.html#elmo). Models was performed with an AutoML method based on [the TPOT library](http://epistasislab.github.io/tpot/).  

Calculation of baselines is presented in the jupyter notebook ```./notebooks/Baseline_accuracy.ipynb``` Before running the notebook, you need to download the data and place them in a folder ```./data/```.

The F1-scores of the selected classifiers in comparison with the results of the baseline methods are presented in Table2.

#### Table #2. The F1-micro (mic.) and F1-macro (mac.) of detecting different emotions.

coming soon

Requirements
---
- Python 3.7+
- datasets 1.11.0
- scikit-learn==0.22.1
- tpot==0.11.1
- xgboost==0.90

Citing & Authors
---
If you have found our results helpful in your work, feel free to cite our publication and this repository as

```
@article{sboev2021data,
  title={Data-Driven Model for Emotion Detection in Russian Texts},
  author={Sboev, Alexander and Naumov, Aleksandr and Rybka, Roman},
  journal={Procedia Computer Science},
  volume={190},
  pages={637--642},
  year={2021},
  publisher={Elsevier}
}
```

Contributions
---
Thanks to [@naumov-al](https://github.com/naumov-al) for adding this dataset.