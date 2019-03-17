## [To Tune or Not to Tune? Adapting Pretrained Representations to Diverse Tasks](https://arxiv.org/abs/1903.05987)

TLDR; The authors study two common forms of adaptation, Feature Extraction (FE) and direct FineTuning (FT) of the pretrained model. They explore the best adaptation mechanism for a downstream task given a pretrained model (ELMo/BERT). They show that the relative performance of FE vs. FT depends on the pretraining and target tasks. They establish a set of adaptation guidelines for the NLP practitioner. 

#### Key Points
- Experiments are performed on seven diverse tasks including NER, NLI, SA, STS and paraphrase detection.
- FE setup:
  - (1) ELMo-style linear combination of the words from all layers. 
  - (2) SA (Bi-attentive network), Sentence pair (ESIM model) and NER (BiLSTM + CRF)
- FT setup
  - (1) ELMo - SA (Max-pool + Softmax), Sentence pair (Cross-sentence bi-attention + pool + Softmax), NER (CRF)
  - (2) BERT - SA, Sentence pair (following Devlin et al.), NER (first word piece for each token + Softmax)
- FE<FT between closely aligned tasks (next-sentence prediction in BERT and STS tasks) and FE>FT for distant tasks (LM in ELMo and Sentence pair tasks).
  - (1) ELMo - Large differences for Sentence pair tasks where FE>FT significantly.
  - (2) BERT - FE<FT significantly for STS tasks, with smaller differences for the others.
- Modeling pairwise interaction
  - (1) ELMo + FT + Bi-attention > ELMo + FT. LSTM has difficulty modeling the pairwise interactions.
  - (2) BERT + joint encoding of sentences > BERT + separate encoding
- Impact of additional parameters (biLSTM, CRF, gradual unfreeze) for NER: Useful for FE than FT.
- ELMo fine-tuning: Requires careful hyper-parameter tuning. Once tuned for one task, other tasks have similar hyper-parameters.
- Impact of target domain: No significant correlation between BERT data (books + wikipedia) and each MNLI domain.
- Correlating representations at different layers with linguistic property (diagnostic classifer and EDGE based mutual information):
  - Knowledge for single sentence classification tasks seems mostly concentrated in the last layers.
  - Sentence Pair tasks gradually build up info. in the intermediate and last layers of the model.

#### Thoughts






