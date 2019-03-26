## [Dissecting Contextual Word Embeddings: Architecture and Representation](https://arxiv.org/abs/1808.08949)

TLDR; This paper studies how the choice of neural architecture affects the downstream task accuracy and qualitative properties of Contextual Word Representations (CWR) that are learned. All architectures learn high quality contextual representations that outperform word embeddings for four challenging NLP tasks. Also, they learn a hierarchy of information: morphology (word embedding layer), local syntax (lower contextual layer) and longer range semantics such as coreference (upper layer).

#### Key Points
- Problem
  - Despite large gains from CWR, we do not understand why or how pretraining works in practice.
  - ELMo uses LSTM-based biLMs, yet there is no prior reason to believe this is the best possible architecture. => Can try CNN, Transformer
- Architectures for deep biLM
  - Test if architecture choice is important for the quality of CWR.
  - Models: LSTM (2 and 4 layer), Transformer, Gated CNN; Equally skilled biLMs as measured by perplexity.
  - Data: 1B Word Benchmark
  - Results
    - First time that Transformer provides competitive results for LM.
    - Speed: Transformer, CNN > LSTM (3-5X over 2-layer LSTM) => Faster architecture allow training to scale to large unlabeled corpora (useful especially for syntactic tasks).
- Evaluation as word representations
  - Test directly CWR on downstream tasks without finetuning.
  - Tasks: MultiNLI, SRL, Constituency Parsing (CP), NER
  - Results
    - LSTM perform the best for all tasks.
    - Transformer, CNN, LSTM are competitive => Regardless of the architecture choice, they learn good quality CWR.
    - They all outperform GloVe. 
- Intrinsic properties of contextual vectors
  - Contextual Similarity
    - Intra-sentence similarity
      - Similarity between all pairs of words in single sentence for 4-layer LSTM.
      - Lower layer capture local info. while top layer capture longer range relationships.
      - Lower layer place words from the same syntactic constituents ('the russian government') in similar parts of the vector space. 
      - Upper layer: All the verbs have high similarity (PoS info.) and model learns to perform coreference resolution (associating 'it' to 'government').
    - Span representations
      - Vector: Concatenate first and last context vector using element wise product and difference.
      - Data: CoNLL 2000 chunking
      - Visualize 1st layer of 4-layer LSTM: Span representation captures elements of phrasal syntax.
    - Unsupervised pronomial coref
      - Design a new experiment to perform unsupervised coreference resolution.
      - Transfomer performs the best.
      - biLM capture number agreement across coreferent clusters.
      - Accuracies are high in upper layer of each model. => Longer range info. is captured there.
  - Context independent word representation
    - Word analogy task introduced in Mikolov et al.
    - Compare word embedding layer from the biLMs to word vectors.
    - Performance of biLM has good syntactic accuracies, bad semantic accuracies. => Word embedding layer encodes morphology with little semantics.
  - Probing contextual info.
    - PoS tagging, CP
    - biLM learn a hierarchy of info. 
      - Lower layer: span-based syntax, PoS
      - Higher layer: constituent structure
  - Learned layer weights
    - Plot softmax-normalized layer weights for each biLM.
    - NER, MultiNLI: Focus on layers below the top layers as the most transferable CWR occur in the middle layers, while the top layers specialize for LM. 

#### Thoughts
As a natural extension to this work, it would be interesting to study the linguistic information encouraged by different pretraining objectives such as LM, MT, masked LM, NSP as well as multi-task objectives. It would be challenging to tease apart the information naturally encouraged by the neural architecture from that of the learning objective.


