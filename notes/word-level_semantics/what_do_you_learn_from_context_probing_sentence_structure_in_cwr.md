## [What do you learn from context? Probing for sentence structure in contextualized word representations ](https://openreview.net/forum?id=SJzSgnRcKX)

TLDR; This paper builds on token-level probing work and introduces a novel edge probing task to investigate how CWR encode sentence structure across a range of syntactic, semantic, local and long-range phenomena. CWR trained on LM and MT encode syntactic phenomena strongly, but offer comparably small improvements on semantic tasks over a non-contextual baseline.

#### Key Points
- Problem: understand where CWR excel over conventional word embeddings.
- Questions asked
  - What info. is encoded at each position?
  - How well it encodes structural info. about word's role in the sentence?
  - Is this info. primarily syntactic in nature or do CWR encode higher-level semantic relationships?
  - Is this info. local or do encoders also capture long-range structure?
- Edge probing model
  - Decompose each structured NLP task into a set of graph edges which we can predict independently using a common classifier.
  - Input: Embeddings within given spans (e.g. predicate-argument pair).
  - Output: properties such as semantic roles, which require whole-sentence context.
  - Data: Structured NLP tasks such as tagging, parsing, coreference and semantic roles. (OntoNotes)
  - Models: CoVe, ELMo, GPT, BERT vs. word-level baselines (to separate contribution of context from lexical priors), augmented baselines (to better understand the role of pretraining and ability of encoders to capture long-range dependencies.)
  - Tasks: POS, constituent/dependency labeling, NE labeling, SRL, coreference, semantic proto-role, relation classification.
  - Probing model: Span representations + Concatenation + 2-layer MLP + Sigmoid output.
- Experiments and results
  - Baseline: Word embedding layer from biLM, randomized ELMo (all layers above lexical layer is randomized), Word-level CNN (to factor out local relationships)
  - mix mode: ELMo-style mix of different layers of the model.
  - Comparison of representation models
    - ELMo, GPT (comparable) > CoVe  => info. underlying ELMo, GPT can recover sentence structure.
    - Lexical representations from ELMo, GPT > GloVe => benefit from better handling of morphology by character-level or subword representations. 
    - For BERT and GPT, mix is better than concatenation => most relevant info. is contained in intermediate layers.
    - In mix mode, BERT-large > BERT-base > GPT
    - BERT improvement is not uniform across tasks.
  - Genre Effects
    - Mismatch between probing task data (closer to 1BWB used by ELMo) and GPT training data (Books Corpus).
    - Train GPT on 1BWB.
    - Slightly better, still underperforms ELMo.
  - Encoding syntactic vs. semantic info.
    - Large gains on syntactic tasks than semantic tasks.
    - ELMo encodes local type information. (PoS and entities)
  - Effects of architecture
    - How much peformance can be attributed to architecture rather than knoweldge from pretraining?
    - Compare to orthonormal encoder.
    - Learned weight account for over 70% improvement from full ELMo.
  - Encoding non-local context
    - How much info. is carried over long distances in the sentence?
    - Use CNN of different width 3 and 5.
    - ELMo's info. about constituent is mostly local (as the gap between CNN and ELMo full is closed by 79% with CNN-5).
    - ELMo's improvements are due to modeling long-range info (for semantic tasks, this gap is large).
    - Vary the performance on dependency labeling with distance between token and its head => full ELMo model holds up better, suggesting CWR encode useful long-distance dependencies.


#### Thoughts


