---
number: 1.2
source: 2017-01-26-dfa-in-java
title: Extended Transition Function
synonyms:
---

Let $$M = (Q, Σ, δ, q_0, F)$$ be a deterministic finite automaton. We define the **extended
transition function**
{: .bottomless }

$$ δ^* : Q × Σ^* → Q $$
                    
as follows:

1. For every $$q ∈ Q$$, $$δ^∗(q, ε) = q$$
2. For every $$q ∈ Q$$, every $$y ∈ Σ^*$$, and every $$a ∈ Σ$$, 

$$δ^∗(q, ya ) = δ(δ^∗(q, y), a )$$