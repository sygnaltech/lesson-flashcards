# Feature 2 — Leitner System

## Requirements

- Cards are organised into 5 boxes (Box 1 = review daily, Box 5 = review rarely)
- All cards start in Box 1
- When a card is answered **correctly**, it moves up one box (Box 1 → 2, etc.)
- When a card is answered **incorrectly**, it moves back to Box 1
- Cards in higher boxes appear less frequently in the rotation
- The UI shows which box the current card is in
- Progress persists across page refreshes (use `localStorage`)

---

## Data Schema

This feature requires a **schema migration**. Each card now needs extra fields to track its position in the Leitner system.

**Old shape (Feature 1 localStorage):**
```json
[1, 4, 7]
```
*(just an array of mastered IDs)*

**New shape — `flashcard_progress` in localStorage:**
```json
[
  {
    "id": 1,
    "boxNumber": 2,
    "lastReviewed": "2024-03-15T10:30:00.000Z",
    "successRate": 0.75
  }
]
```

**Fields:**

| Field          | Type   | Description                                              |
|----------------|--------|----------------------------------------------------------|
| `id`           | number | Matches the card `id` in `seed.json`                    |
| `boxNumber`    | number | 1–5. All cards initialise at 1                          |
| `lastReviewed` | string | ISO timestamp of the last time this card was answered   |
| `successRate`  | number | 0–1. Running ratio of correct answers for this card     |

**Migration strategy:**
On load, check for the old `flashcard_mastered` key. If found, treat those cards as Box 5 (mastered), migrate them into the new format, and delete the old key.

---

## Analysis

**Problem:** The app now needs a scheduling algorithm, not just a mastered flag. The Leitner system is a well-known spaced repetition method — cards you struggle with appear more often; cards you know well appear less often.

**Key decisions:**
- Replace the old `flashcard_mastered` localStorage key with a richer `flashcard_progress` key
- Box review intervals (suggested):
  | Box | Review frequency |
  |-----|-----------------|
  | 1   | Every session   |
  | 2   | Every 2 sessions|
  | 3   | Every 4 sessions|
  | 4   | Every 8 sessions|
  | 5   | Mastered — skip |
- "Correct" and "Incorrect" replace the old single "Mastered" button — the card flip should now show both options
- `successRate` is for display only at this stage (no logic depends on it yet)

**Risks / edge cases:**
- Must migrate old Feature 1 data — students who completed F1 should not lose their progress
- If a card has never been reviewed, it has no entry in `flashcard_progress` yet — treat it as Box 1
- Box 5 cards are excluded from rotation (same behaviour as "Mastered" in F1)

---

## Plan

1. Replace `flashcard_mastered` localStorage key with `flashcard_progress` (array of progress objects)
2. Write a `getProgress(id)` helper that returns a card's progress object, or a default `{ boxNumber: 1, lastReviewed: null, successRate: 0 }` if not found
3. Write a `saveProgress(id, correct)` function that:
   - Increments or resets `boxNumber` based on `correct`
   - Updates `lastReviewed` to `new Date().toISOString()`
   - Recalculates `successRate` as a rolling average
   - Saves the updated array back to `localStorage`
4. Update `activeCards` filtering to use box-based scheduling (exclude Box 5; weight lower boxes higher)
5. Replace "Mastered ✓" button with "Got it ✓" and "Missed it ✗" buttons — shown after the card is flipped
6. Display the current box number in the card UI (e.g. "Box 2 of 5")
7. Write a `migrateFromV1()` function that checks for `flashcard_mastered`, converts it to `flashcard_progress` entries with `boxNumber: 5`, then deletes the old key
8. Call `migrateFromV1()` on app load before any other logic runs
