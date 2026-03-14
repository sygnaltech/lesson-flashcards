# Feature Specification Template

> Copy this file and rename it for each feature (e.g. `feature_1_mastered_button.md`).
> Fill in each section **before** prompting the AI to implement anything.

---

## Feature Name

_A short, descriptive name for the feature._

---

## Requirements

> What must this feature do? Write in plain English. Be specific.

- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

---

## Data Schema

> Does this feature add, change, or remove any fields in the data?
> Reference `seed.json` as your baseline.

**Current shape:**
```json
{
  "id": 1,
  "question": "...",
  "answer": "..."
}
```

**New / changed shape:**
```json
{
  "id": 1,
  "question": "...",
  "answer": "...",
  "newField": "..."
}
```

**Migration notes:** _How do existing cards get updated to the new shape?_

---

## Analysis

> What is the problem you are solving? What are the key decisions?

- **Problem:**
- **Key decisions:**
- **Risks / edge cases:**

---

## Plan

> Step-by-step implementation plan. Write this before you touch the AI.

1. Step one
2. Step two
3. Step three

---

## Prompts Used

> After you implement, paste the prompts you gave the AI here for review.

```
[Prompt 1]
```

---

## Notes / Debrief

> What broke? What surprised you? What would you do differently?
