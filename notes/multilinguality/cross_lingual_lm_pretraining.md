## [Cross-lingual Language Model Pretraining](https://arxiv.org/abs/1901.07291)

TLDR; This work extends BERT for multiple languages and show the effectiveness of cross-lingual pretraining. They propose supervised and unsupervised methods to learn cross-lingual LM (XLM). They show SOTA results on cross-lingual classification, unsupervised/supervised MT, LM for low-resource languages.

#### Key Points
- Existing work: General-purpose sentence representations are monolingual and largely focused around English benchmarks.
- XLM
  - Align distribution of sentences from multiple languages, reducing the need for parallel data.
  - Shared BPE vocabulary learned on subsampled data from all languages. Sampling distribution prevents words of low-resource langauges from being split at the character level.
  - Causal LM (CLM)
    - Transformer trained to predict a word given previous words in a sentence.
  - Masked LM (MLM)
    - Same MLM objective introduced in BERT.
    - Use text streams of an arbitrary number of sentences instead of pair of sentences like in BERT.
    - Subsample to counter imbalance between rare and frequent tokens.
  - Translation LM (TLM)
    - Leverage parallel data when it is available.
    - Input: Concatenate parallel sentences
    - Output: Mask word in both sentences
    - To predict a masked English word, the model can attend to surrounding English words or to the French translation.
    - Encourage the model to align the English and French representations.
    - Leverage French context if English one is not sufficient to infer the masked English words.
  - MLM + TLM: Alternate between two objectives.
- Cross-lingual classification
  - Fine tune XLM on cross-lingual NLI (XNLI) by adding a linear classifier on top of the first hidden state of XLM.
  - MLM sets a new SOTA.
  - MLM + TLM further improves SOTA by 3.6% accuracy.
  - SOTA for low-resource languages like Swahili and Urdu.
  - Fine-tuning on machine translations of XNLI further improves the perf.
- Unsupervised MT
  - Pretrain encoder and decoder with XLM.
  - WMT English to French, German, Romanian
  - MT model: Denoising Auto-Encoding Loss with online back-translation loss.
  - XLM sets a new SOTA.
  - MLM > CLM
  - Pretraining encoder is important than decoder.
- Supervised MT
  - WMT Romanian-English.
  - XLM gives a significant boost. 
  - XLM + Back-Translation achieves the SOTA.
- Low-resource LM
  - Leverage data in similar but higher-resource languages.
  - Hindi -> Nepali
  - Nepali LM vs XLM on Hindi and English
  - Data: Wikipedia
  - Using English data, XLM reduce perplexity by 17.1 points.
  - Using Hindi data also, XLM reduce perplexity by 41.6 points.
  - XML leverages the n-grams anchor points that are shared across languages, for instance in Wikipedia articles.
- Unsupervised cross-lingual word embeddings
  - Metrics: Cosine, L2 distance and cross-lingual word similarity
  - XLM > Muse, Concat
  - XLM have the particularity of being trained together with a sentence cnoder, while Muse and Concat are based on fastText word emebddings.

#### Thoughts
It would be interesting to see the impact of Next Sentence Prediction (NSP) objective introduced in BERT for this setting. NSP objective was crucial to BERT's success.

