# Covid_Abuse: A Large-Scale Dataset of Abusive Language Directed Toward Public Health Professionals in Canada (2020–2021)

Covid_Abuse is a research-grade dataset of abusive vs non-abusive Twitter/X posts that **directly mention or reply to** public health professionals (PHPs) in Canada during the COVID-19 pandemic. The dataset spans **January 1, 2020 through November 30, 2021**, covering the most turbulent period of the pandemic response.

The dataset includes:
- **Tweet IDs**
- **timestamps**
- **author metadata**
- **automated abusive labels from GPT**
- **COVID neologism terms**
- **balanced human annotations**

No tweet text is redistributed.  
Users must **hydrate** the content using their own Twitter/X API access.

---

## Motivation and Contribution

Public health professionals were at the forefront of both:
- health policy, and
- online controversy.

They were targeted through:
- insults, disparagement, harassment, dehumanization,
- misinformation, conspiracy rhetoric, and pandemic-era neologisms.

This dataset enables research into:
- social safety
- hate and harassment detection
- misinformation
- adaptive language modeling
- temporal and socio-political analysis
- agentic moderation systems

**This is the first dataset focused specifically on abuse directed at Canadian public health professionals across the entire pandemic.**

---

## Data Summary

- **Time window**: Jan 1, 2020 → Nov 30, 2021

- **PHP accounts monitored**:

  - **Jan 2020 → May 2021 (first 17 months):**  
    We monitored a comprehensive set of **35 Twitter handles** related to **17 public health professionals** in Canada.  
    These included:
    - personal accounts
    - professional institutional accounts
    - general information accounts
    - parody accounts associated with them

  - **June 2021 → Nov 2021 (remaining period):**  
    We continued data collection for **14 Twitter handles** related to the **four PHPs who received the dominant share of targeted tweets**.  

- **Collection method**:
  - All **tweets that directly mention** one of the monitored PHP accounts  
  - All **tweets that are replies to** tweets posted by those PHP accounts  
  - After collection, we performed **deduplication, cleaning, and validation**

This produced a dataset of:
- **Total unique tweet lebeled by GPT**: ~ **1,884,097 Tweets**
- **Human-annotated subset**: **500 samples (balanced 50% abusive / 50% non-abusive)**

---

## Annotation Method

### 1. Full Automated Annotation (GPT)

All tweets were annotated using:
- Model: **gpt-4o-mini**
- Temperature: **0**
- Output:
  - abusive = {0,1}
  - COVID neologisms list (e.g., “plandemic”, “covidiot”, “anti-vax”)

Annotation was performed **in multiple rounds** to ensure:
- retry on failures
- consistent prompts
- reproducible state

Code for:
- batching
- polling
- sanitization
- merging
is available for full reproducibility.

### 2. Partial Human Annotation

A **stratified balanced sample** was drawn:
- **250 abusive**
- **250 non-abusive**

This subset was labeled by human coders to evaluate:
- **inter-annotator agreement**
- **GPT consistency**

Results (placeholders):
- **Cohen’s κ (alpha)**: **[PLACEHOLDER]**
- **GPT accuracy vs humans**: **[PLACEHOLDER]%**

These numbers will be updated after human annotation campaign.

---

## Dataset Structure

Each row provides:

| Field | Description |
|---|---|
| tweet_id | ID |
| created_at | Timestamp |
| is_abusive | 0 or 1 (GPT) |
| new_covid_terms | JSON list |

### IMPORTANT:
- **No tweet text is included**
- **Hydration required**

---

## How to Use the Dataset

### Step 1: Download the CSV files from `data/`
```
covid_abuse.csv
```

### Step 2: Hydrate tweets
Use any Twitter hydrator and save in `full_covid_abuse.csv`:

```
tweet_ids = list(df.tweet_id)
texts = hydrate(tweet_ids, bearer_token)
```

### Step 3: Join hydrated text with labels
Then apply:
- supervised models
- embeddings
- NLP pipelines

---

## Potential Use Cases

Covid_Abuse enables a wide range of research directions:

### Abusive Language Detection
- Train/test abusive-language classifiers (BERT, LLaMA, GPT, etc.)
- Compare models across different time periods

### Social Safety & Risk
- Analyze harassment patterns targeting health officials
- Study content moderation strategies

### Adaptive & Continual Learning
- Concept drift during pandemic phases
- Evolving vocabulary (e.g., neologisms)

### Agentic & Multi-Agent Systems
- Agentic moderation with:
  - tool use
  - grounding
  - context retrieval

### Temporal and Policy Analysis
- Align abusive spikes with:
  - policy announcements
  - mask mandates
  - vaccination phases

### Sociolinguistics
- Pandemic-era language change
- COVID neologism dynamics

The dataset is inherently longitudinal and socially meaningful.

---

## Compliance with Twitter/X Terms

The dataset:
- **only contains Tweet IDs and metadata**
- **no tweet text is redistributed**

Users are responsible for hydration.

This aligns with:
- Twitter Developer Agreement
- Twitter ToS
- academic norms

---

<!-- ## Citation

Please cite when using:

```
@dataset{moghaddam2025covidabuse,
  author = {Samaneh H. Moghaddam},
  title = {Covid_Abuse: A Dataset of Abusive Language Directed Toward Public Health Professionals in Canada},
  year = {2025},
  url = {https://github.com/...}
}
```

--- -->

## Contact

For collaboration or questions:

samaneh.h.moghaddam@gmail.com

---

## Reproducibility

This repository includes **all code used to generate the dataset**:

- `notebooks/01_labeling.ipynb`  
  multi-round GPT annotation

- `notebooks/02_eda.ipynb`  
  exploratory analysis and visualizations

- `notebooks/03_human_samples.ipynb`  
  subsample creation for human annotation

All notebooks are designed to be **runnable end-to-end**.

---
## Labeling Pipeline

Create a file:

```
credentials/openai.env
```

with:

```
OPENAI_API_KEY=your_key_here
```

Run:

```
notebooks/01_labeling.ipynb
```

This notebook:
- loads unlabeled tweets from `full_covid_abuse.csv`
- assigns them to GPT in batches
- polls for completion
- merges results into the same file
- outputs new rounds until all are labeled

A balanced subsample of size 500 is created by:

```
notebooks/03_human_samples.ipynb
```
And saved in `labled_by_human.csv`

---

# Summary

Covid_Abuse provides:

- a **unique** dataset of harassment against Canadian PHPs
- **full GPT annotation**
- **partial human annotation**
- **agreement & accuracy estimates**
- **complete reproducibility code**
- **statistical and exploratory insights**

We hope this dataset supports ongoing research in:
- online safety
- misinformation
- adaptive AI
- abusive language detection
- social computing
- public health communication

---

End of README.md
