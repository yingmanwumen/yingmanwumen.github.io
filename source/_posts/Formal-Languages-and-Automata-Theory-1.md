---
title: Formal Languages and Automata Theory
date: 2024-10-20 18:17:09
excerpt: 'A brief introduction to formal languages and regular expressions.'
---

# Formal Languages

## Fundamental Facts

> A language can be seen as a system suitable for expression of certain ideas, facts and concepts.

- A **language** is a collection of **sentences**
- A sentence is a sequence of **words**
- A word is a combination of **syllables**
- The set of syllables is called **alphabet**: $\Sigma$, which is a ***non-empty finite set***
- A word or a sentence can be called as a **string**, and a string over an alphabet $\Sigma$ is written as $a_1 a_2 \ldots a_n$, where $a_i$ is the element of $\Sigma$
- $\varepsilon$ denotes an **empty string**
- $\Sigma^*$ denotes **the set of all strings** over $\Sigma$, including $\varepsilon$

### Concatenation over strings

- A **concatenation** of two strings $x$ and $y$ is denoted by $x y$.
- And it is clear that binary concatenation over $\Sigma^*$ is ***associative***. For instance, $\forall x, y, z \in \Sigma^*$:

$$
x(yz) = (xy) z = x y z\\
x \varepsilon = \varepsilon x = x
$$

>  $\Sigma^*$ is a monoid with respect to concatenation.

- But concatenation is ***not commutative***: $x y \neq y x$.
- With $x^0 = \varepsilon$, $x^{n+1} = x^n x$
- $|x|_a$ denotes the count of occurrences of $a$ in $x$, where $a \in \Sigma$, $x \in \Sigma^*$
- Then **the length of a string** is defined as:

$$
|x| = \sum_{a \in \Sigma} |x|_a
$$

- $|\varepsilon| = 0$
- If $y = uxv$, where $x, y, u, v \in \Sigma^*$, then $x$ is a **substring** of $y$
- Specifically, $x$ is a **prefix** of $y$ when $u = \varepsilon$, and $x$ is a **suffix** of $y$ when $v = \varepsilon$

## Languages

- A **language** $L$ is a subset of $\Sigma^*$
- $\varnothing$ is a language over any $\Sigma$, as well as $\set \varepsilon$
- $\varnothing \ne \set \varepsilon$, for $|\set \varepsilon| = 1$ while $|\varnothing| = 0$

- The concatenation of two languages $L_1$ and $L_2$ is denoted by:

$$
L_1 L_2 = \set{x y | x \in L_1 \land y \in L_2}\\
$$

- The concatenation of languages is ***associative*** since the concatenation of strings is associative.

$$
L_1 L_2 L_3 = (L_1 L_2) L_3 = L_1 (L_2 L_3)\\
L \set \varepsilon = \set \varepsilon L = L\\
L \varnothing = \varnothing L = \varnothing
$$

- And the concatenation of a language is not ***commutative*** since the concatenation of strings is not concatenation.

$$
L_1 L_2 \ne L_2 L_1
$$

- With $L^0 = \set \varepsilon$, $L^{n+1} = L^n L$
- Define **Kleene closure** $L^*$ of a language $L$ as:

$$
\begin{aligned}
  L^* &= \bigcup_{n \geq 0} L^n \\
      &= L^0 \cup L^1 \cup L^2 \cup \ldots \\
      &= \set{x_1 x_2 \ldots x_n | n \ge 0, x_i \in L, 1 \le i \le n} \\
      &= \Sigma^*
\end{aligned}
$$

> Kleene closure is frequently used in regular expressions.

- $\varnothing^* = \set \varepsilon$
- $\set \varepsilon^* = \set \varepsilon$
- $(L^* )^* = L^* $
- $L^* L^* = L^* $

- Define **Positive closure** $L^+$ of a language $L$ as:

$$
L^+ = \bigcup_{n \geq 1} L^n
$$

- Thus, $L^* = L^+ \cup \set \varepsilon$. So if $\varepsilon \in L$, then $L^* = L^+$

$$
L^* L = LL^* = L^+\\
$$

- Distributive properties:

$$
(L_1 \cup L_2) L_3 = L_1 L_3 \cup L_2 L_3 \\
L_1 (L_2 \cup L_3) = L_1 L_2 \cup L_1 L_3
$$

## Finite Representation and Regular Expressions

> One should be able to come up with all sentences of a language with limited information.

- Giving a **finite** amount of information, all the strings of a language shall be **enumerated/validated**

- Regular expression is defined over $\Sigma$ recursively:
  - $(r + s)$ represents $R \cup S$
  - $(r s)$ represents $R S$
  - $(r*)$ represents $R^*$
  - where $R$, $S$ are languages and $r$, $s$ are regular expressions

- $L(r)$ denotes a language which is represented by $r$ that is a regular expression.

- $\varnothing$, $\set \varepsilon$, and all finite sets are regular

- $\Sigma^*$ is also regular

- $a \approx b$ represents the regular expressions $a$ and $b$ are equivalent

# Grammars

> A grammar should include terminals and non-terminals along with a set of rules which attribute some information regarding the non-terminals symbols.

$$
\mathcal{G} = (N, \Sigma, P, S)
$$

- $N$: a finite set of nonterminals
- $\Sigma$: a finite set of terminals
- $S$: the start symbol. $S \in N$
- $P$: a finite set of production rules. $P \subset N \times V^*$, where $V = N \cup \Sigma$

For the production rule $(A, \alpha) \in P$, write $A \rightarrow \alpha$

- For grammar $\mathcal{G} = (N, \Sigma, P, S)$ with $V = N \cup \Sigma$, define a binary relation $\underset{\mathcal{G}}{\Rightarrow}$ on $V^*$

  - $\forall \alpha,\beta \in V^*$, we have: $\alpha \underset{\mathcal{G}}{\Rightarrow} \beta \Longleftrightarrow \alpha = \alpha_1 A \alpha_2, \beta=\alpha_1 \gamma \alpha_2, A \rightarrow \gamma \in P$

  - $\alpha \underset{\mathcal{G}}{\Rightarrow} \beta$ called $\alpha$ ***yields*** $\beta$ in one step in $\mathcal{G}$

  - If after limited operations with $\underset{\mathcal{G}}{\Rightarrow}$, $\alpha$ yields $\beta$, write $\alpha \overset{ * }{\underset{\mathcal{G}}{\Rightarrow}} \beta$. The times of the operations is $n$, and it is written as $\alpha \overset{n}{\underset{\mathcal{G}}{\Rightarrow}} \beta$

  - $\alpha \overset{ * }{\underset{\mathcal{G}}{\Rightarrow}} \beta$ called $\alpha$ ***derives*** $\beta$ in $\mathcal{G}$

  - If $S \overset{ * }{\underset{\mathcal{G}}{\Rightarrow}} \alpha$, then the string $\alpha \in V^*$ is called a **sentential form** in $\mathcal{G}$

  - The language generated by $\mathcal{G}$ is denoted by $L(\mathcal{G})$

$$
L(G) = \set{ x \in \Sigma^* | S \overset{ * }{\underset{\mathcal{G}}{\Rightarrow}} x}
$$

## CFG: Context Free Grammar

A context free grammar or simply a CFG is a grammar in which $A$ is a nonterminal in every production rule $A \rightarrow \alpha$.

## Derivations

- **Leftmost derivation**: the production rule applied at each step is on the leftmost nonterminal symbol of the sentential form. It is denoted by $A \overset{*}{\Rightarrow}_L x$
- **Rightmost derivation**: the production rule applied at each step is on the rightmost nonterminal symbol of the sentential form. It is denoted by $A \overset{*}{\Rightarrow}_R x$

$$
A \overset{*}{\Rightarrow} x \Longleftrightarrow A \overset{*}{\Rightarrow}_L x
$$

- ***Two equivalent leftmost derivations are identical***

## Ambiguity and Regular Grammars

- A CFG might give several inquivalent derivations for a string(two or more leftmost or rightmost derivations). This leads to an ambiguity in parsing.
- A grammar with this property is ***ambiguous***.

> A CFG $\mathcal{G}$ is said to be ***linear*** if every production rule of $\mathcal{G}$ has at most one nonterminal symbol in its righthand side.

- If a CFG is linear, every derivation in it is a leftmost derivation as well as a rightmost derivation

Right linear derivations:

$$
A \rightarrow x \quad or \quad A \rightarrow xB
$$

Leftmost linear derivations:

$$
A \rightarrow x \quad or \quad A \rightarrow Bx
$$

> The language generated by a right linear grammar is regular.

- For every regular language $L$, there exists a right linear grammar that generates $L$

# Finite Automata

## DFA

$$
\mathcal{A} = (Q, \Sigma, \delta, q_0, F)
$$

- $Q$ is a finite set of states
- $\Sigma$ is a finite set of input alphabet
- $q_0 \in Q$, the initial state
- $F \subset Q$, the set of final states
- $\delta: Q \times \Sigma \rightarrow Q$, the transition function/next-state function

For every state and input symbol, the transition function $\delta$ assigns a unique next state.

Example of a transition table of a DFA:

$$
\begin{array}{r|cc}
\delta & a & b \\
\hline
\rightarrow p & q & p \\
q & r & p \\
\textcircled{r} & r & r \\
\end{array}
$$

$\forall q \in Q, x \in \Sigma^*, a \in \Sigma$, define $\hat\delta: Q \times \Sigma^* \rightarrow Q$, where 

$$
\begin{aligned}
\hat\delta(q, \varepsilon) &= q\\
\hat\delta(q, xa) &= \delta(\hat\delta(q, x), a)
\end{aligned}
$$

Then the string $x \in \Sigma^*$ that is accepted by the DFA satisfies $\hat\delta(q_0, x) \in F$.

The language $L$ accepted by $\mathcal{A}$ is denoted by:

$$
L(\mathcal{A}) = \{x \in \Sigma^* | \hat\delta(q_0, x) \in F\}
$$

A **configuration** is an element of $Q \times \Sigma^*$: $(q, x)$. For example, the initial configuration of a given string $x$ is $(q_0, x)$, while the final configuration is $(p, \varepsilon)$.

If there are two configurations $C = (p, x)$ and $C^{'} = (q, y)$, and $\delta(p, a) = q$, $x = ay$, then we say that the DFA $\mathcal{A}$ moves from $C$  to $C^{'}$:

$$
C \vdash_\mathcal{A} C^{'}
$$

And the reflective closure of $\vdash$ is denoted by $\vdash^{ * }$:

$$
C \vdash^{ * } C^{'} \Longleftrightarrow C = C_0 \vdash C_1 \vdash C_2 \vdash \dots \vdash C_n = C^{'}
$$

## NFA

$$
\mathcal{N} = (Q, \Sigma, \delta, q_0, F)
$$

- $Q$, $F$, $q_0$, and $F$ are as in a DFA
- $\delta: Q \times (\Sigma \cup \{ \varepsilon \}) \rightarrow \wp(Q)$

Example of a transition table of a NFA:

$$
\begin{array}{r|ccc}
\delta & a & b & \varepsilon \\
\hline
q_0 & \{q_1\} & \varnothing & \{q_4\} \\
q_1 & \varnothing & \{q_1\} & \{q_2\} \\
q_2 & \{q_1, q_3\} & \{q_3\} & \varnothing \\
q_3 & \varnothing & \varnothing & \varnothing \\
q_4 & \{q_1\} & \varnothing & \{q_4\} \\
\end{array}
$$

An **$\varepsilon$-closure** of a state $p$, denoted by $E(p)$ is defined as the set of all states that are reachable from $p$ via zero or more **$\varepsilon$-closure**.

Define $\hat{\delta}: Q \times \Sigma^* \rightarrow \wp(Q)$:

$$
\begin{aligned}
\hat{\delta}(q, \varepsilon) &= E(q)\\
\hat{\delta}(q, xa) &= E(\bigcup_{p\in\hat\delta(q, x)} \delta(p, a))
\end{aligned}
$$

If a string $x \in \Sigma^*$ is accepted by an NFA $\mathcal{N}$, then

$$
\hat\delta(q_0, x) \cap F \ne \varnothing
$$

## DFA and NFA

> Given an NFA with some $\varepsilon$-transitions, there exists an equivalent DFA without $\varepsilon$-transitions.

> For every NFA $\mathcal{N}^{'}$ without $\varepsilon$-transitions, there exists a DFA $\mathcal{A}$ such that $L(\mathcal{N^{'}}) = L(\mathcal{A})$.

We can convert a NFA to a DFA by:

1. Convert the NFA to an equivalent NFA without $\varepsilon$-transitions
2. Convert the NFA without $\varepsilon$-transitions to a DFA
