# P03 report — value iteration

Name:
Student ID:

Two figures (see "What to hand in" in the README) and about five lines per
answer, written in the body of your email. Pass/fail; the numbers you quote must
be the ones your code printed.

---

## Q1 — the slope is the theorem

Attach `contraction.png`. In the left panel the measured error and the dashed
$\gamma^k \lVert V_0 - V^{*}\rVert_\infty$ lie on top of each other until
sweep 7; from sweep 8 the error is exactly zero, and the plot draws it at the
bottom of the axis.

State, for $\gamma = 0.9$: the slope of the measured line on the log axis, and
what that slope is in closed form. Then run `python experiments.py --slip 0.1`
and say whether the measured line still lies on the dashed one, and why that
does not contradict the theorem.

**Answer:**
For $\gamma = 0.9$, the closed-form slope of the error on the log axis is $\log(\gamma)$ (numerically $\ln(0.9) \approx -0.1054$, or $\log_{10}(0.9) \approx -0.0458$). On the deterministic grid, the ratio between consecutive errors attains this bound exactly ($\rho_k = \gamma = 0.900000000$).
With `slip = 0.1`, the measured line no longer lies on the dashed one: the error falls faster and stays well below the theoretical bound, with consecutive error ratios around $\approx 0.372$. This does not contradict the contraction theorem because the theorem establishes an upper bound ($\|BV - BV^{*}\|_\infty \le \gamma \|V - V^{*}\|_\infty$), meaning contraction can be strictly faster than rate $\gamma$.

## Q2 — the bound is right about the rate and wrong about the horizon

The script prints this table:

```
 slip  gamma   measured    bound    exact zero at
  0.0   0.90          8      138                8
  0.0   0.99          8     1382                8
  0.1   0.90         22      138               45
  0.1   0.99         25     1382               50
  0.3   0.90         39      138               95
  0.3   0.99         50     1382              110
```

`measured` is the number of sweeps to reach $10^{-6}$; `bound` is
$\log(1/\varepsilon)/(1-\gamma)$, the standard estimate.

Two things need explaining, and they are different.

**(a)** On the deterministic grid (`slip = 0`) the error reaches **exactly zero**
after 8 sweeps, and 8 does not depend on $\gamma$ at all. Where does the 8 come
from? Why does the $1/(1-\gamma)$ factor never appear?

**Answer (a):**
The number 8 corresponds to the Manhattan distance (shortest path length) from the initial state $(0,0)$ to the absorbing goal $(4,4)$ on the $5 \times 5$ grid. Because the grid is deterministic and transitions form a directed acyclic path under the optimal policy, each Bellman sweep propagates the true reward exactly one step backwards. Once the goal is reached, it is an absorbing state that yields zero subsequent reward. Therefore, there is no infinite tail of future rewards to accumulate asymptotically; the sum is finite ($\gamma^8$) and error reaches exact zero at sweep 8, preventing the $1/(1-\gamma)$ horizon factor from appearing.

**(b)** With `slip = 0.1` the bound is 138 against a measured 22 at
$\gamma = 0.9$, and 1382 against 25 at $\gamma = 0.99$. Raising $\gamma$ by a
factor that multiplies the bound by ten changes the measurement by three sweeps.
What is the bound quantifying that this MDP does not have?

Your answer should use the words *worst case over all MDPs* and should say what
property of this gridworld makes it a long way from that worst case.

**Answer (b):**
The bound $\log(1/\varepsilon)/(1-\gamma)$ assumes the *worst case over all MDPs*, which occurs when an MDP allows an agent to remain in non-terminal reward-generating cycles indefinitely, meaning the value depends on the full infinite effective horizon $1/(1-\gamma)$. This gridworld is very far from that worst case because it features a bounded diameter and an absorbing goal with zero reward after entry. Even with `slip = 0.1`, the expected hitting time to reach absorption is short, so the effective planning horizon is governed by the physical transit time across the grid rather than the infinite geometric horizon $1/(1-\gamma)$.

## Q3 — what you predicted in Part C

For each of the two changes: what you predicted before running, what happened,
and — if you were wrong — which assumption of the theorem you had been using
without noticing.

For $\gamma = 1$ in particular: say whether what you observed contradicts the
contraction theorem, and justify the answer from the theorem's hypotheses rather
than from the plot.

**Answer:**
1. **Using `bellman_expectation` for a fixed policy $\pi$:**
   - *Prediction:* The iteration will converge to the policy-specific value function $V^\pi$ rather than the optimal value $V^*$, contracting with modulus $\gamma$.
   - *What happened:* Exactly as predicted. Because $\mathcal{B}^\pi$ is also a $\gamma$-contraction under the supremum norm ($\|\mathcal{B}^\pi U - \mathcal{B}^\pi V\|_\infty \le \gamma \|U - V\|_\infty$), the iterations converge to $V^\pi$ at the same geometric rate.

2. **Setting $\gamma = 1$:**
   - *Prediction:* Theoretically, in a general MDP without discounting, value iteration can diverge or cycle. But in this gridworld, because reward is paid exactly once upon entering the absorbing goal and states form a finite acyclic path, values should converge to 1.0 (rather than $\gamma^8$) in 8 sweeps.
   - *What happened:* As predicted, the error reaches exactly zero at sweep 8 and the values stabilize at 1.0 for all reachable states.
   - *Does this contradict the contraction theorem?* No. The Banach Fixed Point Theorem provides a sufficient condition: *if* $\gamma < 1$, *then* the operator is a contraction and convergence to a unique fixed point is guaranteed. It does not state that $\gamma < 1$ is a necessary condition for convergence in every MDP. When $\gamma = 1$, the hypothesis is violated, meaning the theorem makes no claims or promises. Convergence occurs here not because of discounting, but due to the episodic structure (a Stochastic Shortest Path problem with an absorbing state and finite horizon).

## Declaration

Who you worked with, and which tools you used and for what.
