# CEDR
**C**orpus for **E**motions **D**etecting in **R**ussian-language text sentences of different social sources.

This repository provides dataset and additional materials of the paper: "[Data-Driven Model for Emotion Detection in Russian Texts](https://www.sciencedirect.com/science/article/pii/S1877050921013247)".

The dataset was prepared for the five basic emotions (joy, sadness, anger, fear, and surprise) using a crowdsourcing platform and a home-grown procedure for collecting and controlling annotators markup.

Version 0.0.1 (paper version)

Data
---
The data is in Russian.

Data was collected from several sources: posts of the Live Journal social network, texts of the online news agency Lenta.ru, and Twitter microblog posts.

In total, 3107 sentences were selected from LiveJournal posts, 3131 sentences from Lenta.Ru, and 3430 sentencesfrom Twitter. After selection, sentences were offered to annotators for labeling.

The dataset includes a set of train/test splits, with 7528, and 1882 examples respectively.

The dataset are uploaded to our cloud storage and are available [here](https://cloud.mail.ru/public/f89f/W4o4PtU9U).

The number of labels by data source is presented in Table 1.

#### Table #1. The number of emotion labels in different subsets of our dataset.

| Data source | Joy |Sadness| Fear |Anger|Surprise|No emotions|Total sentences|
| ----------- | --- | ----- | ---- | --- | ------ | --------- | ------------- |
| Twitter     | 1233| 1372  | 362  | 198 |  221   |   129     |     3430      |
| Lenta.ru    | 190 | 92    | 126  | 121 |  195   |   2411    |     3131      |
| LiveJournal | 433 | 298   | 264  | 231 |  398   |   1519    |     3107      |
| Total       | 1856| 1762  | 752  | 550 |  814   |   4059    |     9668      |

Annotation procedure
---
Annotating sentences with labels of their emotions was performed with the help of a crowdsourcing platform [Yandex Toloka](https://yandex.ru/support/toloka/index.html?lang=en).

Only those of the 30% of the best-performing active users (by the platform’s internal rating) who spoke Russian and were over 18 years old were allowed into the annotation process.

The annotators’ task was: “What emotions did the author express in the sentence?”. The annotators were allowed to put an arbitrary number of the following emotion labels: "joy", "sadness", "anger", "fear", and "surprise".
Sentences which the annotators believed to express no author’s emotion were to be assigned "No emotion" label.

Sentences were split into tasks and assigned to annotators so that each sentence was annotated at least three times.

The final label for the sentence was "No emotion" if less than 60% annotators put any emotion labels on it (regard less of their types). A label of a specific emotion was assigned to a sentence if put by more than half of the annotators (except those who put "No emotion").

Data Fields
---
Each instance is a text sentence in Russian from several sources with one or more emotion annotations (or no emotion at all), which includes the following fields:
- text: the text of the sentence;
- labels: the emotion annotations;
- source: the tag name of the corresponding source
- sentences: text tokenized and lemmatized with [udpipe](https://ufal.mff.cuni.cz/udpipe)
  - 'forma': the original word form;
  - 'lemma': the lemma of this word

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

<table><thead><tr><th rowspan="2">Model</th><th colspan="2">Joy</th><th colspan="2">Sadness</th><th colspan="2">Fear</th><th colspan="2">Anger</th><th colspan="2">Surprise</th><th colspan="2">Mean</th></tr><tr><th>mic.</th><th>mac.</th><th>mic.</th><th>mac.</th><th>mic.</th><th>mac.</th><th>mic.</th><th>mac.</th><th>mic.</th><th>mac.</th><th>mic.</th><th>mac.</th></tr></thead><tbody><tr><td>Random</td><td>0.5 </td><td>0.45</td><td>0.52</td><td>0.45</td><td>0.51</td><td>0.39</td><td>0.5 </td><td>0.38</td><td>0.5 </td><td>0.4 </td><td>0.51</td><td>0.41</td></tr><tr><td>SVM (TF-IDF)</td><td>0.87</td><td>0.71</td><td>0.88</td><td>0.72</td><td>0.94</td><td>0.68</td><td>0.95</td><td>0.53</td><td>0.93</td><td>0.69</td><td>0.91</td><td>0.66</td></tr><tr><td>Lexicon</td><td>0.76</td><td>0.65</td><td>0.7 </td><td>0.59</td><td>0.79</td><td>0.65</td><td>0.76</td><td>0.62</td><td>0.79</td><td>0.64</td><td>0.76</td><td>0.63</td></tr><tr><td>Our ensemble</td><td>0.93</td><td>0.88</td><td>0.91</td><td>0.83</td><td>0.95</td><td>0.75</td><td>0.92</td><td>0.66</td><td>0.93</td><td>0.77</td><td>0.93</td><td>0.78</td></tr></tbody></table>

Requirements
---
- Python 3.7+
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
