---
number: 1.3
source: 2017-01-26-dfa-in-java
title: String Acceptance by DFA
synonyms:
---

Let $$M = (Q, Σ, δ, q_0, F)$$ be a deterministic finite automaton, and let $$a ∈ Σ^*$$. The string $$a$$ is
**accepted** by $$M$$ if 
{: .bottomless }

$$δ^*(q_0, a) ∈ F$$


and is **rejected** by $$M$$ otherwise.
