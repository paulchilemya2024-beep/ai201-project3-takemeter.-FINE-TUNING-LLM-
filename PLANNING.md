# Fine-Tuning Plan for Tech Twitter Quality Classification

## Project Goal
This project is about classifying short tech/social posts by the quality of their reasoning, not by the topic they mention.

The model should predict one label from:
- `low_quality_take`
- `medium_quality_take`
- `high_quality_take`

---

## Community
I chose the AI/startup/developer discourse community on Tech Twitter/X because it produces a wide range of writing styles, from pure hype to genuinely useful analysis. That variety is ideal for a classification task because the same topic can be discussed with very different levels of reasoning, evidence, and specificity.

This community is a good fit because people are constantly sharing opinions about tools, workflows, markets, products, and technical tradeoffs, which gives the dataset enough diversity to make the labels meaningful.

---

## Labels
I will use three labels so the boundary between them stays clear and practical:

### `low_quality_take`
A low-quality take is a post that is mostly hype, bait, or unsupported opinion with little reasoning or evidence.
- Example post 1: “If you’re not using AI for everything right now you’re already obsolete 😂”
- Example post 2: “This model is going to change the world forever, trust me.”

### `medium_quality_take`
A medium-quality take is a post that shows some real insight or context, but still leaves important reasoning incomplete or too general.
- Example post 1: “Most devs overestimate how much AI will replace them and underestimate how much it will change their workflow.”
- Example post 2: “Open-source models are improving fast, but the biggest bottleneck is still getting teams to use them well.”

### `high_quality_take`
A high-quality take is a post that gives a specific, grounded, and useful argument with evidence, examples, tradeoffs, or clear logic.
- Example post 1: “Most ‘AI will replace devs’ takes ignore that 80% of enterprise code is glue work between legacy systems, where context and ownership matter more than raw coding speed.”
- Example post 2: “The biggest moat in AI is not the model itself; it is the feedback loop, data pipeline, and trust layer that make the product usable every day.”

---

## Hard Edge Cases
The hardest ambiguous posts are ones that sound thoughtful but do not really provide enough specific reasoning to justify a high-quality label. For example, a post might mention evaluation, trust, or workflow without offering concrete detail, evidence, or a clear argument.

When I encounter a borderline post during annotation, I will use this rule: if the post is mostly hype or unsupported opinion, it is `low_quality_take`; if it has some insight but still lacks detail, it is `medium_quality_take`; if it provides a clear argument with specifics or evidence, it is `high_quality_take`.

---

## Data Collection Plan
I will collect examples from Tech Twitter/X posts, especially AI, startup, and developer threads, using a mix of manual selection and search-based collection. I will aim for roughly 60–80 examples per label by the time I finish the first annotation pass, and I will expand the dataset until I have at least 200 labeled examples total.

If one label is underrepresented after 200 examples, I will intentionally collect more posts that match that label and add a few boundary cases so the model can learn the difference more reliably.

---

## Evaluation Metrics
Accuracy alone is not enough because the labels are not equally easy to distinguish and some mistakes matter more than others. I will use:
- accuracy, to measure overall correctness
- per-label precision and recall, to see whether the model is missing or over-predicting specific classes
- F1 score, especially because it balances false positives and false negatives for a multi-class problem

These metrics are appropriate because the task is not just about getting the overall answer right; it is about making sure the model can reliably separate hype, partial insight, and strong reasoning.

---

## Definition of Success
A classifier would be genuinely useful if it can correctly separate clearly low-quality posts from clearly high-quality posts most of the time and can handle borderline cases consistently enough that a human reviewer only needs to correct a small number of examples.

For a real community tool, I would consider the model good enough if it reaches strong macro-F1 performance and if its mistakes are concentrated in genuinely ambiguous cases rather than in obvious examples.

---

## Review of Success Criteria
These criteria are specific enough to judge objectively because they define what matters: overall correctness, per-label performance, and whether the remaining mistakes are concentrated in ambiguous cases. At the end of the project, I can measure those numbers directly and decide whether the classifier is useful for real annotation support.

---

## AI Tool Plan

### Label stress-testing
I will ask an AI tool to generate 5–10 posts that sit exactly at the boundary between two labels using the definitions above. If the AI creates posts that I still cannot classify cleanly, I will revise the label definitions before annotating a large batch.

### Annotation assistance
I may use an LLM to pre-label a small batch of examples before I review them myself. If I do this, I will track which examples were pre-labeled so I can disclose that in the final AI usage section.

### Failure analysis
After evaluating the model, I will give the wrong predictions to an AI tool and ask it to identify repeated patterns, such as confusion between medium and high quality posts or overreliance on buzzwords. I will then verify those patterns manually before writing the evaluation conclusions.

---

## Fine-Tuning Workflow
1. Clean the dataset and remove malformed entries.
2. Split the data into training, validation, and test sets.
3. Fine-tune a text classifier on the labeled examples.
4. Track accuracy, precision, recall, and F1 by label.
5. Review mistakes to see whether the model is confusing hype with weak-but-useful arguments.

---

## Success Criteria
A good model should:
- separate shallow hype from thoughtful analysis
- remain consistent on borderline examples
- avoid overreacting to buzzwords without real reasoning

