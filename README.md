# Vinayak Ajith

I build agents that run against real company data, where being wrong is expensive.

Most of my work is closed-source, so instead of repos, here's what it taught me.

---

## Notes from a year of putting an agent in production

**The last message in the loop is not the answer.**
An agentic loop that runs out of steps will happily emit its own internal reasoning as a final response — plans, half-thoughts, "let me check one more thing." The fix isn't a better prompt. It's a terminal synthesis node that always runs and is *forced* via `tool_choice` to call a submit-findings tool. Now the output is a schema, not whatever the model was mid-sentence about. Structure beats instruction.

**Never let a model write its own citations.**
Every source id my email subagent cites gets re-verified against the store, and the Sources block is assembled from store data rather than model output. One hallucinated reference in fifty makes a user distrust all fifty — provenance is the one thing you cannot delegate to a probabilistic system.

**Delegate context, not just work.**
Raw email bodies never enter the main transcript. A subagent gets a self-contained objective, reads bodies on a cheap Haiku-class model in its own private tool registry, and returns a ≤500-word digest. The main agent still reasons at full capability, at roughly 3–5× lower cost. Context isolation turns out to be a cost lever *and* a debuggability lever.

**Put the determinism underneath the model, not around it.**
Freshness, conflict resolution, safety checks — all Python. LLMs are unreliable at "which of these two contradictory facts is newer," and prompting harder doesn't fix a capability gap. The model decides what to ask. Code decides what's true.

**AI features should degrade to the deterministic baseline.**
My dashboard builds a spec from schema heuristics first, then an optional LLM pass refines it — validated tile by tile. Any tile the model malforms gets dropped and the baseline kept. Designed this way, the AI pass can only improve the result, never break it. This is the shape I now reach for by default.

**Rewrites need a harness, not confidence.**
Replacing the hand-rolled agent loop with a LangGraph `StateGraph` shipped only after it matched the old engine event-for-event against pinned golden fixtures. "It looks like it still works" is not a migration strategy for nondeterministic systems.

**Write down the trade-offs you accepted.**
My docs have a "known limitations at scale" section — the unbounded `COUNT(*)` in hybrid search, the unbounded `fetchall()`, the per-process rate limiter. Naming them turns future surprise bugs into decisions someone already made on purpose.

**The moat is the decision log, not the model.**
Metrics only enter my semantic layer when a human explicitly promotes a verified answer. The agent never writes there itself. What accumulates is a human-confirmed record of what this business actually means by its own words — and that dataset is the part nobody can copy.

---

## What I'm learning

Karpathy's Zero to Hero, closed-book — the rule is I rebuild each piece from scratch with the video shut before moving on. Currently on micrograd's `Value` class.

I've spent two years calling `.backward()` without being able to derive it. Framework fluency isn't understanding, and the gap only shows up when something breaks below the API surface — which, in production, it always does.

Also working through PostgreSQL from first principles right now: B-tree internals, query planner behavior, why HNSW and GIN indexes behave the way they do under my own search workload. Learning the layer you already depend on beats learning the layer you might use someday.

---

## Where this is going

Toward physical AI. Text agents fail softly — a bad answer gets rewritten. Robots fail into the world, which makes every problem I care about (grounding, verification, failure modes that degrade instead of explode) sharper and less forgiving.

Immediate plan is unglamorous: finish the Karpathy rebuilds, buy a printer, build an SO-101 arm, and find out how much of what I know about agents survives contact with actuators.

---

## Working with

`Python` · `LangGraph` · `FastAPI` · `PostgreSQL + pgvector` · `Anthropic API` · `Docker` · `LangFuse`

Comfortable in PyTorch and Hugging Face. Have used TensorFlow, vLLM, and AWS — wouldn't claim expertise in any of them.

---

Chennai, India · [LinkedIn](https://linkedin.com/in/vinayak-ajith-208993266) · thevinayakajith@gmail.com

Happy to talk about agent architecture, retrieval that survives real corpora, or why your eval set is probably too easy.
