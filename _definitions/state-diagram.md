---
number: 1.1
source: 2017-01-26-dfa-in-java
title: State Diagram
synonyms:
- Transition Diagram
---

A **state diagram** for DFA $$M = (Q, Σ, δ, q_0, F)$$ is graph defined as follows:

* For each state $$q$$ in $$Q$$ there is a node.
* For each state $$q$$ in $$Q$$ and each input symbol $$a$$ in Σ, let $$δ(q,a) = p$$. Then the state diagram has an arc
(arrow) from node $$q$$ to node $$p$$, labeled $$a$$. If there are several input symbols that cause transitions from
$$q$$ to $$p$$, then the state diagram can have one arc, labeled by the list of these symbols.
* There is an arrow into the start state $$q_0$$ which does not originate at any node.
* Nodes corresponding to accepting states (those in $$F$$) are marked by double circle. States not in $$F$$ have a
single circle.
