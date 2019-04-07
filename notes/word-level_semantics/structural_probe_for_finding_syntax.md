## [A Structural Probe for Finding Syntax in Word Representations](https://nlp.stanford.edu/pubs/hewitt2019structural.pdf)

TLDR; This paper attempts to test whether syntax trees are represented in their entirety in CWR. They propose a structural probe which evaluates whether syntax trees are embedded in a linear transformation of a NN's word representation space. They show that ELMo and BERT captures entire syntax trees implicitly with high consistency, but not the baselines.  

#### Key Points
- Open Question: Does CWR encode entire parse trees in their word representations?
- Structural Probe
  - Tests whether syntax trees are consistently embedded in a low-rank linear transformation of a NN's word representation space.
  - Tree structure is embedded if the transformed space has the property that squared L2 distance between two words' vectors correspond to the number of edges between the words in the parse tree.
  - To reconstruct edge directions, we hypothesize a linear transformation under which the squared L2 norm correspond to the depth of the word in the parse tree.
  - Uses supervision to find the transformation under which these properties are best approximated for each model.
  - If such transformation exist, they define inner products on the original space under which squared distances and norms encode syntax trees even though the model being probed were never given trees as input or supervised to reconstruct them.
- Experiments
  - CWR: ELMo and BERT (fixed)
  - Baseline
    - Linear: Encodes position of words
    - ELMo0: Char-level (bottom layer) of ELMo; No info. about position.
    - Decay0: Weighted average of all word embeddings with exponential decrease in weight with distance between the words.
    - Proj0: untrained biLSTM.
  - Linear transformation matrix B: Square (full-rank)
  - Tree distance evaluation metric
    - UUAS = Predicted parse tree + MST ~ Gold Parse
    - Spearman correlation = Predicted distance ~ Actual distance
  - Tree depth evaluation metric
    - norm Spearman (NSpr.) - True depth and predicted ordering.
    - root% - ability to identify the root of the sentence as the least deep.
- Results
  - Our probe can't simply "learn to parse" on top of any informative representation.
  - Proj0 performs the best among the baselines and behaves with mostly simple deviations from linearity.
  - BERT-large > BERT-base > ELMo > baselines on both probes.
  - Analysis of linear transformation rank
    - How compactly syntactic information is encoded in the vector space of ELMo and BERT?
    - Larger the rank, more the space devoted to syntax.
    - Effective rank of linear transformation required is surprisingly low. (parsing performance doesn't improve beyond 64 or 128)
    - BERT-base, BERT-large, ELMo surprisingly require transformations of approximately the same rank. Future work to investigate. (@yoavgo suggested the rank is a property of the English parse trees, not of the pretrained models.)
  - Structure of syntax trees emerges through properly defined distances and norms on CWR.

#### Thoughts
@yoavgo suggested to see the extent to which the representation is rich enough to represent "arbitrary" trees, not necessarily syntactic (i.e. left branching, balanced binary, random...), analyze for non-english text, compare different syntactic representations and head choices.


