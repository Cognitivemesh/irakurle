# Memory

Irakurle is a Knowlege Base Harness. As part of this Harness, it has Memory stack: explicit layered memory

This is one of the clearest contributions of the repo. The README says it uses four memory layers:

> working/
> episodic/
> semantic/
> personal/

and that each layer has its own retention policy. It also mentions query-aware retrieval using salience × relevance, plus nightly compression into reviewable candidates.

That maps very well to your framework:

working memory = near-term active state
episodic memory = what happened
semantic memory = distilled reusable lessons
personal memory = preferences and user-specific style constraints

This is a strong answer to your earlier concern that memory should hold things like:

facts
rules
constraints
contracts
prior decisions

The repo does not use exactly that same wording for every category, but structurally it is trying to do exactly that through differentiated storage classes.
