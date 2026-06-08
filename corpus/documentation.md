# Documentation

The point of documentation is to transfer the *why* and the *non-obvious* into another human's head as fast as possible. Everything else — repetition of what the code already says, marketing adjectives, hedged "this should generally" prose — is friction. Friction in docs is worse than friction in code, because docs are the layer everyone reads first and last.

> **Rules extracted:** [`guides/documentation.md`](../guides/documentation.md). This file holds the reasoning and anti-patterns.

## The Hemingway test

Read every sentence and ask: *did this sentence change what the reader will do?* If not, delete it. If yes, can it be shorter without losing the change? If yes, shorten it.

The test is named after Hemingway because his prose is famously terse, but the discipline is older: Strunk and White (*Elements of Style*) said it as "omit needless words"; Orwell (*Politics and the English Language*) said it as "if it is possible to cut a word out, always cut it out." The exact formulation doesn't matter. What matters is the **act of cutting**.

## What gets cut

- **Marketing adjectives.** "Powerful." "Robust." "Seamless." "Best-in-class." These say nothing the reader can verify. Cut.
- **Hedged advice.** "You may want to consider possibly..." → just say what to do or don't say it.
- **Restated facts.** "This function `getUser()` gets a user." Cut.
- **Setup that doesn't pay off.** Long preamble before the actual point. Move the point to the top; cut everything that doesn't support it.
- **Filler transitions.** "Now that we've covered X, let's move on to Y." Headings already do this work.

## What stays

- **Why this exists.** The motivation, the constraint that forced the design.
- **Non-obvious behavior.** "This caches for 5 minutes because the upstream API rate-limits at 200/hour."
- **The thing that will trip up the next reader.** Edge cases, gotchas, surprising defaults.
- **External references the reader will need.** RFC numbers, papers, ticket IDs.

## Anti-patterns

- **Comments that paraphrase the function name.** `// Get the user by ID` above `func getUserByID()`. If a human-named function needs a comment to say what it does, the name is wrong.
- **Docstrings as resume padding.** Multi-paragraph descriptions of trivial functions. Optimize for *not writing* the docstring unless something non-obvious deserves saying.
- **"Why" without specifics.** "Done this way for historical reasons" tells the next reader nothing — they want to know *which* historical reason, so they can decide whether it still applies.
- **Aspirational docs.** Describing how the code *should* work rather than how it *does* work. Docs that lie are worse than no docs.

## Why this matters more than it seems

Bad docs are a force multiplier in the wrong direction. They cost time at write-time (someone wrote them), cost time at read-time (someone has to filter the noise), and they erode trust in *all* documentation in the codebase (readers learn to skip everything). The path of least resistance is to write less, more carefully.

The compounding cost is the real argument: a one-sentence comment that lives for ten years and saves five minutes per reader pays back hundreds of times over. A ten-sentence comment that wastes thirty seconds per reader costs hundreds of times over. The leverage points are sharp; aim carefully.
