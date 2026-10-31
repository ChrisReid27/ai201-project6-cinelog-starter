# PR Response Doc — CineLog Watchlist Feature

## AI Usage
<!-- Fill in at the end — how you used AI tools during this project -->

## Comment 1 — Rename
**What I did:** I renamed the watchlist service helper to add_to_watchlist() in services/watchlist_service.py and routes/watchlist/watchlist.py. There were three references of saved_to_watchlist() that got changed.
**How I verified:** I verified these changes by using the Find All References tool in VS Code, in the references tab I could see that all renaming was done everywhere in the project.

## Comment 2 — Deduplication
**What I did:** I reviewed the logic for add_to_collection() within services/collection_service.py and compared it to what was the current set up in add_to_watchlist(). I added AlreadyInWatchlistError to services/watchlist_service.py and I also updated routes/watchlist/watchlist.py to return HTTP 409 for duplicates. Now, the add_to_collection() pattern reference correctly checks whether an entry already exists for the same user_id and film_id and when a duplicate is found, it raises AlreadyInCollectionError instead of creating or committing another collection entry.
**How I verified:** This fix once done was compared against the procedure in add_to_collection within service/collection_service.py and it matched up. Syntax validity was confirmed with no errors after using compile all for the services and routes packages.

## Comment 3 — Missing test
**What I did:** I examined test_collection.py's test_add_to_collection_nonexistent_film_raises test and made a new file called test_collection.py. In it I made an equivalent test for the watchlist service. Similar to the collection version, test_add_to_watchlist_nonexistent_film_raises tests if the system correctly raises the FilmNoTFound error when a user tries to add a film id that doesn't exist to their watchlist before the watchlist entry is made. The fake_film_id is "00000000-0000-0000-0000-000000000000" and when entered as an id for a watchlist addition, will trigger the FilmNotFound error.
**How I verified:** I ran this test with pytest tests/test_watchlist.py -v in which it passed. All tests (ran with pytest tests/ -v) still pass with this recent addition.

## Comment 4 — Default visibility
**My position:** I want to keep `public=True` as the default for new watchlist entries.
**Reasoning:** This would be optimal for users who want to quickly share and discover film recommendations without needing the extra visibility step for every item. The default makes watchlists useful as social, discoverable lists while still allowing the application to expose visibility controls where users would need them.
**Tradeoff acknowledged:** The tradeoff is having the default be private, which would mean stronger privacy for users and that users explicitly opt in to sharing. I say no to this because it would make new watchlists less useful for discovery until users change the setting. Keeping the setting default as public is intentional, it will provide users with easier sharing and increase their engagement with the system. Changing the visibility to private is still an option the user can choose.

## Comment 5 — Sort order
**My position:** I think that watchlists should default to date added rather than alphabetical.
**Reasoning:** Date added, with the most recently added at the top, is more immediately understandable to users. It's the most relevant item they interacted with whenever they used the system last and their watchlist entry should reflect that.
**Engagement with reviewer's point:** I agree with this point and support the suggestion. If a film they add starts with J, they might be scrolling through their potentially long watchlist to find it when instead, it could be at the top of the list. Users will also be given the option to switch to alphabetical view if they so choose, as well as date added in the opposite direction, as in earliest first.

## Comment 6 — Rebase
**What conflicted:** I fetched the updated main branch and rebased feature/watchlist onto origin/main. The rebase conflicted in models.py (and also in .gitignore and pr-response.md but those conflicts were very minor) because the WatchlistEntry.film_id column was defined as a UUID string on main but as an integer in the watchlist changes.
**How I resolved it:** I kept the UUID definition to match the post-refactor Film.id type, removed the conflict markers, and updated the watchlist service and route documentation so they refer to film UUIDs rather than integer IDs.
**How I verified no conflict remains:** I searched the resolved files for merge-conflict markers, ran Python syntax checks on the affected modules, and then checked the branch history with git log --merges. The working tree is clean and no merge commits remain in the feature/watchlist history.

## PR Description
<!-- Written at the end — feature overview, design decisions, manual testing steps -->