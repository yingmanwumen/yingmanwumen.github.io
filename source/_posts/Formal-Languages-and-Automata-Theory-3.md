---
title: Formal Languages and Automata Theory [3]
excerpt: ''
date: 2024-10-22 23:21:04
---

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
