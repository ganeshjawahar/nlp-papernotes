## [Learning to Speak and Act in a Fantasy Text Adventure Game](https://arxiv.org/abs/1903.03094)

TLDR; The authors propose a large-scale text adventure game as a platform to study grounded dialogue. Agents can perceive, emote, act while conducting dialogue with other agents. They show that grounding on the information from the local environment such as description of location, objects and characters allows both generative/retrieval models to predict the agent behavior and dialogue well.

#### Key Points
- Hypothesis: Dialogue agents embodied in a rich and cohesive world can be trained to use language well than those exposed only to large-scale text-only corpora.
- LIGHT platform:
  - Multi-player fantasy text adventure world designed for studying situated dialogue,
  - interaction between human, models as embodied agents and the world itself.
  - An episode consists of character-driven human-human interaction which includes action, emote and dialogue.
  - Almost all the aspects of the platform such as locations, objects, characters, dialogues, utterances, emotes and actions are crowd-sourced by human annotators thereby inheriting natural language properties such as ambiguity and coreference.
- Different Aspects:
  - Location (e.g. graveyard): Category, Description, Backstory, Neighbors, Characters, Objects
  - Character (e.g. gravedigger): Persona, Description, Carrying, Wearing, Wielding
  - Object (e.g. shovel): Name, Description, Tags
  - Physical Action (e.g., put, get, give, ...): Each action  has an explicit unambiguous effect on the underlying game state. Action ('put robes in closet') can be executed only if constraints are met, e.g. if the agent is holding the robes.
  - Emote (e.g. wink, cry, blush, ...): Notify nearby characters, which can affect their behavior.
  - Interaction: 
    - Place 2 characters in a random location
    - Character has access to their persona, location and object description.
    - 10,777 dialogues.
    - Unseen Test Set: Locations in this set don't overlap with those in the train/valid set.
- Learning Methods
  - Ranking:
    - Random baseline: Select a random answer candidate.
    - IR: TF/IDF weighing
    - Starspace: BOW for context to maximize inner product with true label with a ranking loss.
    - FastText: to predict emote
    - Transformer Memory: Use dialogue context to attend over grounding info.
    - BERT Bi-Ranker: Embed context and candidate separately.
    - BERT Cross-Ranker: Concatenate context and candidate before embed. (~11K slower than Bi-Ranker)
  - Generative: based on Transformer Memory Network
  - Measure: Recall@1/20 for ranking (19 candidates randomly chosen), Perplexity, Unigram F1 for generative.
- Results on Dialogue, Action and Emote
  - Best Models:
    - IR baseline shows non-random performance
    - Starspace > IR
    - Transformer architecture is strong at all tasks.
    - Cross-Ranker > Bi-Ranker
    - Human performance is still above all these models => scope for future improvements.
    - Generative model did not work well.
  - Generalization Capability on Unseen Test
    - BERT-based models exhibit good transfer ability.
    - 21 and 11 point difference for unseen and test set from human performance.
  - Importance of Grounding
    - Dialogue Task: Having access to all of the environment information provides the best performance.
    - Dialogue + Object: Least improvement because of the long description of the object the model is unable to associate that information to dialgoue, action and emote.
    - Action, Emote Task: Performs poorly when we include all the features due to the enlargement of the input.
    - Dialogue + MISC features > Model with no access to dialogue > Action + Emote features
    - Dialogue + Actions improves results almost everywhere.
    - Qualitative results on how predicted utterances change when the context changes for the same character and dialogue but in a different location.

#### Thoughts


