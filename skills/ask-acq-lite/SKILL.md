---
name: ask-acq-lite
description: Answer a question with the combined perspective of the Acquisition.com orbit (Alex Hormozi, Leila Hormozi, Sharran Srivatsaa), grounded in their three hosted lite 2026 knowledge bases (paywalled sources excluded) with per-person attribution and citations. Use for "ask acq", "what would the Acquisition.com experts say", "what do Alex, Leila, and Sharran think about X", or any question wanting all three views side by side.
---

# Ask ACQ Lite (three-person lite 2026 corpus)

Thin dispatcher. All intelligence lives in the `ask-acq-lite` subagent
(`agents/ask-acq-lite.md` from this repo, installed to your agents
directory): the attribution rule (every claim named to its person, never a
blended voice), the three verified shared principles, the epistemic rules
(answer only from retrieved hits plus those principles, never from training
data), and the hosted retrieval procedure (namespaces `alex-hormozi-lite`,
`leila-hormozi-lite`, `sharran-srivatsaa-lite`, each passed explicitly,
called sequentially) are its system prompt, injected on every spawn. The
parent NEVER reads the principles files or adds context.

## Procedure

1. Spawn the `ask-acq-lite` subagent with the user's question as the ENTIRE
   prompt. No added instructions, no pasted files, no framing.
2. Return the subagent's answer with citations intact.

## Verification

A good answer attributes every claim to a named person (or to one of the
three verified shared principles), cites each claim from that person's own
corpus, and flags any topic a corpus does not cover instead of papering over
the silence.
