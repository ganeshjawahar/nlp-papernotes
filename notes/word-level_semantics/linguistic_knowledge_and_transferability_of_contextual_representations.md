## [Linguistic Knowledge and Transferability of Contextual Representations](https://arxiv.org/abs/1903.08855)

TLDR; This paper mainly asks three questions: what features of language do  capture/miss, how and why does transferability vary across different layers of CWR and how does the choice of pretraining affect the vector's linguistic knowledge and transferability. The linear models trained on top of frozen CWR are competitive with task-specific SOTA models in many cases, but fail on tasks requiring fine-grained linguistic knowledge. Higher layers of RNNs are more task-specific, while transformer layers do not exhibit the same monotonic trend. For any task, pretraining on a closely related task yields better performance than language model pretraining (which on more data gives the best results).  

#### Key Points
- CWR are outputs of neural nets trained on tasks with large datasets, such as MT and LM. Their broad success indicates that they encode useful, transferable features of language. Still, their linguistic knowledge and transferability are not understood well.
- Study CWR with 16 probing tasks to assess phenonmena such as coreference, knowledge of semantic relations, entity info. and so on.
  - Token Labeling: PoS (basic syntax), CCG (fine-grained syntactic roles), ...
  - Segmentation: Chunking, NER, ...
  - Pairwise relations: Arc prediction, Arc classification, ...
- CWR: 
  - ELMo original, 4-layer, transformer.
  - OpenAI GPT
  - BERT base, cased and large, cased
  - Compared to GloVe
- Linear Probing Results:
  - Non-contextual baseline (Glove) vs CWR vs Task-specific Model (TSM, arefully tuned)
  - CWR >> Glove 
  - CWR is competitive with TSM
  - GPT < ELMo, BERT => Bidirectionality is crucial.
  - CWR do not capture much transferable information about entities and coreference phenomena.
- Probing failure: Probing models lack of capacity vs CWR's inability
  - Experiment with contextual probing model (task-trained LSTM) or replace linear probing model with MLP (just add more parameters) along with full-featured model (FFM, upper bound perf.).
  - Focus on ELMo only.
  - Include tasks requiring highly specific syntactic knowledge like NER, grammatical error detection, ...
  - Adding more parameters leads to significant gains over linear probing model.
  - For 2 tasks, task-trained model > MLP => CWR doesn't capture the info. since that info. can be learned by a task-specific contextualizer.
  - Task-specific contextualization (TSC) is important when the end task requires specific info. that may not be captured by pretraining task. Still unclear which TSC method to use: fine-tuning or using frozen features as inputs to a task-trained contextualizer.
- Layerwise Transferability:
  - Model layers vs. Probing Task
    - 1st layer of RNN based contextualizer: ELMo (original and 4-layer) is consistently transferable.
    - No single most-transferable layer, the best layer varies for task and it's in the middle somewhere.
  - Task-specificity of each ELMo layer for the pretraining task (biLM) as a probing task
    - Take pretrained representation from each layer and relearn the LM softmax classifiers.
    - Expect biLM perplexity to be low for layers containing more info. about the pretraining task.
    - Higher layers in RNN achieve low perplexity (task-specific) but transformer model do not follow such trend.
    - Best layer for LM are worst for probing => Contextualizer layers trade off between encoding general and task-specific features.
    - Reveals why gradual unfreezing from higher to lower layers makes sense. Lower layers are highly transferable and requires less fine-tuning compared to higher layers.   
- Transferring between tasks:
  - Understand how the choice of pretraining task affects the linguistic knowledge and transferability.
  - ELMo model vs random ELMo vs original ELMo vs GloVe
  - Train data = PTB data.
  - Pretraining tasks = Probing tasks + biLM, Target tasks = Probing tasks
  - biLM pretraining is most effective.
  - Target tasks gets benefitted by training from related tasks.
  - original ELMo is the best => pretrain on large corpora => unsupervised pretraining is vital.

#### Thoughts
It was interesting to see the linguistic features that CWR lack (e.g., entity info.) thereby proposed suggestions like augmenting CWR with explicit entity representations look very promising. 

