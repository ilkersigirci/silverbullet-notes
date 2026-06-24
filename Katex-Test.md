---
title: Katex Test
date: 2026-06-24
tags: 
  - md-test
---

# KaTeX Test

This page is meant to verify that Silverbullet renders KaTeX correctly.

If everything is working, the formulas below should display as proper math instead of plain text.

## Inline Math

Euler's identity is written as $e^{i\pi} + 1 = 0$.

The quadratic formula looks like $x = \frac{-b \pm \sqrt{b^2 - 4ac}}{2a}$.

For a simple limit, write $\lim_{x \to 0} \frac{\sin x}{x} = 1$.

## Display Math

$$
\int_{-\infty}^{\infty} e^{-x^2}\,dx = \sqrt{\pi}
$$

$$
\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}
$$

$$
\mathbf{A}
=
\begin{bmatrix}
1 & 2 \\
3 & 4
\end{bmatrix}
,\quad
\det(\mathbf{A}) = -2
$$

## Piecewise Function

$$
f(x) =
\begin{cases}
x^2 & \text{if } x \ge 0 \\
-x & \text{if } x < 0
\end{cases}
$$

## Operators

The gradient is $\nabla f(x, y)$ and the derivative is $\frac{d}{dx} \sin(x) = \cos(x)$.

The binomial theorem expands as:

$$
(a+b)^n = \sum_{k=0}^{n} \binom{n}{k} a^{n-k} b^k
$$

## What to Check

- Inline math should stay on the same line as the text.
- Display math should be centered and spaced like an equation block.
- Fractions, roots, sums, integrals, matrices, and braces should all render cleanly.
- If any formula shows raw `$...$` or `$$...$$`, KaTeX is not being applied correctly.
