## [Short-term meaning shift: an exploratory distributional analysis](https://arxiv.org/abs/1809.03169)

TLDR; This paper studies short-term meaning shift in a Reddit community. They highlight the challenges faced by Word2vec based models in this setting. In particular, these models detect most meaning shifts, but overgeneralizes for cases where contextual changes do not correspond to a meaning shift. To remedy this, they propose to use contextual variability.

#### Key Points
- Existing works: Focus on meaning shift in long periods of time. Ignore the meaning shift in shorter time spans (e.g. new meanings in relatively small communities of people).
- Challenges: Hard to observe in standard language like books, news as it takes longer time to be widely accepted.
- Experimental Setup
  - Collect a new dataset from LiverpoolFC subreddit. t1 = 2011-2013; t2 = 2017-Present
  - Model initialization: SkipGram trained on random crawl from Reddit in 2013. (community-independent)
  - Train separately for t1 and t2 after initialization.
  - Vocabulary: intersection of 2013 (freq>20), t1 and t2 (all words).
  - Evaluation dataset
    - Sample words with significant increase in frequency between t1 and t2.
    - Frequency change is positively correlated but not a necessary condition for meaning change.  => Dataset is thus biased towards precision over recall.
    - Annotate for semantic change (change in ontological type of what a word denotes), considering new senses.
    - 97 words, 26 annotators, Semantic Shift Index (SSI) label=%shift
- Results and Analysis
  - Positive correlation between Cosine Distance (CD) and SSI
  - e.g. F5 - refresh a news vs. the key in keyboard
  - Systematic Deviations
    - False positives
      - Words that do not undergo semantic shift but with large cosine distance.
      - Error cause: Referential effect (words used exclusively to refer a specific person or event, context of use is narrowed down with respect to t1).
      - e.g. 'stubborn' in context of football coach, which was not there in 2013 but only in 2017.
      - e.g. 'independence' for the political events of Catalunya.
      - Model is not sensitive to the nature of these words, whose meaning hasn't changed.
      - Not a problem for long-term shift studies, where the referential aspect is diluted against a large number of occurrences.
      - Problem when we study changes with a smaller, community corpora.
    - False negatives
      - Words that undergo semantic shift but not captured by the model.
      - Error cause: extended metaphor, i.e., metaphor is developed throughout the whole text produced by an author.
      - e.g. 'shovel' used in 'welcome aboard, here is your shovel', 'you boys know how to shovel coal': team is referred as a train that is running through the season and supporter is expected to shovel coal (contribution) into the train boiler. 
      - The local context of these words (with metaphoric usage) is similar to the literal one => model doesn't spot it.
  - Contextual Variability (CV)
    - Remedy referential aspect problem. (false +ves)
    - CV(word) = average pairwise cosine distance between different contexts (represented by sum of embeddings of words present in the context) of the word.
    - Fit a Linear Regression model with CD and CV as features to predict SSI.
    - CD and CV are complementary. => investigate in depth the interplay between CV, CD and SSI both for short and long-term shift.

#### Thoughts
This is the first work to formalize the short-term meaning shift problem. It would be interesting to see how to incorporate CV during training of word vectors so that interesting words can be retrieved using simpler metrics based on CD.


