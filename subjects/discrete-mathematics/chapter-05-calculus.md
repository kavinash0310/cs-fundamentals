# Chapter 5 — Calculus for Computer Science

## 1. Limits and continuity

A limit describes behavior near x=a, not necessarily the function's value at a. A function is continuous at a when the limit exists and equals f(a). For polynomials and rational functions (where denominator is nonzero), substitute directly.

The derivative is the instantaneous rate of change:
~~~text
f'(x)=lim(h→0) [f(x+h)-f(x)]/h
~~~
Rules: derivative of xⁿ is nxⁿ⁻¹; sum rule is termwise; product rule (fg)'=f'g+fg'; chain rule (f(g(x)))'=f'(g(x))g'(x).

Example: f(x)=x²-3x. f'=2x-3; at x=2 slope is 1.

## 2. Maxima, minima, and mean value theorem

An interior differentiable local extremum has f'(x)=0. These critical points are candidates, not automatic maxima/minima. Check endpoints and use sign changes or second derivative.

For f(x)=x²-4x+5 on [0,3], f'=2x-4; critical x=2. Values: f(0)=5, f(2)=1, f(3)=2, so minimum 1 and maximum 5.

Mean Value Theorem: if f is continuous on [a,b] and differentiable on (a,b), some c satisfies f'(c)=(f(b)-f(a))/(b-a). It links average slope to an instantaneous slope.

## 3. Integration

Definite integral is signed area/accumulation. Antiderivative rules reverse differentiation: ∫xⁿdx=xⁿ⁺¹/(n+1)+C for n≠-1. The Fundamental Theorem of Calculus: if F'=f, then ∫ₐᵇ f(x)dx=F(b)-F(a).

Example: ∫₀² 3x²dx = [x³]₀²=8. Integrals can represent total work, probability density, or accumulated cost.

## 4. Error checks

Derivative gives a local rate; integral gives accumulation. A stationary point may be inflection rather than extremum. Include interval endpoints for constrained optimization. When integrating a density, total probability must be 1.

## 5. Questions and practice

**Q1.** Differentiate x³+2x. **A:** 3x²+2.
**Q2.** Find critical point of x²-6x+10. **A:** x=3.
**Q3.** Evaluate ∫₀¹ 2x dx. **A:** 1.
**Q4.** What conditions does the Mean Value Theorem require? **A:** Continuity on closed interval, differentiability inside.
**Practice:** Differentiate a product and a composition; find extrema on a closed interval; evaluate a polynomial integral; explain why f'(x)=0 alone does not prove a maximum.

## Notebook work

Sketch a function with a local minimum and label tangent slope, then interpret a definite integral as area and accumulated quantity.


## Worked derivative and integral

For f(x)=x(10-x)=10x-x² on [0,10], derivative f'=10-2x. Set it to zero: x=5. The derivative is positive before 5 and negative after, so maximum f(5)=25. Endpoints are zero.

Chain rule: d/dx (3x+1)⁴ = 4(3x+1)³·3. Forgetting the inner derivative is a common error.

Integration example: ∫₀³(2x+1)dx = [x²+x]₀³=12. Check by interpreting area under a line or differentiating the antiderivative.

Continuity trap: a function can have a limit at a point but be assigned a different value there, so it is not continuous. A derivative may be zero at a stationary inflection, e.g. x³ at x=0, without a local maximum/minimum.

**Practice:** Find max/min of x²-2x on [0,3] by checking critical point and endpoints. Differentiate sin(x²) using chain rule if familiar; otherwise explain the chain rule structure with a polynomial. Integrate 3x² from 1 to 2 = 7.