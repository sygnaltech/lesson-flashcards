# Project Requirements

> This is a living document. Update it as the project evolves.
> Each feature stage should be reflected here before you start coding.

---

## Project Overview

A personal knowledge flashcard app for practising AI-related concepts.
Built with plain HTML, CSS, and JavaScript — no frameworks or build tools required.

---

## Current Stage: Base App

### What it does

- Loads flashcard data from `data/seed.json`
- Displays one random flashcard at a time (question side up)
- User can flip the card to reveal the answer
- User can move to the next random card

### What it does NOT do (yet)

- Track which cards have been seen or mastered
- Persist any state between sessions
- Support multiple users

---

## Data Schema (Base)

Each flashcard in `seed.json` has the following shape:

```json
{
  "id": 1,
  "question": "What is a prompt?",
  "answer": "A prompt is the input text you give to an AI model..."
}
```

**Fields:**

| Field      | Type   | Description                     |
|------------|--------|---------------------------------|
| `id`       | number | Unique identifier for the card  |
| `question` | string | The front of the flashcard      |
| `answer`   | string | The back of the flashcard       |

---

## Future Stages

These sections will be filled in as the lesson progresses.

### Feature 1 — Mastered Button
_To be defined._

### Feature 2 — Leitner System
_To be defined._

### Feature 3 — Multi-user / Session Support
_To be defined._
