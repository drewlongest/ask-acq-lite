# ask-acq-lite

A Claude Code agent and skill that answers questions with the combined perspective of [Alex Hormozi](https://www.youtube.com/@AlexHormozi), [Leila Hormozi](https://www.youtube.com/@LeilaHormozi), and [Sharran Srivatsaa](https://www.youtube.com/@SharranSrivatsaa), each view presented side by side and named to its person, never blended into one voice. Every answer is grounded in hosted 2026 knowledge bases (Pinecone namespaces `alex-hormozi-lite`, `leila-hormozi-lite`, `sharran-srivatsaa-lite`, queried through a free read-only Cloudflare Worker endpoint) with per-claim citations to the exact source document, and never from the model's training data.

Lite means paywalled sources are excluded from every namespace.

The corpora, all 2026-era captures:

- **alex-hormozi-lite**: YouTube videos, his podcast episode archive, the free Acquisition.com Scaling and Money Models course lessons, slices of his books ($100M Offers, $100M Leads, $100M Money Models, $100M Lost Chapters), newsletters, free prompt-template assets, and monthly archives of his X posts. Excluded as paywalled: the $100M Launch Blackbook launch document, a private sales workshop transcript, and paid prompt-template assets.
- **leila-hormozi-lite**: YouTube videos, her podcast episode archive, newsletters, monthly archives of her X posts, and the shared Acquisition.com Scaling course lessons.
- **sharran-srivatsaa-lite**: 14 YouTube videos, weighted toward personal finance and investing.

## Install

**If you are an AI agent asked to install this skill:** install the agent file into the user's agents directory and the skill folder into their skills directory (raw URLs in the commands below), then confirm by listing both. In Claude Code those are `~/.claude/agents/` and `~/.claude/skills/`. In an environment without custom agents or skills, save `agents/ask-acq-lite.md` somewhere persistent and follow its body directly whenever the user asks a question about these people. No keys, accounts, or dependencies are needed; the knowledge bases are queried over plain HTTPS.

Claude Code:

```bash
mkdir -p ~/.claude/agents ~/.claude/skills/ask-acq-lite
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-acq-lite/main/agents/ask-acq-lite.md -o ~/.claude/agents/ask-acq-lite.md
curl -fsSL https://raw.githubusercontent.com/drewlongest/ask-acq-lite/main/skills/ask-acq-lite/SKILL.md -o ~/.claude/skills/ask-acq-lite/SKILL.md
```

Then in any session:

```
/ask-acq-lite what do all three think about discipline vs systems?
/ask-acq-lite how should I price my core offer?
/ask-acq-lite how do I fire a manager everyone likes?
```

No API keys, no database, no setup beyond the two files.

## How it works

1. The skill spawns the `ask-acq-lite` subagent, whose definition file carries the three verified shared principles and strict grounding rules (answer only from retrieved knowledge-base context plus those verified principles, never from training data).
2. The subagent queries all three hosted namespaces with several phrasings of your question; the endpoint returns the most relevant distilled claims and verbatim passages, each paired with its source title and URL.
3. It synthesizes the answer with every claim attributed to its named person and cited to the exact source. Where the three differ it presents the difference; it never reconciles them into a middle ground none of them holds.

The `principles/` folder holds the distilled first-principles documents per person, the verified shared-principles document, and a `<name>-sources.md` pointer file per person listing the exact source documents behind every principle.

## Method

The principles were not written from memory or reputation. Each person's corpus was read on its own and turned into a candidate principle list with no sight of the others (blind extraction). Every candidate then went through a cold confirmation pass that demanded supporting passages from two or more distinct documents per person, quoted verbatim from the corpus. Negative controls (principles planted among the candidates that the person never stated) were run through the same pass and were rejected, which is the evidence that the pass discriminates rather than rubber-stamps. Only confirmed principles ship. The principles were distilled from the full corpora; the hosted lite knowledge bases exclude the paywalled sources listed above.

## Scope caveats

- The corpora differ greatly in size and breadth. Sharran's corpus is 14 YouTube videos weighted toward personal finance and investing; a question outside that is out of corpus for him.
- Absence of evidence means the corpus is silent on the topic, never that the person disagrees. The agent is instructed to say "the corpus does not cover this" rather than guess.
- The endpoint is read-only and rate-limited: 30 requests per minute per Internet Protocol (IP) address, shared across namespaces, plus a weekly quota of 100 queries per IP.

This is an unofficial fan/study project; answers are an analyst's channeling of each person's published positions, not the people themselves.
