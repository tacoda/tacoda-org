# Documentation — rules

The rules from [`corpus/documentation.md`](../corpus/documentation.md).
Loaded ambient; enforced by the [drift sensor](../../../../sensors/drift.md). The corpus file holds the full reasoning and anti-patterns.

## GOLDEN RULES

- **Apply the Hemingway test to every sentence.** Each sentence must change what the reader does. If a sentence can be deleted without losing instruction or context, delete it. If it can be shorter without losing meaning, shorten it.
- **Default to no comment.** Add one only when the *why* is non-obvious: a hidden constraint, a subtle invariant, a workaround for a specific bug. If removing the comment wouldn't confuse a future reader, don't write it.
- **Lead with the why.** Documentation explains motivation and non-obvious behavior. Well-named identifiers explain *what*; comments and docs explain *why*.

## RULES

- **No marketing adjectives** in docs or comments: "powerful," "robust," "seamless," "best-in-class," "comprehensive," etc. They tell the reader nothing verifiable.
- **No hedged advice.** Replace "you may want to consider possibly..." with a direct statement, or delete the sentence.
- **No restating the code.** A comment that paraphrases the function name is noise; rename the function instead.
- **Aspirational docs are forbidden.** Document how the code *does* work, not how it *should* work. Out-of-date docs that lie are worse than no docs — fix the doc or delete it.
- **External references include the specific link.** RFC number, paper citation, ticket ID — enough that the next reader can verify the claim.
- **No transition filler** ("Now that we've covered X..."). Headings carry the structure.

---

Traces to: [`corpus/documentation.md`](../corpus/documentation.md).
