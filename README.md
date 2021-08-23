# CEDR
**C**orpus for **E**motions **D**etecting in **R**ussian-language text sentences of different social sources.

This repository provides dataset and additional materials of the paper: "[Data-Driven Model for Emotion Detection in Russian Texts](https://www.sciencedirect.com/science/article/pii/S1877050921013247)".

The dataset was prepared for the five basic emotions (joy, sadness, anger, fear, and surprise) using a crowdsourcing platform and a home-grown procedure for collecting and controlling annotators markup.

The data is in Russian.

Version 0.1.1 (actual version)

In this version of the dataset (the previous one is described in the article, branch ```v_0.0.1_(paper)```) an inaccuracy with the presence of similar sentences was corrected. The texts of sentences that completely (or almost completely) repeated each other were removed. In addition, some new examples were added, but the total number of examples was reduced from 9668 to 9410.

This dataset was published on [hugging face](https://huggingface.co/datasets/sagteam/cedr) and is available for use as:

```
from datasets import load_dataset

train_df = load_dataset('sagteam/cedr', name='enriched', split='train')
test_df = load_dataset('sagteam/cedr', name='enriched', split='test')
```

Here are 2 dataset configurations:
- "main" - contains "text", "labels", and "source" features;
- "enriched" - includes all "main" features and "sentences".

Data
---
Data was collected from several sources: posts of the Live Journal social network, texts of the online news agency Lenta.ru, and Twitter microblog posts.

An example for an instance from the dataset is shown below:
```
{
  'text': 'Забавно как люди в возрасте удивляются входящим звонкам на мобильник)',
  'labels': [0],
  'source': 'twitter',
  'sentences': [
    [
      {'forma': 'Забавно', 'lemma': 'Забавно'},
      {'forma': 'как', 'lemma': 'как'},
      {'forma': 'люди', 'lemma': 'человек'},
      {'forma': 'в', 'lemma': 'в'},
      {'forma': 'возрасте', 'lemma': 'возраст'},
      {'forma': 'удивляются', 'lemma': 'удивляться'},
      {'forma': 'входящим', 'lemma': 'входить'},
      {'forma': 'звонкам', 'lemma': 'звонок'},
      {'forma': 'на', 'lemma': 'на'},
      {'forma': 'мобильник', 'lemma': 'мобильник'},
      {'forma': ')', 'lemma': ')'}
    ]
  ]
}
```

Emotion label codes: {0: "joy", 1: "sadness", 2: "surprise", 3: "fear", 4: "anger"}

In total, 3069 sentences were selected from LiveJournal posts, 2851 sentences from Lenta.Ru, and 3490 sentencesfrom Twitter. After selection, sentences were offered to annotators for labeling.

The dataset includes a set of train/test splits, with 7528, and 1882 examples respectively.

The number of labels by data source is presented in Table 1.

#### Table #1. The number of emotion labels in different subsets of our dataset. The difference from the previous version of the dataset (v_0.0.1_(paper)) is shown in parentheses.

| Data source |   Joy    | Sadness  |  Fear   | Anger   |Surprise |No emotions|Total sentences|
| ----------- | -------- | -------- | ------- | ------- | ------- | --------- | ------------- |
| Twitter     |1306 (+73)|1409 (+37)|351 (-11)|195 (-3) |197 (-24)| 121 (-8)  |  3490 (+60)   |
| Lenta.ru    | 185 (-5) |  89 (-3) |115 (-11)|112 (-9) |190 (-5) |2164 (-247)|  2851 (-280)  |
| LiveJournal | 431 (-2) | 298 (0)  |264 (0)  |229 (-2) |390 (-8) |1492 (-27) |  3069 (-38)   |
| Total       |1922 (+66)|1796 (+34)|730 (-22)|536 (-14)|777 (-37)|3777 (-282)|  9410 (-258)  |

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

| Model       |   Joy   | Sadness |   Fear  |  Anger  | Surprise|  Mean   |
|             |mic.|mac.|mic.|mac.|mic.|mac.|mic.|mac.|mic.|mac.|mic.|mac.|
| ----------- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- |
| Random      |0.49|0.44|0.49|0.44|0.49|0.39|0.5 |0.39|0.51|0.41|0.5 |0.41|
| SVM (TF-IDF)|0.86|0.67|0.86|0.71|0.94|0.66|0.93|0.5 |0.93|0.67|0.9 |0.64|
| Lexicon     |0.83|0.73|0.71|0.62|0.83|0.68|0.76|0.57|0.88|0.76|0.8 |0.67|
| Our ensemble|0.92|0.87|0.92|0.86|0.93|0.73|0.9 |0.62|0.93|0.76|0.92|0.77|

Requirements
---
- Python 3.7+
- datasets==1.11.0
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
