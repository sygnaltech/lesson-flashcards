# Feature 3 — Multi-user / Session Support

## Requirements

- On first visit, the app asks for a username (or student ID)
- All progress is stored and retrieved under that username — two students on the same machine have separate progress
- The current username is displayed in the UI
- A "Switch user" option lets someone log out and enter a different name
- Progress still persists across page refreshes for each user
- Existing single-user data from Feature 2 is not destroyed — it should be migrated to a default user

---

## Data Schema

This is the biggest schema change yet. All progress must now be **keyed by username**.

**Old shape — `flashcard_progress` (Feature 2):**
```json
[
  { "id": 1, "boxNumber": 2, "lastReviewed": "...", "successRate": 0.75 }
]
```

**New shape — `flashcard_users` in localStorage:**
```json
{
  "alice": [
    { "id": 1, "boxNumber": 2, "lastReviewed": "...", "successRate": 0.75 }
  ],
  "bob": [
    { "id": 1, "boxNumber": 1, "lastReviewed": null, "successRate": 0 }
  ]
}
```

**Active session — `flashcard_current_user` in localStorage:**
```json
"alice"
```

**localStorage keys summary:**

| Key                      | Value                                      |
|--------------------------|--------------------------------------------|
| `flashcard_users`        | Object keyed by username → progress array  |
| `flashcard_current_user` | String — the currently active username     |

**Migration strategy:**
On load, check for the old `flashcard_progress` key (Feature 2 format). If found, move it into `flashcard_users` under the key `"default"`, set `flashcard_current_user` to `"default"`, then delete the old key.

---

## Analysis

**Problem:** `localStorage` is shared across all users of the same browser on the same machine. Without namespacing by user, one student's progress will overwrite another's.

**Key decisions:**
- Username is a plain string — no passwords, no accounts. Simple enough for a classroom.
- All per-user data lives under one `flashcard_users` key (one object, many users) rather than one key per user — easier to inspect and migrate
- `flashcard_current_user` is a separate key so the app knows who is active without parsing the whole users object
- Username prompt appears on first load (when `flashcard_current_user` is not set) — not on every visit
- "Switch user" clears `flashcard_current_user` and reloads — the next load triggers the prompt again

**Risks / edge cases:**
- Username collision: if two students enter the same name, they share progress — acceptable for a classroom, worth a note in the UI
- Empty username: validate that the input is not blank before proceeding
- Very long usernames: cap at ~30 characters in the input
- The `flashcard_users` object could grow large if many users are added — not a concern at classroom scale

---

## Plan

1. Write a `migrateFromV2()` function:
   - Check for `flashcard_progress` key
   - If found, copy it into `flashcard_users["default"]` and set `flashcard_current_user` to `"default"`
   - Delete `flashcard_progress`
2. On app load (after migration), read `flashcard_current_user`:
   - If not set → show the username prompt (a simple overlay with a text input and confirm button)
   - If set → proceed to load the app for that user
3. Update `getProgress(id)` and `saveProgress(id, correct)` to read/write from `flashcard_users[currentUser]` instead of the flat `flashcard_progress` array
4. Display the current username in the header (e.g. "Studying as: alice")
5. Add a "Switch user" button that:
   - Removes `flashcard_current_user` from `localStorage`
   - Reloads the page (triggers the username prompt)
6. Username prompt validation: trim whitespace, reject empty input, cap at 30 characters
7. On username confirm: set `flashcard_current_user`, initialise `flashcard_users[username]` to `[]` if it doesn't exist, then load the app
