# Expected Number of Coin Flips to get HH

## Problem Description (English)
What is the expected number of fair coin flips to get two consecutive heads (**HH**)?

## Mathematical Derivation (State Method)
We define the states based on the progress towards the goal "HH":
- $E_0$: Expected remaining flips when we have 0 consecutive heads. (Start state)
- $E_1$: Expected remaining flips when we have 1 consecutive head.
- $E_2$: Expected remaining flips when we have 2 consecutive heads. (Goal state, $E_2 = 0$)

### Setting up the Equations
From each state, we consider the outcome of the next flip (Head with prob 0.5, Tail with prob 0.5):

1.  **From $E_0$**:
    - Flip H (0.5 prob): We move to state $E_1$.
    - Flip T (0.5 prob): We stay in state $E_0$.
    $$E_0 = 1 + 0.5 E_1 + 0.5 E_0$$
    Simplify: $0.5 E_0 = 1 + 0.5 E_1 \implies E_0 = 2 + E_1$ (Eq. 1)

2.  **From $E_1$**:
    - Flip H (0.5 prob): We move to state $E_2$ (Goal).
    - Flip T (0.5 prob): We go back to state $E_0$.
    $$E_1 = 1 + 0.5 E_2 + 0.5 E_0$$
    Since $E_2 = 0$:
    $$E_1 = 1 + 0.5 E_0$$ (Eq. 2)

### Solving the System
Substitute (Eq. 2) into (Eq. 1):
$$E_0 = 2 + (1 + 0.5 E_0)$$
$$E_0 = 3 + 0.5 E_0$$
$$0.5 E_0 = 3 \implies E_0 = 6$$

**Result**: The expected number of flips to get HH is **6**.

---

## Comparison: Expected flips for HT
Interestingly, the expected number of flips for **HT** is different:
- $E_0 = 1 + 0.5 E_H + 0.5 E_0 \implies E_0 = 2 + E_H$
- $E_H = 1 + 0.5 E_{HT} + 0.5 E_H \implies E_H = 1 + 0 + 0.5 E_H \implies 0.5 E_H = 1 \implies E_H = 2$
- $E_0 = 2 + 2 = 4$.

**Conclusion**: It takes an average of **6** flips for **HH** but only **4** flips for **HT**. This is because if you fail on the second flip of HH (i.e., you get HT), you are sent all the way back to the start. If you fail on the second flip of HT (i.e., you get HH), you still have a 'H' ready for the next attempt.

## Spoken / Interview Answer
"I can solve this using the **State Transition** method. We define $E_0$ as the expected flips from the start, and $E_1$ as the expected flips after getting one head. 
For $E_0$, the next flip either gets us to $E_1$ or keeps us at $E_0$, so $E_0 = 1 + 0.5E_1 + 0.5E_0$. 
For $E_1$, the next flip either finishes the task or sends us back to $E_0$, so $E_1 = 1 + 0.5(0) + 0.5E_0$. 
Solving these linear equations, we find that $E_0 = 6$. 
A common follow-up is to compare this with **HT**, which only takes **4** flips on average because a 'fail' flip for HT (getting HH) doesn't reset your progress as completely as it does for HH."


