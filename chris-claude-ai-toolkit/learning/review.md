# Review

Keep short retrieval prompts or review metadata for meaningful topics.

`/retrieve` reads this file to pick what to ask. Keep **Topic** consistent with tags used in `mistakes.md`, `concepts.md`, and `questions.md` so entries about the same idea can be linked. Keep **Next review** and **Difficulty** current — update them each time `/retrieve` covers this topic rather than adding a new entry.

## YYYY-MM-DD: Topic

- **Topic:** (short tag, e.g. `closures`, `sql-joins` — reuse existing tags across files where possible)
- **Recall prompt:**
- **Last reviewed:**
- **Confidence:** (low / medium / high — how well the learner answered last time)
- **Difficulty:** (easy / medium / hard — starting point for the next `/retrieve` on this topic; does not ratchet upward permanently, see Adaptive Difficulty)
- **Next review:** (date, or "due" if overdue)
