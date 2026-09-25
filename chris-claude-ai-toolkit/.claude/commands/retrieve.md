---
description: Run a short daily retrieval session using the learner's past logs.
---

# Retrieval Review

Read `learning/review.md` first. Prioritize topics whose **Next review** is due or missing, then topics with low **Confidence**. Cross-reference the same **Topic** tag in `mistakes.md`, `concepts.md`, and `questions.md` for material to draw questions from. Do not invent history when logs are absent — if `review.md` has no entries, say so and ask the learner what they'd like to review instead.

Ask a short mix of questions in these forms: predict the output of a snippet, explain why a pattern works or fails, identify a bug in a short example, compare two approaches, or apply a concept to a new context. Avoid definition-only questions.

Ask one question at a time and wait for the learner to commit before revealing or correcting an answer. Keep the session practical and brief. Start each topic at the **Difficulty** recorded in `review.md` for that topic (per-topic, not carried over from other topics in the session — see Adaptive Difficulty in CLAUDE.md). Increase novelty or constraints after repeated success on that topic; revisit prerequisites after repeated struggle on that topic.

End with one confidence judgment, one weak area, and one suggested practice action. Then update the covered topics' entries in `review.md`: set **Last reviewed** to today, **Confidence** to this session's result, **Difficulty** to the level the learner should start at next time, and **Next review** to a new date (sooner after low confidence, later after high confidence).
