# Script
## Overview
Use a Python script to handle all interactions.

Execute from the repository root directory:
```bash
uv run python scripts/main.py [COMMAND]
```

## Status
```bash
uv run python scripts/main.py status
```

Returns:
- Total number of cards
- Today's review progress (reviewed / total tasks for today)
- Number of cards due each day for the upcoming week
- Average retrievability

### Generate Cards
```bash
uv run python scripts/main.py card new -q "question" -a "answer"
```
Creates a new card and returns the `CARD_ID`.

```bash
uv run python scripts/main.py card override [CARD_ID] -q "question" -a "answer"
```
Overrides the question and answer, returns the `CARD_ID` (keeping it unchanged), and resets the scheduling.

```bash
uv run python scripts/main.py card rm [CARD_ID]
```
Soft deletes the card while preserving the historical review log.

### Review
```bash
uv run python scripts/main.py review get-question
```
Returns the `CARD_ID` and the question (QUESTION).

```bash
uv run python scripts/main.py review get-answer
```
Returns the `CARD_ID` and the standard answer (ANSWER).

```bash
uv run python scripts/main.py review rate [RATING]
```
`RATING` options include: `again`, `hard`, `good`, `easy`.

Returns:
- Card ID
- Result of the current rating
- Historical accuracy
- Progress of remaining due cards
- Change in retrievability

## Notes
- You must follow the sequence `get-question -> get-answer -> rate`; otherwise, a warning will be issued, or the previous question will be repeated.
