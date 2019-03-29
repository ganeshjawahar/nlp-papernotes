## [Learning from Dialogue after Deployment: Feed Yourself, Chatbot!](https://arxiv.org/abs/1901.05415)

TLDR; The authors propose a dialogue agent with the ability to extract new training examples from the conversations during deployment. If the agent finds the conversation to be going well, the user's responses become new training examples to imitate. If the agent believes it has made a mistake, it asks for feedback and learning to predict the feedback that will be given improves the agent's dialogue abilities further. This agent significantly improves performance regardless of the amount of traditional supervision.

#### Key Points
- Challenges
  - Need of sufficient quantity of supervised data.
  - Data settings are quite different from the deployment environment.
- Self-feeding chatbot
  - Uses abundant, task-specific, dynamic and cheap conversations collected after deployment.
  - Similar to human learning = expert-level conversations + feddback from our own conversations.
  - Jointly predicts the response and speaking partner's satisfaction with its responses.
  - When the human is satisfied, the agent imitates the human responses (Human-Bot Dialogue).
  - Otherwise, it asks for a feedback, predicting it as an auxiliary task.
  - User satisfaction is thresholded to control the rate at which new DIALOGUE or FEEDBACK examples are collected.
  - User responses are always in the form of natural dialogue => there is a learnable relationship between conversation contexts and their corresponding feedback.
- Sub-tasks
  - Dialogue (Next utterance prediction)
    - X: Context of conversation delmited with speaker tokens.
    - Y: response given by the human.
    - Data
      - PersonaChat (initial training) (Human-Human)
      - Human-Bot (where Y is always human)
      - HH and HB can be used together because chitchat domain is symmetric (both human and bot has the same role).
  - Satisfaction
    - X: Same context as previous task
    - Y: satisfaction label (dissatisfied to satisfied)
    - X always ends with the human utterance ('what are you talking about?') which has useful info. than bot utterance to predict user satisfaction.
  - Feedback
    - X: Same context as previous task
    - Y: Feedback utterance.
    - Bot question: Oops! Sorry. What should I have said instead? => Future work should explore asking different kinds of questions.
    - New Example: Conversation till the agent made the poor response and user's response.
    - Reset the bot's history and instruct the user to continue, asking for a new topic.
- Model and Settings
  - Model component
    - Shared by Dialogue and Feedback task.
  - Interface component
  - Each task's cross-entropy loss has a task-specific scaling factor.
  - Architecture: Transformer
  - Dialogue & Feedback: Cast as ranking problems.
- Results
  - Metric: Hits@X/Y
  - Benefitting from Deployment Examples
    - Utilizing deployment examples improves accuracy on DIALOGUE task regardless of the number of supervised HH examples.
    - Dialogue and feedback gives complementary signals.
    - When number of candidates is set to 10K (realistic setting), model with deployment samples > baseline.
    - 20K Feedback examples = 60K HB Dialogue examples => learning from model mistakes is crucial.
    - Including the poor response made by model in HB example degrades model performance.
    - Including the high satisfactory chatbot responses as new training examples is not beneficial.
    - Fresher feedback results in bigger gains.
      - 20K HH + 40K feedback compared to 20K HH + 1st 20K feedback before collecting 20K feedback (slighly better than former)
      - New feedback examples collected based on failure modes of the current model make them more efficient in a manner similar to new training examples selected via active learning.
      - Gains might be further improved by
        - collecting Feedback examples specific to each model.
        - more frequently retraining the MTL model (every 5k instead of 20K).
      - This phenomenon was not there for HB Dialogue examples because they are less targeted at current model failures than Feedback ones.
  - Predicting User Satisfaction
    - Compared to asking for feedback when the model is most uncertain what to say next. (Difficult to recognize one's own mistakes compared to checking if the mistake has already been made.)
    - Predict a mistake 
      - when the confidence in the top rated response is below some threshold.
      - when the gap between the two top rated responses is below threshold.
    - With 1K examples, trained satisfaction classifier outperformed both the uncertainty-based methods and regular expression method based on common ways of expressing dissatisfaction.

#### Thoughts
As pointed out by the authors, the meta-learning themes such as learning which question to ask in a given context to receive the most valuable feedback and alternating between FEEDBACK and SATISFACTION ('did my last response make sense?') look very promising.


