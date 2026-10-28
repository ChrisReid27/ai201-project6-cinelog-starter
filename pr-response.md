# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** I renamed the watchlist service helper to add_to_watchlist() in services/watchlist_service.py and routes/watchlist/watchlist.py. There were three references of saved_to_watchlist() that got changed.
**How I verified:** I verified these changes by using the Find All References tool in VS Code, in the references tab I could see that all renaming was done everywhere in the project.

## Comment 2 — Deduplication
**What I did:** I reviewed the logic in the main branch for add_to_collection() within services/collection_service.py and compared it to what was the current set up in add_to_watchlist(). I added AlreadyInWatchlistError to services/watchlist_service.py which gets raised if after add_to_watchlist() checks for if an entryy already exists within it, the entry does. I also updated routes/watchlist/watchlist.py to return HTTP 409 for duplicates.
**How I verified:** This fix once done was compared against the procedure in add_to_collection within service/collection_service.py and it matched up. The test suite was ran and all four passed, including the deduplication test. Syntax validity was confirmed with no errors after using compile all for the services and routes packages.

## Comment 3 — Missing test
**What I did:**
**How I verified:**

## Comment 4 — Default visibility
**My position:**
**Reasoning:**
**Tradeoff acknowledged:**

## Comment 5 — Sort order
**My position:**
**Reasoning:**
**Engagement with reviewer's point:**

## Comment 6 — Rebase
**What conflicted:**
**How I resolved it:**
**How I verified no conflict remains:**

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->