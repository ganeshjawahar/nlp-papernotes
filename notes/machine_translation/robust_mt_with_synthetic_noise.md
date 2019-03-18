## [Improving Robustness of Machine Translation with Synthetic Noise](https://arxiv.org/abs/1902.09508)

TLDR; The authors improve the robustness of the vanilla MT system by emulating naturally occurring noise in addition to clean data. These synthetic noise partially mitigates loss in accuracy resulting from idiosyncrasies of social media like typos, slang, dialect and so on.

#### Key Points
- Challange: MT systems trained on clean parallel data when tasked with UGC exhibit degraded performance.
- Contribution: Synthetic Noise Induction model which introduces types of noise unique to social media text and the introduction of back translation as a means of emulating target noise.
- Data: MTNT French to English for Test; EuroParl (EP) and TED talks for training;
- Baseline Model: biLSTM Seq2Seq with BPE.
- Synthetic Noise Induction (SNI):
  - Inject artificial noise in the clean data based on the distribution of types of noise found in MTNT.
  - Spelling (p=0.04) = randomly add or drop a character in a given word.
  - Grammar (p=0.015) and profanity (0.007) = Randomly select and insert a stop word or an expletive and its translation on either side.
  - Emoticon (p=0.002) = Randomly select an emoticon and insert it on both sides.
- Noise Generation through Back-Translation:
  - Un-tagged back-translation (UBT): Train baseline en-fr and fr-en model using TED. Feed 100K french sentences from EP and pass it through en-fr followed by fr-en. Imperfect translation from intervening MT systems makes the resulting translation inherently noisy.
  - Tagged back-translation (TBT): Leverage the particular style of MTNT corpus. Train en-fr and fr-en using both TED and MTNT. Append a tag in front of sentence to denote the origin data set of each sentence.
- Results:
  - TBT provides the most pronounced increase in BLUE although the baseline model is not fine-tuned with in-domain data.
  - More in-domain training data, higher the BLUE score.
  - Out-of-domain clean data + Noise > Out-of-domain clean data
- Analysis:
  - As you increase noise level for SNI, profanity turns out to have a very negative effect. Random insertion of expletives might need to be replaced with a more nuanced linguistic usage.
  - Qualitative comparison of UBT vs. TBT: TBT style is more informal and follows the style of MTNT.

#### Thoughts
This paper shows that we can synthesize the types of noise common to social media text and improve the vanilla MT system by leveraging artificially generated noise. Their idea to use back translation to simulate noise is very interesting.
