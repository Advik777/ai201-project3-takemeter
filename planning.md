# Project Planning: TakeMeter (r/soccer)

## 1. Community Description
I have chosen r/soccer, the largest football community on Reddit. It is an ideal fit for a classification task because the discourse is extremely varied. On any given day, the front page contains a mix of deep tactical breakdowns, bold opinions, and thousands of high volume, emotional reactions. This variety creates clear, distinct patterns in text that a model can learn to distinguish.

## 2. Label Taxonomy

### tactical_analysis
*   **Definition:** A structured argument regarding team performance, formations, or player statistics supported by verifiable evidence.
*   **Example 1:** "Arsenal’s high press forced 12 turnovers in the final third today, significantly higher than their season average of 8."
*   **Example 2:** "Looking at the heatmaps, Salah is cutting inside much earlier this season to accommodate the new overlapping fullback."

### fan_reaction
*   **Definition:** Immediate, emotional responses to specific match events such as goals, red cards, or VAR decisions.
*   **Example 1:** "GOOOOOOAL! I CAN'T BELIEVE HE SCORED THAT!"
*   **Example 2:** "That is never a penalty in a million years. The game is officially gone."

### hot_take
*   **Definition:** A bold, provocative opinion about a player or team’s overall quality stated without supporting evidence or specific match context.
*   **Example 1:** "Haaland is just a glorified poacher who would struggle in a mid table side."
*   **Example 2:** "The Italian league is currently much higher quality than the Premier League, people just won't admit it."

## 3. Hard Edge Cases

### Case 1: "Sack the manager now!"
*   **Ambiguity:** This sits between fan_reaction and hot_take.
*   **Handling:** I used a **time-based rule**: if the comment was found in a live match thread or within 2 hours of a game ending, it is a fan_reaction. If found in a standalone discussion thread or mid-week, it is a hot_take.

### Case 2: "He played well today because he stayed wide."
*   **Ambiguity:** This sits between tactical_analysis and fan_reaction.
*   **Handling:** I labeled this as fan_reaction because it lacks specific statistical evidence, heatmaps, or deeper tactical context. It is a simple observation of play rather than a structured argument.

### Case 3: "Messi had to remind everyone who the goat is."
*   **Ambiguity:** This sits between fan_reaction and hot_take.
*   **Handling:** I labeled this as hot_take. Even if posted during a match, it is a broad claim about a player's all-time status rather than an emotional reaction to a specific goal or save.


## 4. Data Collection Plan
*   **Source:** I will collect examples directly from r/soccer.
*   **Target:** 201 examples total, aiming for 67 examples per label to ensure a balanced dataset.
*   **Underrepresentation Strategy:** If tactical_analysis is underrepresented, I will search for keywords like "xG," "tactical," or "breakdown."

## 5. Evaluation Metrics
*   **Accuracy:** To see the overall percentage of correct predictions.
*   **F1-Score (Per Class):** Critical to ensure the model isn't just over predicting the easiest class (like fan_reaction).
*   **Confusion Matrix:** To identify if the model struggles to distinguish between fan_reaction and hot_take.

## 6. Definition of Success
A successful classifier would have an overall F1 score of 0.75 or higher. This would be "good enough" for a community tool that filters "High Effort" (Analysis) vs "Low Effort" (Reaction) posts.

## 7. AI Tool Plan
*   **Label Stress-Testing:** I will use Claude to generate 5-10 "borderline" r/soccer posts. If I cannot easily classify them using my current rules, I will refine the definitions before starting annotation.
*   **Annotation Assistance:** I will use a zero shot prompt with Groq (Llama 3) to suggest initial labels for the first 50 examples. I will manually review 100% of these to ensure accuracy and will mark them as "AI assisted" in my internal tracking.
*   **Failure Analysis:** After training, I will feed the model's "wrong" predictions into Claude to identify systematic patterns. I will then manually verify these patterns for the final report.

## 8. Stretch Features
*   **Error Pattern Analysis:** I will go beyond just listing 3 wrong predictions. I will analyze the full set of misclassifications to identify systematic patterns such as the model struggling with sarcasm, short comments, or specific club biases—and document these findings in my final evaluation.