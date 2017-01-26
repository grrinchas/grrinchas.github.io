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
description: DFA (Deterministic Finite Automata) implementation in JAVA.
---

**DFA (Deterministic Finite Automata)** sometimes called **DFSM (Deterministic Finite State Machine)** is a simple computational machine. Although there are many usages of it, I will focus
on using DFA for language recognition and will show how to implement DFA in JAVA. But before we start to write a
code let's refresh our knowledge about them.

## State Diagram

State diagrams are very nice way of defining and visually representing finite automatas. Unfortunately, these diagrams
tend to be very messy if DFA's have many states. Nevertheless, for simple ones is a perfect choice. For example, 

<a name="figure 1"></a>
![Automata](/resources/images/automata.svg)


Circles represents states, while arrows - transitions from one state to another. Transitions are based on input symbols near arrow.
For example, if automata's current state is **Q0** and the input symbol is `:`, then it looks for the arrow with the symbol `:`
and transitions to the state which arrow points to, in our case **Q1**. If the input symbol is `_`, then it transitions to
itself (**Q0**).

<div class="note alert"> <strong>IMPORTANT:</strong> 
Each state in DFA has to have a transition for each input symbol. But sometimes different symbols points to the same state. Thus, to simplify 
a state diagram, I merged some transitions into one arrow with multiple symbols.
</div>

We say that automata accepts string, if given last symbol, it transitions to **accept** state. Conventionally, in the state 
diagrams, all accept states are drawn as double circles. DFA can have zero or many states. For example, in 
[Figure 1](#figure 1) we can see two accept states: **Q2** and **Q3**. Contrary to accept states, DFA has to have exactly one 
**start** state (**Q0**). Some automatas may or may not have a state from which is impossible to reach an accept state, 
called **dead** state (**Q4**).

<div class="warning alert"> <strong>NOTE:</strong> 
While it is not necessary, I color coded states in the state diagram. Thus, orange stands for start, green - accept and
red for dead states.
</div>

## Regular languages
The main job of automata is to recognise a language by accepting or rejecting sequence of symbols, lets call them **characters**, 
and a sequence - **string**. But what does it mean, when we say "accepting"?
 
Let _Σ_ (read as "sigma") be an alphabet and let _A_ be a set of strings _w_, where each character in the string 
belongs to the alphabet _Σ_, then _M_ **accepts** _w_ if sequence of states _r<sub>0</sub>_, _r<sub>1</sub>_, . . .,_r<sub>n</sub>_ 
exists with three conditions:

1. _r<sub>0</sub>_ is a start state of _M_,
2. _M_ goes from one state to another based on transition function, 
3. _r<sub>n</sub>_ is one of the accept states.

We say that _M_ **recognizes** language _A_ if _M_ accepts all the strings in the language. In addition, 
language is called a **regular language**, if some finite automata _M_ recognises it. 

For example, [Figure 1](#figure 1) shows 
an automata which recognises a language consisting of the strings having unlimited smiley `:)` and sad `:(` faces. Where each face 
starts and ends by zero or more `_` characters, and each face is separated by at least one `_` character. 
Below we can see some strings which are in the language, and some which are not, 

* **Is**: `:)`, `:(`, `:(_:)`, `:)___:)__:(__`, `_________:)__`.
* **Is not**: `____`, `::)`, `:((`, `:(:()`, `:)_):)`, `(:`.

So if a string `_:)__:(` is in the language then our DFA should accept it. Let's see how it does that. Initially, DFA 
is in the **Q0** state, then it receives 
`_` character, the arrow with it points to the same state **Q0**. Next character is `:`, which points to **Q1** state, 
then following `)` DFA goes to **Q2** state. From there, we have `_` to **Q3**, then one more `_` from **Q3** to itself.
After that, `:` character points to **Q1** and following `(` DFA ends up in **Q2** state. 
As we can see, the last state is accept state, thus automata accepts string `_:)__:(`.

In similar manner we can track any string consisting of these four characters (symbols) `_`, `:`, `(`, `)` and check
if it belongs to our language. 


## Formal definition

Even state diagrams are nice to look at, they are not much of the help for translating DFAs into source code. So let's 
define it formally then.

A **deterministic finite automata** M is a 5-tuple (Q, Σ, δ, q<sub>0</sub>, F ), where

1. Q is a finite set called the **states**,
2. Σ is a finite set called the **alphabet**,
3. δ : Q × Σ → Q is the **transition function**,
4. q<sub>0</sub> ∈ Q is the **start state**, and
5. F ⊆ Q is the set of **accept states**.

This definition is much more precise. DFA has to have some set of states _Q_, from which one state has to be 
the start state _q<sub>0</sub>_ and some subset of accept states _F_. To go from one state to another it needs an alphabet
_Σ_ (sigma) and transition function _δ_ (delta). There is some ambiguity concerning _δ_ function. Is it partial or not? 
If it is partial, then it does not map every combination of the domain _Q × Σ_ to elements of codomain _Q_. Meaning, that there
exist a state and a symbol which does not have a transition function. But, as we have seen from the state diagram, that was not the case. 
For each state and symbol there was a transition to other state. Thus, transition function must be total.


Anyway. Given DFA in [Figure 1](#figure 1) its formal definition would be:

M = (Q, Σ, δ, q<sub>0</sub>, F ), where

1. Q = {**Q0**, **Q1**, **Q2**, **Q3**, **Q4**}, 
2. Σ = {`_`, `:`, `)`, `(`}, 
3. q<sub>0</sub> = **Q0**, 
4. F = {**Q2**, **Q3**}.
5. δ is described as a **transition table**

<table style="max-width:300px;margin-right:auto;margin-left:auto">
<thead><tr><th></th><th>:</th><th>)</th><th>(</th><th>_</th></tr></thead>
<tbody>
<tr><th>Q0</th><td>Q1</td><td>Q4</td><td>Q4</td><td>Q0</td></tr>
<tr><th>Q1</th><td>Q4</td><td>Q2</td><td>Q2</td><td>Q4</td></tr>
<tr><th>Q2</th><td>Q4</td><td>Q4</td><td>Q4</td><td>Q3</td></tr>
<tr><th>Q3</th><td>Q1</td><td>Q4</td><td>Q4</td><td>Q3</td></tr>
<tr><th>Q4</th><td>Q4</td><td>Q4</td><td>Q4</td><td>Q4</td></tr>
</tbody>
</table>

If we compare DFA's state diagram [Figure 1](#figure 1) and this definition, we see that transitions are more understandable. For example, 
if I want to know next state from current **Q2** given symbol `)`, I just need to find
**Q2** state in left column, then `)` in the top row, and check which state is at their intersection (`Q4`).

## DFA implementation in JAVA

Given all the information about DFA, it should not be hard to implement it. From the formal definition we see, that our automata 
has 4 states: **Q0**, **Q1**, **Q2**, **Q3**, **Q4**, which are known in advance. Thus, we can create `enum States` 
to represent them. Nice thing about JAVA enum, is that you can define methods and instance variables in them. For example, 
to differentiate accept and non accept states, we can introduce instance variable `boolean accept` for each state. 

<div class="highlighter-header">DFA states</div>
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

Just like that, we defined automata's states _Q_ and a subset of accepting states _F_. Now, let's think how to implement 
transition table. Probably your first thought is "We can use two dimensional array or a map or a hashtable". All these 
solutions are good, and they would work. Unfortunately they are not very object oriented. Think of a state as being 
an object, to which you can send some messages for enquiry or making them to do something for you. For example, we can 
ask it, if it is an accept state or not? By introducing a method `boolean isAccept()` which would return it's
instance variable `boolean accept`. But in our case, it is not necessary, because `enum States` will be a private class 
inside `class DFA` and it can access any variable from `States` directly. 


Similarly, given a character from the alphabet, we can ask a state to transit to another. So, instead of writing 
transition table and then passing it to some kind of function, we will define method `State transition(char ch)` inside 
`enum State` to get next state. For that we will have to introduce additional instance variables in the enum.
Each variable will contain a reference to the state. I think, to show is much easier than to explain.


<div class="highlighter-header">DFA transitions</div>
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
symbols in the alphabet, thus each state will have four transitions. For example `States eyes` represents a state, to which
current state would transit if input symbol would be `:`.
 
Given these extra variables, we need to assign values to them. We do this in `static` block. You may ask "Why can't we pass 
values as arguments to the constructor, like we did for `accept` variable". For two reasons: 

1. **Forward referencing**. When we are trying to assign a state to the variable, some of the states are not have been 
initialised. For example, to assign
`Q1` to `eyes` in `Q0` we need reference of `Q1`, but it is not initialised yet, so we get an error.
2. **Circular referencing**. It is similar to forward referencing. We are trying to pass non existing state to the constructor.
 The difference is, that constructor belongs to the state itself. For example, passing `Q4` to `Q4` constructor.

On the other hand, in `static` block all the states are initialised and available to use. For convenience, I arranged assignments of variables, so that 
they would resemble transition table. Also, lets not forget about `transition` method which checks what kind of 
character is passed, and based on that returns next state. We could have transition function in `DFA` class, it would
make no difference.

<div class="note alert"> <strong>IMPORTANT:</strong>
Contrary to formal definition of DFA, in my implementation, transition function is not total, but partial. Because it maps
only four characters from the alphabet (Unidocde). If input character is not one of the four, then exception is
thrown. In theory, it should not happen. To fix that, I could create <code>enum Alphabet</code> with four characters and change
transition signature to <code>States transition(Alphabet symbol)</code>. This way would be impossible to pass non
existing characters of the alphabet.
</div>

The only thing what is left is to create `class DFA` with the method `boolean accept(String string)` which checks
if a string belongs to the language.

<div class="highlighter-header">DFA accept</div>
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