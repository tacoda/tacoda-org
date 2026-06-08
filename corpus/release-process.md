# Release process

A release is a single moment that is hard to undo. The process exists so the team can verify *that it works* before claiming it does, and so the next reader of the code can tell *what shipped* without running git archaeology.

> **Rules extracted:** [`guides/release-process.md`](../guides/release-process.md). This file holds the reasoning and anti-patterns.

This guide is **not strict**. Consumers (nested plugins or the project) may layer their own release conventions on top. See `harness/plugins/<nested-plugin>/guides/release-process.md` or `harness/guides/release-process.md` for refinements.

## What a release must do

1. **Document what changed.** A reader six months from now should be able to open `CHANGELOG.md` and answer "what did 0.X.Y add or break?" without reading commits.
2. **Be tagged.** Tags are how downstreams pin. Untagged commits are not releases.
3. **Be testable.** The smoke test for the release runs on the artifact that was actually built, not on `main` at some later time.

## Anti-patterns

- **Marketing release notes.** "We're excited to announce…" tells readers nothing. Lead with the *change*: what's new, what's removed, what's broken.
- **Squashing the changelog into the commit body.** Commits are for code archeology; the changelog is for downstreams. They have different audiences.
- **"Will land in the next release."** Aspirational changelog entries lie. Document what shipped, not what's coming.

## Why this lives in a shared plugin

Cross-project consistency. When tacoda projects are read together — by a new hire, an auditor, an LLM doing a portfolio sweep — the release format being the same across all of them lowers cognitive load by an order of magnitude. Consumers can refine *within* this shape; they shouldn't invent a new one.
