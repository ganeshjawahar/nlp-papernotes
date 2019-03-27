## [Language Modeling Teaches You More Syntax than Translation Does: Lessons Learned Through Auxiliary Task Analysis](https://arxiv.org/abs/1809.10040)

TLDR; This paper analyzes how the choice of pretraining objective affects the type of linguistic information that model learns. They run probing experiments with LM, MT, Autoencoding and Skip-thought objective by holding the architecture (LSTM) and data constant. They find that LM to be the best pretraining task for transfer learning applications requiring syntactic information. They also find that random LSTM performs well on syntactic auxiliary tasks, but this effect disappears when the amount of training data for the auxiliary tasks is reduced.

#### Key Points
- Questions
  - How does the pretraining task affect how well models learn syntactic properties? 
  - How does the amount of data the model is trained on affect these results?
- Setup
  - Model architecture (LSTM) constant.
  - Data source (WMT 2016) constant.
  - Vary training tasks and amount of training data (subset of original data 5M and a large monolingual corpus 58M).
  - Pretraining task
    - MT (English-German 5M)
    - LM
    - Skip-Thought (ST)
    - Autoencoding (AE)
    - Untrained LSTM (baseline)
  - Probing task (focused on syntax)
    - POS tagging
    - CCG supertagging (building block for parsing as sequence of tags map sentences to small subsets of possible parses)
    - Create 10% and 1% samples to assess data efficiency.
    - Model: MLP (1000D + RelLU), same training details as used for pretraining the encoder.
  - Word Identity
    - Input: LSTM hidden state
    - Output: Identity of the word at different time step
    - e.g. -2 model: 'I love NLP'  state['NLP'] predict 'I'
    - Data: WSJ
    - Target words occur between 100 and 1000 times in corpus.
- Comparing pretraining tasks
  - POS/CCG accuracies increase with more pretraining data, but the marginal improvement decreases with the increase in the amount of training data.
  - LM and MT
    - biLM slightly underperforms Translation for PoS and CCG.
    - biLM trained on small data (!M) outperform models trained on other tasks and data sizes (5M for MT, upto 63M for ST and AE).
    - biLM better for transfer learning of syntactic info. => LM have a per-token loss unlike other models. (and evaluation also predict a single label for each token)
    - biLM > 1000D forward-only LM, for all amounts of training data
    - biLM is important for CCG than PoS.
    - biLM and MT robust to decrease in classifier training data.
  - Skip-Thought
    - ST is the only model to show improvement with increase in pretraining data without plateauing.
    - LM is regularized version of ST since LM is trained on ordered sentences.
  - Untrained LSTM
    - weights: U(-0.1,0.1), bias: 0
    - ULSTM is below LM by few points with full probing data. => memorize word configurations and their associated tags.
    - ULSTM performance drops with decrease in probing data. Even goes below word conditional most frequent class (MFC) baseline (1% data).
    - ULSTM captures neighboring word identity information than any trained model.
  - Autoencoder
    - AE is the only model not to show improvement with increase in pretraining data. => unsupervised autoencoders are prone to learn identity mappings.
    - AE > ULSTM for low auxiliary data regime so they do learn some useful structure.
    - AE don't learn syntactic features as AE < ULSTM in high auxiliary data regime.     
- Comparing layers
  - Layer 0 (word embedding)
    - ULSTM ~ MFC on POS/CCG with all auxiliary classifier data => auxiliary classifier are learning to memorize and classify the random vectors.
    - Trained models > ULSTM => smaller amounts of classifier data.
  - Upper layer
    - PoS info. stored in lower layer for all trained models except biLM as well as untrained model. => PoS info. is stored in the lower layer, not necessarily because the training task encourages this, but due to the properties of LSTM architecture.
    - Which layer performs best seems to be independent of absolute perf. on the supertagging task.
- Word Identity Prediction
  - Trained encoders < Untrained model => Trained encoders genuinely capture substantial syntactic features, beyond mere word identity that the auxiliary classifiers can use.
  - For all models
    - 1st layer > 2nd layer in predicting identity of immediate neighbors of a word.
    - 1st layer < 2nd layer in predicting identity of distant neighbors of a word.
    - Like CNN, depth in RNN has the effect on increasing the receptive field. => allows upper layers to capture larger context.

#### Thoughts



