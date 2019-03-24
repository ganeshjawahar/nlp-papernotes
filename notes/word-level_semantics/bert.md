## [BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding](https://arxiv.org/abs/1810.04805)

TLDR; The authors introduce a Transformer based contextualizer which is a deep bidrectional representation learned by jointly conditioning on both left and right context in all layers, thanks to the new objective (masked word and next sentence prediction). This is the first model to be directly fine-tuned for a wide range of tasks without substantial task-specific architecture modifications. It establishes the SOTA on 11 different NLP benchmarks.

#### Key Points
- Limitation of existing works: 
  - ELMo uses shallow concatenation of independently trained left-to-right and right-to-left LMs.
  - Unidirectional language models are sub-optimal to be leveraged for sentence-level tasks as well as token-level tasks such as SQuAD where it is crucial to incorporate context from both directions.
- Bidirectional Encoder Representations from Transformers (BERT)
  - Addresses the unidirectional constraints by proposing a new pretraining objective:
    - Masked Language Model (MLM): Randomly mask some tokens from the input and predict the masked word based only on its context. Two caveats:
      - Follow a procedural masking approach to handle the mismatch between pretraining and finetuning. Only 1.5% of all tokens are masked, which does not seem to harm the model's language understanding capability.
      - Only 15% of tokens are predicted in each batch requiring more pretraining steps. Empirical improvements outweight the increased training cost. 
    - Next sentence prediction (NSP): Predict if two sentences in the input occur consecutively in the corpus or not. 
  - Bidirectionality elminates the need of heavily engineered task-specific architectures.
  - 2 variants: Base (similar parameters to OpenAI GPT) and Large
  - WordPiece embeddings, learned positional embeddings, 512 tokens max, special input marker [CLS] (first token used to aggregate sequence representations for classification task) and [SEP] to separate two sentences in the input.
  - Training Data: BooksCorpus + English Wikipedia
  - Finetuning procedure:
    - Classification: Project on [CLS] token.
    - Large data sets were far less sensitive to hyperparameter choice than small data sets.
    - SQuAD: Learn a start vector and end vector.
    - NER: Project the hidden state corresponding to the first sub-token as classifier input.
    - SWAG: Concatenate question with each candidate before embed.
  - GPT compared to BERT:
    - Less data (BooksCorpus)
    - Not sentence markers.
    - Less batch size (32K vs 128K)
    - Same learning rate for all finetuning experiments vs. BERT which chooses task-specific finetuning rate.
- Experiments
  - Baseline: ELMo, OpenAI GPT
  - GLUE
    - Tasks: MNLI, QQP, QNLI, SST-2, CoLA, STS-B, MRPC, RTE, WNLI
    - Base and Large outperforms existing models.
  - SQuAD and SWAG: +0.2 and +27.1%
- Ablation
  - Effect of pretraining tasks
    - No NSP, LTR & No NSP
    - Removing NSP hurts performance significantly on pairwise tasks such as QNLI, MNLI and SQuAD.
    - MLM > LTR => LTR is poor at SQuAD and MRPC as it does not have right-side context.
    - LTR + rand-init biLSTM > LTR for SQuAD.
  - Effect of model size
    - Vary layers, hidden units and attention heads.
    - Tasks: LM, MRPC, SST-2 and MNLI
    - Larger the model, better is the accuracy.
    - First work to demonstrate that scaling the model leads to improvement on very small scale tasks, provided that the model has been sufficiently pretrained.
  - Effect of number of training steps
    - Task: MNLI
    - MLM converge slower than LTR but MLM>LTR almost immediately.
    - BERT-base +1.0% accuracy on 1M steps compared to 500K
  - Feature-based approach with BERT
    - Useful for NLP tasks which can't be represented by a Transformer encoder architecture and also computationally cheap.
    - Task: NER (bert rep + rand-init biLSTM)
    - Best strategy: Concatenate top 4 layers (0.3 F1 behind finetuning the entire model.)
    - BERT is good for finetuning as well as feature-based approaches.

#### Thoughts
This work established solid performance across the board outperforming previous models by a huge margin. However, it looks impossible to retrain the model from scratch without access to TPUs. I hope this bottleneck is removed soon.