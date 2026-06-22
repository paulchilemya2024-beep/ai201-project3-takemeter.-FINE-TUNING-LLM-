# Fine-Tuning Plan for Tech Twitter Quality Classification

## Project Goal
Build a classifier that reads short tech/social posts and predicts whether the post is:
- `low_quality_take`
- `medium_quality_take`
- `high_quality_take`

The goal is to judge the quality of the argument and reasoning, not the topic itself.

---

## Label Definitions

### `low_quality_take`
Use this when the post is mostly hype, bait, vague claims, or shallow opinion with little evidence.

Signals:
- No clear reasoning or evidence
- Heavy engagement bait or clickbait
- Very broad statements like “AI will replace everything”
- Claims that sound confident but are unsupported

Example:
> “If you’re not using AI for everything right now you’re already obsolete 😂”

---

### `medium_quality_take`
Use this when the post shows some reasoning, context, or insight, but still lacks depth or precision.

Signals:
- Some useful perspective or observation
- A mix of good thinking and weak claims
- Reasonable but not fully grounded
- The post is interesting, but not especially rigorous

Example:
> “Most devs overestimate how much AI will replace them and underestimate how much it will change their workflow.”

---

### `high_quality_take`
Use this when the post is specific, grounded, and clearly insightful.

Signals:
- Strong reasoning or evidence
- Specific examples, numbers, tradeoffs, or clear logic
- Helpful framing that teaches or clarifies something real
- A take that a thoughtful tech audience would save or quote

Example:
> “Most ‘AI will replace devs’ takes ignore that 80% of enterprise code is glue work between legacy systems, where context and ownership matter more than raw coding speed.”

---

## Data Preparation Rules

1. Keep the raw post text in the dataset exactly as written.
2. Do not label based on whether the post is popular or viral.
3. Judge the quality of the argument, not the brand, company, or hype level.
4. Use the same label format every time.
5. If a post is unclear, choose the label that best matches the reasoning quality.

---

## Suggested Dataset Format
Each example should look like:

```json
{
  "text": "post text goes here",
  "label": "high_quality_take"
}
```

The training data should include a good mix of:
- strong, evidence-based posts
- medium posts with partial insight
- low posts that are hype-heavy or shallow

---

## Fine-Tuning Workflow

1. Build and clean the dataset
   - Remove empty or malformed entries
   - Make sure each text has a valid label

2. Split the data
   - Keep a training set for fine-tuning
   - Keep a validation set for checking performance
   - Optionally hold out a test set for final evaluation

3. Choose a model
   - Start with a lightweight transformer that can handle text classification well
   - Compare performance against a prompt-only baseline

4. Fine-tune
   - Train on the labeled examples
   - Track loss and validation accuracy
   - Watch for overfitting if the dataset is too small

5. Evaluate
   - Measure accuracy overall
   - Check per-label performance
   - Review mistakes to see whether the model is confusing hype with medium quality posts

---

## Success Criteria
A successful model should:
- correctly separate shallow hype from thoughtful takes
- be consistent when posts use similar wording but differ in reasoning quality
- avoid overreacting to buzzwords alone

---

## Notes for the Project
This project is not just about finding “good” topics. It is about detecting how well a post argues its point. That means the best examples are usually ones that are specific, grounded, and clear.
