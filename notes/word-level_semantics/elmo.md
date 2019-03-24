## [Deep contextualized word representations](https://arxiv.org/abs/1802.05365)

TLDR; The authors propose deep contextualized word representation that can capture syntax and semantics of word and usage of a word across linguistic contexts. This representation is obtained from the activations of a biLM trained on a large text corpus. The downstream models can select the information from the different layers pertinent to the target task. The result is that this setup improves the state of the art across several challenging NLP problems like QA, TE and SA.

#### Key Points
- Build high quality representation that can model complex characteristics of word use (e.g., syntax and semantics) and usage of word across linguistic contexts (to model polysemy). It should be integrated easily into existing models like Word2vec.
- Embeddings from Language Models (ELMo)
  - Vectors derived from biLSTM-LM trained on large text corpus
  - Vectors are function of the entire input sentence.
  - Vectors are deep, since they are a function of all the internal layers of the biLM.
  - During integration, the existing model learns a linear combination of different frozen layers, which improves over just using top LSTM layer for representing the input sentence.
  - Data: 1B Word Benchmark
  - biLM: LSTM, character-level, layers=2, epochs=10
  - fine-tune biLM using sentences from the target task (domain transfer).
- Extrinsic Evaluation
  - Downstream tasks: Question Answering (QA), Textual Entailment (TE), SRE, Coreference resolution (CR), NE extraction (NER), Sentiment Analysis (SA)
  - Simply adding ELMo establishes a new SOTA with relative error reduction from 6-20% over strong base models.
- Alternate layer weighting schemes
  - Previous approaches: Last layer of biLM or MT encoder.
  - Lambda to regluarize ELMo weights is crucial. Lambda=1 ~ weight average and small Lambda let layer weights to vary.
  - Tasks: SQuAD, SNLI and SRL
  - All layers task-specific avg. > layer avg. > Last Only
  - Small lambda is preferred in most cases to use ELMo.
- Where to include ELMo?
  - ELMO as input to the lowest layer of biRNN vs. output of biRNN.
  - Tasks: SNLI, SQuAD and SRL.
  - Input + Output: Useful for SNLI and SQuAD as attention layers after biRNN can directly use biLM's internal representation. Not useful for SRL as task-specific context representations are likely more important than those from the biLM.
- Information captured by biLM's representations?
  - Nearest neighbors of polysemous words like play using ELMo shows that ELMo can disambiguate both PoS and word sense in the source sentence.
  - Probing tasks: Word Sense Disambiguation (WSD) and PoS tagging 
  - Higher-level LSTM states capture context-dependent aspects of word meaning (e.g. features useful for word sense disambiguation tasks).
  - Lower-level LSTM states model aspects of syntax (e.g. features useful for PoS tagging).
  - Exposing all of the layers is highly beneficial allowing the learned models select the types of semi-supervision that are most useful for each end task.
- Sample efficiency
  - In terms of number of parameter updates to reach SOTA and training set size.
  - SRL: 10 (with ELMo) vs 486 epochs (without ELMo) to reach maximum dev F1 score.
  - Less training samples: ELMo improvements are large for smaller training sets.  
- Visualization of learned weights:
  - ELMo in input: Task model favors 1st biLSTM layer. Strongly favored for CR and SQuAD.
  - ELMo in output: Relatively balanced, with a slight preference for the lower layers.

#### Thoughts
This is a seminal work to introduce contextualized models which has since inspired a stream of pretraining works with better architectures. However, it's unclear how to incorporate ELMo to a Seq2Seq task such as MT and Summarization.  

