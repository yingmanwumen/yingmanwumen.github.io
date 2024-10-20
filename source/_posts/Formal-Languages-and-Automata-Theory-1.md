---
title: Formal Languages and Automata Theory [1]
excerpt: 'A brief introduction to formal languages and regular expressions.'
date: 2024-10-20 18:17:09
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

