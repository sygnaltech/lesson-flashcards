# Feature 1 — Mastered Button

## Requirements

- A "Mastered" button appears on each flashcard
- Clicking it marks that card as mastered and removes it from the active rotation
- A count of mastered cards is shown (e.g. "3 of 10 mastered")
- If all cards are mastered, show a congratulations message instead of a card
- Mastered state persists when the page is refreshed (use `localStorage`)
- A "Reset" button clears all mastered state and restores all cards

---

## Data Schema

The app now needs to **write** data, not just read it. `seed.json` stays read-only — it is the source of truth for card content. Mastered state is stored separately in `localStorage`.

**seed.json shape (unchanged):**
```json
{
  "id": 1,
  "question": "...",
  "answer": "..."
}
```

**localStorage key:** `flashcard_mastered`

**localStorage value:** a JSON array of mastered card IDs
```json
[1, 4, 7]
```

**Why separate?**
`fetch()` can only read files, not write them. Keeping progress in `localStorage` means the source data is never touched — important for when multiple students use the same machine or the seed file is reset.

---

## Analysis

**Problem:** The app currently only reads data. Adding "Mastered" introduces the first write operation. The key architectural decision is *where* to store that state.

**Key decisions:**
- `localStorage` is the right choice at this stage — zero setup, survives page refresh, scoped to the browser
- Do NOT modify `seed.json` — it should always contain all cards
- Mastered IDs are stored as an array, not as a flag on each card object

**Risks / edge cases:**
- What happens when the last non-mastered card is shown and the user clicks "Mastered"? → Show the congratulations state
- What if `localStorage` has IDs that no longer exist in `seed.json`? → Filter them out on load

---

## Plan

1. On app load, read `flashcard_mastered` from `localStorage` (default to `[]` if absent)
2. Filter `cards` to only those whose `id` is not in the mastered list — call this `activeCards`
3. If `activeCards` is empty, show a congratulations message; otherwise show a random card from `activeCards`
4. Add a "Mastered ✓" button to the card UI
5. On click: add the current card's `id` to the mastered array, save back to `localStorage`, call `showRandomCard()`
6. Update the counter to show `X of Y mastered`
7. Add a "Reset" button that clears `localStorage` and reloads the card state
