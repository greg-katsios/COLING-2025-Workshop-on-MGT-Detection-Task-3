# Task 3: Cross-Domain Machine-Generated Text Detection

<p align="left" float="left">
  <img src="logo.png" height="40" />
</p>


[News](#news) | [Competition](#competition) | [Dataset](#dataset) | [Important Dates](#important_dates) | [Data Format](#data_format) | [Evaluation Metrics](#scorer_and_official_evaluation_metrics) | [Baselines](#baselines) | [Contact](#contact)

<!-- Large language models (LLMs) are becoming mainstream and easily accessible, ushering in an explosion of machine-generated content over various channels, such as news, social media, question-answering forums, educational, and even academic contexts. Recent LLMs, such as GPT-4o, Claude3.5 and Gemini1.5-pro, generate remarkably fluent responses to a wide variety of user queries. The articulate nature of such generated texts makes LLMs attractive for replacing human labor in many scenarios. However, this has also resulted in concerns regarding their potential misuse, such as spreading misinformation and causing disruptions in the education system. Since humans perform only slightly better than chance when classifying machine-generated vs. human-written text, there is a need to develop automatic systems to identify machine-generated text with the goal of mitigating its potential misuse. -->

When applying detectors to machine-generated text in the wild, the dominant emerging paradigm is to use an **open-domain API-based** detector. However, many commonly used detectors exhibit poor cross-domain and cross-model robustness. Thus, it is critical to train our detectors to be able to handle text from many domains with both high accuracy and low false positive rates.

In the COLING Workshop on MGT Detection Task 3, we focus on **cross-domain robustness** of detectors by testing submissions on the [RAID benchmark](https://raid-bench.xyz/). We adopt the same straightforward binary problem formulation as Task 1 however the texts will come from 8 different domains, 11 generative models, and 4 decoding strategies.

Our domains are:

| Domain              | Source | Dataset Link |
| :---------------- | :------: | :----: |
| Arxiv Abstracts | [arxiv.org](https://arxiv.org) | [(Link)](https://www.kaggle.com/datasets/Cornell-University/arxiv) |
| Book Plot Summaries | [wikipedia.org](https://wikipedia.org) | [(Link)](https://paperswithcode.com/dataset/cmu-book-summary-dataset) |
| BBC News Articles |  [bbc.com/news](https://www.bbc.com/news) | [(Link)](https://github.com/derekgreene/bbc-datasets) |
| Poems |  [poemhunter.com](https://www.poemhunter.com/)   | [(Link)](https://www.kaggle.com/datasets/michaelarman/poemsdataset) |
| Reddit Posts | [reddit.com](https://www.reddit.com/) | [(Link)](https://huggingface.co/datasets/sentence-transformers/reddit-title-body) |
| Recipes | [allrecipes.com](https://www.allrecipes.com/) | [(Link)](https://recipenlg.cs.put.poznan.pl/) |
| IMDb Movie Reviews | [imdb.com](https://www.imdb.com/) | [(Link)](https://ieee-dataport.org/open-access/imdb-movie-reviews-dataset) |
| Wikipedia Articles | [wikipedia.org](https://www.wikipedia.org/) | [(Link)](https://huggingface.co/datasets/aadityaubhat/GPT-wiki-intro) |

<!-- [cookbooks.com](https://cookbooks.com/), [food.com](https://www.food.com/), [yummly.com](https://www.yummly.com/) -->

There are two subtasks:
- Subtask A: Non-Adversarial Cross-Domain MGT detection.
- Subtask B: Adversarial Cross-Domain MGT detection.

## <a name="news"></a>NEWS 

### 18 Sep 2024

We have released our instructions and training set.

## <a name="competition"></a>Competition

The competition will be held on the [RAID Website](https://raid-bench.xyz/). We will be releasing a separate leaderboard specifically for the shared task that will exist alongside the main RAID leaderboard and will be populated with results after the task finishes.

Submission will work identically to how it works on the main RAID leaderboard. To submit to the shared task, you will be required to open up a pull request to this github repository containing the predictions of your detector as well as some additional metadata information. Please consult the [RAID Leaderboard Submission Instructions](https://github.com/liamdugan/raid?tab=readme-ov-file#leaderboard-submission) for more details and information on formatting.

> [!NOTE]
> Please **DO NOT SUBMIT** to the main RAID leaderboard during the duration of the shared task. If you do so, **you will be disqualified.**

## <a name="dataset"></a>Dataset
For this task we will be using the RAID dataset.
**Download RAID** by consulting the [RAID Github Repository](https://github.com/liamdugan/raid?tab=readme-ov-file#download-raid).

## <a name="important_dates"></a>Important Dates

- 18th September, 2024: Training set release
- 20th October, 2024: Test set release and evaluation phase starts
- 25th October, 2024: Evaluation phase closes
- 28th October, 2024: Leaderboard to be public
- 15th November, 2024: System description paper submission

## <a name="data_format"></a>Prediction File Format and Format Checkers

We currently are working on a file format checker. This will be released soon.

## <a name="scorer_and_official_evaluation_metrics"></a>Scorer and Official Evaluation Metrics

The **official evaluation metric** is **TPR @ FPR=5%**. That is, the accuracy of the model on detecting machine-generated text at a fixed false positive rate of 5%.
To calculate this, our scorer uses the model predictions on human data to search a classification threshold that results in 5% FPR for each domain.

To run the scorer, first run `pip install raid-bench` then use the RAID pip package as follows:

```py
from raid import run_detection, run_evaluation
from raid.utils import load_data

# Define your detector function
def my_detector(texts: list[str]) -> list[float]:
    pass

# Download & Load the RAID dataset
train_df = load_data(split="train")

# Run your detector on the dataset
predictions = run_detection(my_detector, train_df)

# Evaluate your detector predictions
evaluation_result = run_evaluation(predictions, train_df)
```

## <a name="baselines"></a>Baselines

We have run a number of publicly available open-source detectors on RAID. [Binoculars](https://arxiv.org/abs/2401.12070) gets **79.0%**, [RADAR](https://huggingface.co/TrustSafeAI/RADAR-Vicuna-7B) gets **65.6%**, and [roberta-base-openai-detector](https://huggingface.co/openai-community/roberta-base-openai-detector) gets **59.2%** on the non-adversarial RAID test set.

We will also be releasing some simple baseline trained models on the RAID dataset. These will be released shortly.

## <a name="contact"></a>Contact

Website: [https://genai-content-detection.gitlab.io](https://genai-content-detection.gitlab.io)  
Email: genai-content-detection@googlegroups.com or directly to ldugan@seas.upenn.edu
