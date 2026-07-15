# PR Response Doc — CineLog Watchlist Feature

## AI Usage

I used AI to help me understand the existing codebase, troubleshoot Git and rebase issues, and explain why some tests were failing. I wrote the code changes myself after reviewing the existing project patterns. I also used AI to double-check my commit messages followed the conventional commit format.

---

## Comment 1 – Rename

**What I did:**

I renamed `save_to_watchlist()` to `add_to_watchlist()` in `services/watchlist_service.py` and updated every place where it was called, including the watchlist route.

**How I verified:**

I searched the project for the old function name to make sure there were no remaining references, then ran the test suite.

---

## Comment 2 – Deduplication

**What I did:**

I added a duplicate check before creating a new watchlist entry. If the film is already in the user's watchlist, the service raises `AlreadyInWatchlistError` instead of creating another entry.

**How I verified:**

I compared the implementation to the existing collection service pattern and confirmed the behavior by running the tests.

---

## Comment 3 – Missing Test

**What I did:**

I created `tests/test_watchlist.py` and added a test to verify that adding a nonexistent film raises `FilmNotFoundError`.

**How I verified:**

I ran the new watchlist test by itself and then ran the complete test suite to confirm everything still passed.

---

## Comment 4 – Default Visibility

**My position:**

I decided to keep `public=True` as the default.

**Reasoning:**

I think most users adding a film to a watchlist expect it to behave like a normal watchlist without having to change extra settings every time. It keeps the common case simple while still allowing the value to be changed later if needed.

**Tradeoff acknowledged:**

This means some users might accidentally create a public watchlist entry if they don't realize the default. Using `public=False` would prioritize privacy, but it would also require more manual changes for users who normally want public watchlists.

---

## Comment 5 – Sort Order

**My position:**

I chose to sort by the date the film was added, with the newest entries first.

**Reasoning:**

When I add something to a watchlist, I usually want to see the movies I recently saved first. It makes it easier to continue where I left off instead of searching through older entries.

**Engagement with the reviewer's point:**

I understand that alphabetical order makes movies easier to find, especially in very large watchlists. However, for a personal watchlist, I think showing the newest additions first is more useful for day-to-day use.

---

## Comment 6 – Rebase

**What conflicted:**

During the rebase, `.gitignore` conflicted because both branches added one. I also had to restore the `WatchlistEntry` model after rebasing onto the updated `main` branch.

**How I resolved it:**

I merged the `.gitignore` changes and updated the watchlist model so it matched the UUID-based film IDs used in the updated project.

**How I verified no conflict remains:**

I ran the full test suite after the rebase and confirmed all tests passed successfully.