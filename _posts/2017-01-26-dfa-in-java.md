---
layout: post
title: DFA in JAVA
category: Tutorial
comments: true
tags:
    - java
    - dfa
quote:
    quote: There is no mistakes or failures, only lessons.
    author: Denis Waitley
description: DFA (Deterministic Finite Automata) implementation using JAVA.
---

**DFA (Deterministic Finite Automata)** sometimes called **DFSM (Deterministic Finite State Machine)** is a a finite state 
machine which given sequence of symbols accepts them or not. 


## Why DFA?

<a name="task 1"></a>Lets, say you are given a task to write a program which accepts strings with following conditions: 

1. It must contain only `:`, `)`, `(`, and `_` characters.
2. `:` has to be followed by `)` or `(`.
3. Has to have least one smiley `:)` or sad `:(` face.
4. If string has more than one face, then all of them have to be separated by at least one `_` character.
5. Each string can start or end with zero or more `_` characters.

Of course, this task can be easily accomplished using regular expressions. But for the sake of argument, let's say we 
can't use them. In fact, behind the scenes, all regular expressions are transformed into automata anyway. 

One way to accomplish this task is to create method with a `for` loop and deeply nested `if` statements in it. Loop
will go through each character and checks if it is acceptable, based on previous characters. The problem is that, there
are lots of cases to consider, thus making this approach error prone. 

Another way of thinking about it, is to imagine that a program (DFA) has finite set of states. For example, a state when it 
begins reading characters, a state after last character, and intermediate states which it visits between 
start and finish. A transition between states are based on two things: 1. Current state. 2. Read in character. There
can be situations when different characters leads to the same state. For example, after reading `:)` there are only one 
acceptable character `_`, all other characters `:`, `)`, `(` will lead to not acceptable state. Also, some 
characters will lead to the same current state. For example, if the first character was `_`, then no matter how many of `_` 
program will read in, all the states `_`, `__`, `_______` will not impact transition to the next state, 
thus current state will stay the same. If the DFA after reading last character will end up in state which 
is acceptable, then a string can be considered accepted.


## Formal definition

While we could create DFA using the reasoning presented earlier. There are some questions which begs an answer: 
"what are the start and accept states?", "can it have zero accept states?", 
"can it have more than one start state? and so on. All these questions can be answered by looking at the definition of 
DFA.

<div class="env-header">Definition 1.0 <a name="definition 1.0"></a> </div>
{::options parse_block_html="true" /}
<div class="definition alert">
A **deterministic finite automata** $$M$$ is a 5-tuple $$(Q, Σ, δ, q_0, F)$$, where

1. $$Q$$ is a finite set called the **states**,
2. $$Σ$$ is a finite set called the **alphabet**,
3. $$δ : Q × Σ → Q$$ is the **transition function**,
4. $$q_0 ∈ Q$$ is the **start state**, and
5. $$F ⊆ Q$$ is the set of **accept states**. 

</div>

{::options parse_block_html="false" /}

The first thing what we should notice is that DFA is defined as 5-tuple. Meaning that it has five elements and order
of the elements matter, thus $$(Q, Σ, δ, q_0, F) \neq (Σ, Q, δ, q_0, F)$$. First element $$Q$$ represents all the states 
which DFA has. In general, when we are talking about finite state machine, term **finite state** is analogous to $$Q$$.

Second element is an alphabet $$Σ$$ (read as "sigma"). It represents all the symbols which are allowed to go into machine. 
For example, if $$Σ = \{0, 1, 2\}$$, then only characters $$1$$, $$2$$, and $$3$$ are allowed. But what if we have more 
symbols? In that case we have to modify DFA to support them. Remember that we are dealing with **deterministic** finite
automata, meaning that it's next state is determined by input symbol, thus unrecognised symbols are not allowed. 
Also, in alphabet symbols do not need to be one letter characters. For example, $$Σ = \{health, stamina, shield\}$$ 
is perfectly valid alphabet for game AI.

Third element is a function $$δ$$ (read as "delta") where $$δ : Q × Σ → Q$$. It represents a transition from one state
to another. $$Q × Σ$$ is a domain of the function. From the set theory, we know that if we have two sets $$Q$$ and $$Σ$$, 
then their **cartesian product** $$Q × Σ$$ is a set of all the pairs $$(a, b)$$ where $$a ∈ Q$$ and $$b ∈ Σ$$. Codomain
of $$δ$$ is $$Q$$. In plain english function $$δ : Q × Σ → Q$$ says: _"give me any sate and any symbol and I will return
a state"_. Notice, it does not say that a _"returned"_ state has to be different, it can be the same state as _"given"_.

Fourth element $$q_0$$ represents start state which has to be one of the states in $$Q$$. Also, by definition of the 
start state, we can answer a question "can it have more than one state?". No, it can't. Every DFA has to have only one 
start state. 

Lastly, fifth element $$F$$ represents set of accept states which is subset of $$Q$$. Notice the difference between $$∈$$
and $$⊆$$. $$∈$$ means "member of", thus start state $$q_0$$ is a member of $$Q$$. While $$⊆$$ means "subset of", thus
$$F$$ is a subset of $$Q$$. We know that every set has empty set $$∅$$ as it's subset. Therefore, $$F$$ can be $$∅$$, meaning
that DFA can have zero accept states.


## State Diagram

While formal definition is a nice way of defining DFA, it does not help us to visualise it.
On the other hand, state diagrams can be used to define and to visually representing finite automatas. 
The only downside of them, is that state diagrams tend to be very messy if DFA's have many states. 
Nevertheless, for simple ones is a perfect choice.


<div class="env-header">Definition 1.1<a name="definition 1.1"></a> </div>
{::options parse_block_html="true" /}
<div class="definition alert">
A **state diagram** for DFA $$M = (Q, Σ, δ, q_0, F)$$ is graph defined as follows:

* For each state $$q$$ in $$Q$$ there is a node.
* For each state $$q$$ in $$Q$$ and each input symbol $$a$$ in Σ, let $$δ(q,a) = p$$. Then the state diagram has an arc
(arrow) from node $$q$$ to node $$p$$, labeled $$a$$. If there are several input symbols that cause transitions from 
$$q$$ to $$p$$, then the state diagram can have one arc, labeled by the list of these symbols.
* There is an arrow into the start state $$q_0$$ which does not originate at any node.
* Nodes corresponding to accepting states (those in $$F$$) are marked by double circle. States not in $$F$$ have a 
single circle.

</div>

{::options parse_block_html="false" /}

For example, lets draw a state diagram for [Task 1](#task 1).

{::options parse_block_html="true" /}

<figure>
<a name="figure 1"></a>
![Automata](/resources/images/automata.svg)
<figcaption>
**Figure 1:** Shows state diagram of DFA which recognises language of smiley and sad faces.
</figcaption>
</figure>

{::options parse_block_html="false" /}

We can see that nodes represents states, while arcs - transitions from one state to another. Transitions are based on 
current state and input symbol. For example, if automata's current state is $$q_0$$ and the input symbol is `:`, 
then it goes to $$q_1$$. If the input symbol is `_`, then it transitions to its current state ($$q_0$$). Notice that one 
arrow pointing to $$q_0$$ does not originate from any node, thus by of [Definition 1.1](#definition 1.1) $$q_0$$ is a start state.
And nodes with double circle $$q_2$$ and $$q_3$$ represent accept states. One special type of non accepting states are those which has
only arrows pointing to it but has no arrows pointing to other states. Thus, if any transition
leads to that kind of state, then it is dead end because it is impossible to reach an accepting state, 
these states are called **dead states**. For example, $$q_4$$.

{::options parse_block_html="true" /}

<div class="success alert"> 
**NOTE:** While it is not necessary, I color coded states in the state diagram. Thus, orange stands for start, green - accept and
red for dead states.
</div>

{::options parse_block_html="false" /}


## Extended transition function

At this point we only talked about transitions as a separate functions. That is, given current state and an input symbol,
DFA transitions to next state or current. Then, that state becomes an input for the next transition function, 
and so on. This cycle repeats until there are no more input symbols. Thus, producing a sequence of state transitions. This
sequence can be expressed by **extended transition function** denoted by $$δ^*$$. This function is very similar to $$δ$$, 
they both take state as an input and return state as an output. But contrary to $$δ$$, function $$δ^*$$ takes not a symbol 
from the alphabet $$Σ$$ but a string consisting of symbols from the alphabet $$Σ$$. So, if the input state is a start state $$q_0$$, then
given a string, $$δ^*$$ will return a state, which we would get after following sequence of state transitions. Maybe
formal definition will be more understandable.

<div class="env-header">Definition 1.2<a name="definition 1.2"></a> </div>

{::options parse_block_html="true" /}

<div class="definition alert">
Let $$M = (Q, Σ, δ, q_0, F)$$ be a deterministic finite automata. We define the **extended
transition function**
{: style="padding-bottom: 0"}

$$ δ^* : Q × Σ^* → Q $$
                    
as follows:

1. For every $$q ∈ Q$$, $$δ^∗(q, ε) = q$$
2. For every $$q ∈ Q$$, every $$y ∈ Σ^*$$, and every $$a ∈ Σ$$, 

$$δ^∗(q, ya ) = δ(δ^∗(q, y), a )$$

</div>

{::options parse_block_html="false" /}

Or maybe not :). The problem is that, $$δ^*$$ is defined recursively. We have a base case (**1.**) and then we define it
recursively (**2.**). Anyway, if you look more closely, then you would see a main difference between 
$$ δ^* : Q × Σ^* → Q $$ and $$ δ : Q × Σ → Q $$. Contrary to $$δ$$'s domain $$Q × Σ$$, extended transition 
function's domain $$Q × Σ^*$$ is a cartesian product of set of states $$Q$$ and 
[Kleene Star](https://en.wikipedia.org/wiki/Kleene_star) $$*$$ over alphabet $$Σ$$ denoted by $$Σ^*$$. Lets call it a
**Kleene closure**, which is a set of all strings over alphabet $$Σ$$, including the empty string $$ε$$. For example, 

If $$Σ = \{aa, b\}$$, then $$Σ^* = \{ε, aa, b, aab, baa, aaaa, bb, aaaab, \dots \}$$

You may ask "why do wee need it"? Remember when I said, that automata accepts strings where each symbol is in alphabet $$Σ$$. 
So Kleene closure is a nice way of representing all those strings. 

After defining $$δ^∗$$ signature, we need to define how it functions. We start with the base case, which says that 
for every $$q ∈ Q$$, $$δ^∗(q, ε) = q$$. In plain english, function $$δ^∗$$ says _"give me any state and empty string and
I will give you back the same state"_. It may sound useless, but in fact it is very important case because, as we 
will see, it terminates a function. Empty string $$ε$$ indicates the end of the string, so it makes sense to return
input state. Just like $$δ$$ transition would return the state after last input symbol. 

Recursive case defines function for those cases when a string is not empty. $$ya$$ in $$δ^∗(q, ya)$$ means, **concatenation** 
between a string $$y$$ from $$Σ^*$$ and one symbol $$a$$ from the alphabet $$Σ$$. This is just another representation of
original string. Advantage of $$ya$$ is that we can use different parts of it to define $$δ^∗(q, ya)$$. 
{: style="padding-bottom: 0"}

$$δ^∗(q, ya ) = δ(δ^∗(q, y), a )$$

As you can see it is equal to the original transition function $$δ$$ from the definition of $$M$$. First argument of 
it is extended transition function $$δ^∗$$ with two arguments: a state and a string which is one symbol shorter than, 
original $$ya$$. And the last argument of $$δ$$ is symbol $$a$$ from original $$ya$$ string. For example, given
following state diagram, 

{::options parse_block_html="true" /}

<figure>
<a name="figure 1"></a>
![Automata](/resources/images/automata_2.svg)
</figure>

{::options parse_block_html="false" /}

and the string $$abc$$, we can trace $$δ^∗(q_0, abc)$$ function,  
{: style="padding-bottom: 0"}

$$
\begin{align} δ^∗(q_0, abc) &= δ(δ^∗(q_0, ab), c) \\
&= δ(δ(δ^∗(q_0, a), b), c) \\
&= δ(δ(δ(δ^∗(q_0, ε), a), b), c) \\
&= δ(δ(δ(q_0, a), b), c) \\
&= δ(δ(q_1, b), c) \\
&= δ(q_2, c) \\
&= q_3
\end{align}
$$

which is correct.

## Regular languages

The main job of automata is to recognise a language by accepting or rejecting sequence of symbols, lets call them **characters**, 
and a sequence - **string**. But what does it mean, when we say "accepting"?

<div class="env-header">Definition 1.0<a name="definition 1.0"></a> </div>

{::options parse_block_html="true" /}

<div class="definition alert">
Let $$M = (Q, Σ, δ, q_0, F)$$ be a finite automata and let $$w = w_1w_2\dots w_n$$ be
a string where each $$w_i$$ is a member of the alphabet $$Σ$$. Then $$M$$ **accepts** $$w$$ if a
sequence of states $$r_0, r_1,\dots , r_n$$ in $$Q$$ exists with three conditions:

1. $$r_0 = q_0$$,
2. $$δ(r_i, w_{i+1}) = r_{i+1}$$, for $$i = 0,\dots , n − 1$$, and
3. $$r_n ∈ F$$.

</div>

{::options parse_block_html="false" /}


{::options parse_block_html="true" /}

<div class="success alert">
**NOTE:** For the meaning of $$M = (Q, Σ, δ, q_0, F)$$ please see [Definition 1.3](#definition 1.3). 
</div>

{::options parse_block_html="false" /}


Condition 1 says that automata starts at start state. Condition 2 says that it goes from one state to another according
transition function. Condition 3 says that machine accepts its input if it ends up in accept state.
We say that $$M$$ **recognizes** language $$A$$ if $$M$$ accepts all the strings in the language. In addition, 

<div class="env-header">Definition 1.1 <a name="definition 1.1"></a> </div>
{::options parse_block_html="true" /}
<div class="definition alert">
A language is called a **regular language**, if some finite automata recognises it. 
</div>
{::options parse_block_html="false" /}


For example, [Figure 1](#figure 1) shows 
an automata which recognises a language consisting of the strings having unlimited smiley `:)` and sad `:(` faces. Where each face 
starts and ends by zero or more `_` characters, and each face is separated by at least one `_` character. 
Below we can see some strings which are in the language, and some which are not, 

* **Is**: `:)`, `:(`, `:(_:)`, `:)___:)__:(__`, `_________:)__`.
* **Is not**: `____`, `::)`, `:((`, `:(:()`, `:)_):)`, `(:`.

So if a string `_:)__:(` is in the language then our DFA should accept it. Let's see how it does that. Initially, DFA 
is in the $$q_0$$ state, then it receives 
`_` character, the arrow with it points to the same state $$q_0$$. Next character is `:`, which points to $$q_1$$ state, 
then following `)` DFA goes to $$q_2$$ state. From there, we have `_` to $$q_3$$, then one more `_` from $$q_3$$ to itself.
After that, `:` character points to $$q_1$$ and following `(` DFA ends up in $$q_2$$ state. 
As we can see, the last state is accept state, thus automata accepts string `_:)__:(`.

In similar manner we can track any string consisting of these four characters (symbols) `_`, `:`, `(`, `)` and check
if it belongs to our language. 




Anyway. Given DFA in [Figure 1](#figure 1) its formal definition would be:

$$M = (Q, Σ, δ, q_0, F )$$, where

1. $$Q = \{q_0, q_1, q_2, q_3, q_4\}$$, 
2. $$Σ = \{$$`_`$$,$$`:`$$,$$`)`$$,$$`(`$$\}$$, 
3. $$q_0 = q_0$$, 
4. $$F = \{q_2, q_3\}$$.
5. $$δ$$ is described as a **transition table**

{::options parse_block_html="true" /}
<figure class="table">

|         |$$:$$    |$$)$$    |$$($$    |$$\_$$   |
|:-------:|:-------:|:-------:|:-------:|:-------:|
|$$q_0$$  |$$q_1$$  |$$q_4$$  | $$q_4$$ | $$q_0$$ |
|$$q_1$$  |$$q_4$$  |$$q_2$$  | $$q_2$$ | $$q_4$$ |
|$$q_2$$  |$$q_4$$  |$$q_4$$  | $$q_4$$ | $$q_3$$ |
|$$q_3$$  |$$q_1$$  |$$q_4$$  | $$q_4$$ | $$q_3$$ |
|$$q_4$$  |$$q_4$$  |$$q_4$$  | $$q_4$$ | $$q_4$$ |

<figcaption>
**Table 1:** Transition table.
</figcaption>
</figure>

{::options parse_block_html="false" /}


If we compare DFA's state diagram [Figure 1](#figure 1) and this definition, we see that transitions are more understandable. For example, 
if I want to know next state from current $$q_2$$ given symbol `)`, I just need to find
$$q_2$$ state in left column, then `)` in the top row, and check which state is at their intersection ($$q_4$$).

## DFA implementation in JAVA

Given all the information about DFA, it should not be hard to implement it. From the formal definition we see, that our automata 
has 4 states: $$q_0$$, $$q_1$$, $$q_2$$, $$q_3$$, $$q_4$$, which are known in advance. Thus, we can create `private enum States {...}` 
to represent them. Nice thing about JAVA enum, is that you can define methods and instance variables in them. For example, 
to differentiate accept and non accept states, we can introduce instance variable `final boolean accept;` for each state. 

<div class="env-header">DFA states</div>
{% highlight java linenos%}
private enum States {

  Q0(false), Q1(false), Q2(true), Q3(true), Q4(false);

  final boolean accept;

  // Constructor for sate. 
  // Every state is either accepting or not.
  States(boolean accept) {
    this.accept = accept;
  }
  
  //...
}
{% endhighlight %}

Just like that, we defined automata's states $$Q$$ and a subset of accepting states $$F$$. Now, let's think how to implement 
transition table. Probably your first thought is "We can use two dimensional array or a map or a hashtable". All these 
solutions are good, and they would work. Unfortunately they are not very object oriented. Think of a state as being 
an object, to which you can send some messages for enquiry or making them to do something for you. For example, we can 
ask it, if it is an accept state or not? By introducing a method `boolean isAccept() {...}` which would return it's
instance variable `accept`. But in our case, it is not necessary, because `private enum States {...}` will be a member of
`public class DFA {...}`, thus all its variables will be accessible.


Similarly, given a character from the alphabet, we can ask a state to transit to another. So, instead of writing 
transition table and then passing it to some kind of function, we will define method `States transition(char ch) {...}` inside 
`private enum States {...}` to get next state. For that we will have to introduce additional instance variables in the enum.
Each variable will contain a reference to the state. I think, to show is much easier than to explain.


<div class="env-header">DFA transitions</div>
{% highlight java linenos%}
private enum States {

  Q0(false), Q1(false), Q2(true), Q3(true), Q4(false);
  
  States eyes;
  States smile;
  States sad;
  States space;
  
  static {
    Q0.eyes = Q1; Q0.smile = Q4; Q0.sad = Q4; Q0.space = Q0;
    Q1.eyes = Q4; Q1.smile = Q2; Q1.sad = Q2; Q1.space = Q4;
    Q2.eyes = Q4; Q2.smile = Q4; Q2.sad = Q4; Q2.space = Q3;
    Q3.eyes = Q1; Q3.smile = Q4; Q3.sad = Q4; Q3.space = Q3;
    Q4.eyes = Q4; Q4.smile = Q4; Q4.sad = Q4; Q4.space = Q4;
  }

  States transition(char ch) {
    switch (ch) {
      case ':':
          return this.eyes;
      case ')':
          return this.smile;
      case '(':
          return this.sad;
      case '_':
          return this.space;
      default:
        throw new RuntimeException("Symbol is " +
                "not in the alphabet");
      }
  }
  
  //...
}
{% endhighlight %}

First few lines are nothing new. On the Line 5, like I said, we are introducing some instance variables. There are four 
symbols in the alphabet, thus each state will have four transitions. For example `States eyes;` represents a state, to which
current state would transit if input symbol would be `:`.
 
Given these extra variables, we need to assign values to them. We do this in `static {...}` block. You may ask "Why can't we pass 
values as arguments to the constructor, like we did for `final boolean accept;` variable. For two reasons: 

1. **Forward referencing**. When we are trying to assign a state to the variable, some of the states are not have been 
initialised. For example, to assign
`Q1` to `eyes` in `Q0` we need reference of `Q1`, but it is not initialised yet, so we get an error.
2. **Circular referencing**. It is similar to forward referencing. We are trying to pass non existing state to the constructor.
 The difference is, that constructor belongs to the state itself. For example, passing `Q4` to `Q4` constructor.

On the other hand, in `static {...}` block all the states are initialised and available to use. For convenience, I arranged assignments of variables, so that 
they would resemble transition table. Also, lets not forget about `States transition(char ch) {...}` method which checks what kind of 
character is passed, and based on that returns next state. We could have transition function in `public class DFA {...}` class, it would
make no difference.

{::options parse_block_html="true" /}

<div class="note alert"> 
**IMPORTANT:** Contrary to formal definition of DFA, in my implementation, transition function is not total, but partial. Because it maps
only four characters from the alphabet (Unidocde). If input character is not one of the four, then exception is
thrown. In theory, it should not happen. To fix that, I could create `private enum Alphabet {...}` with four members
representing four charachters and change
transition signature to `States transition(Alphabet symbol) {...}`. This way would be impossible to pass non
existing characters of the alphabet to transition method.
</div>

{::options parse_block_html="false" /}

The only thing what is left is to create `public class DFA {...}` with the method `public boolean accept(String string) {...}` which checks
if a string belongs to the language.

<div class="env-header">DFA accept</div>
{% highlight java linenos%}
public class DFA {

  private enum States {
  //...
  }
  
  public boolean accept(String string) {
    States state = States.Q0;
    
    for (int i = 0; i < string.length(); i++) {
      state = state.transition(string.charAt(i));
    }
    return state.accept;
  }
}
{% endhighlight %}

## Additional notes

Using enums we can implement any DFA. Unfortunately they are not very flexible. Meaning, that if we would like to introduce 
additional state or extra character, then we would need to modify enum itself, which violates 
[Open/Close principle](https://en.wikipedia.org/wiki/Open/closed_principle). For that we could use
[state pattern](https://en.wikipedia.org/wiki/State_pattern). But if you are thinking about modification of DFA, then
probably your don't need DFA in the first place. Because by definition, DFA (Deterministic Finite Automata) is **finite**, 
meaning it has finite set of states, and **deterministic** - its next state is determined by input symbol.

[Download](https://gist.github.com/grrinchas/534614339f786c7528a588426f08d4ef)