# Fine-Tuning Plan for Tech Twitter Quality Classification

## Project Goal
This project is about classifying short tech/social posts by the quality of their reasoning, not by the topic they mention.

The model should predict one label from:
- `low_quality_take`
- `medium_quality_take`
- `high_quality_take`

---

## Community and Why These Labels Matter
The target community is people who follow AI, startup, and developer discourse on Tech Twitter/X: builders, analysts, and curious readers who want to separate hype from real insight. These labels matter because the same topic can be discussed in very different ways, and readers need a quick way to tell whether a post is mostly noise, partially useful, or genuinely valuable.

---

## Label Definitions and Boundary Tests

### `low_quality_take`
Definition: A low-quality take is mostly hype, bait, or vague opinion with little evidence, reasoning, or specific detail.

Clear examples:
1. “If you’re not using AI for everything right now you’re already obsolete 😂”
2. “This new model is going to change the world forever, trust me.”

Borderline / uncertain example:
3. “AI is making software development much easier now, and everyone should be using it.”

Why this boundary matters: this post has some real-world flavor, but it still lacks enough evidence or specificity to move above low quality.

---

### `medium_quality_take`
Definition: A medium-quality take shows some insight or context, but the reasoning is incomplete, partially grounded, or still too general.

Clear examples:
1. “Most devs overestimate how much AI will replace them and underestimate how much it will change their workflow.”
2. “Open-source models are improving fast, but the biggest bottleneck is still getting teams to use them well.”

Borderline / uncertain example:
3. “AI is useful for coding, but it still needs human review and good product thinking.”

Why this boundary matters: this is stronger than low quality because it has a real claim, but it is still not specific enough to be high quality.

---

### `high_quality_take`
Definition: A high-quality take is specific, grounded, and clearly useful because it gives a strong argument, evidence, tradeoff, or concrete example.

Clear examples:
1. “Most ‘AI will replace devs’ takes ignore that 80% of enterprise code is glue work between legacy systems, where context and ownership matter more than raw coding speed.”
2. “The biggest moat in AI is not the model itself; it is the feedback loop, data pipeline, and trust layer that make the product usable every day.”

Borderline / uncertain example:
3. “AI tools are becoming more reliable, but the best teams still need good evaluation and careful deployment.”

Why this boundary matters: this post is thoughtful, but it still needs enough concrete detail to clearly deserve the high-quality label.

---

## Mutual Exclusivity Check
These labels are designed to be mutually exclusive by asking one question first: how much specific reasoning does the post actually provide?

- If the post is mostly hype or unsupported opinion, label it `low_quality_take`.
- If it has some real insight but is still broad or incomplete, label it `medium_quality_take`.
- If it gives a clear argument, concrete detail, or strong evidence, label it `high_quality_take`.

The main overlap to watch for is posts that sound smart but do not actually say much. Those should usually stay in `medium_quality_take` unless they clearly provide strong reasoning, evidence, or examples.

---

## Data Preparation Rules
1. Keep the raw post text exactly as written.
2. Judge the argument quality, not the topic or the popularity of the post.
3. Use exactly one label per example.
4. Prefer consistency over trying to reward every clever phrase.
5. When unsure, use the boundary rules above and choose the label that best fits the reasoning quality.

---

## Suggested Dataset Format
Each example should follow this pattern:

```json
{
  "text": "post text goes here",
  "label": "high_quality_take"
}
```

The dataset should include a balanced mix of:
- clearly low-quality hype posts
- partially insightful posts
- genuinely strong, grounded posts

---

## Fine-Tuning Workflow
1. Clean the dataset and remove malformed entries.
2. Split the data into training, validation, and test sets.
3. Fine-tune a text classifier on the labeled examples.
4. Track accuracy and per-label performance.
5. Review mistakes to see whether the model is confusing hype with weak-but-useful arguments.

---

## Success Criteria
A good model should:
- separate shallow hype from thoughtful analysis
- remain consistent on borderline examples
- avoid overreacting to buzzwords without real reasoning

