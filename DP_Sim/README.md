# $(\epsilon, \delta)$ - Differential Privacy Simulation

This folder contains a Python-based Jupyter Notebook demonstrating the formal mathematical bounds and practical implementations of Differential Privacy (DP). It compares the accuracy and confidence intervals of the Laplacian (Pure DP) and Gaussian (Approximate DP) mechanisms across varying privacy budgets ($\epsilon$) and failure probabilities ($\delta$).

## Theoretical Foundation

The simulation is grounded in the formal definition of $(\epsilon, \delta)$-Differential Privacy. A randomized mechanism $\mathcal{M}$ guarantees $(\epsilon, \delta)$-DP if for all neighboring databases $\chi$ and $\mathcal{Y}$ differing by exactly one element ($\vert{}\vert{}\chi - \mathcal{Y}\vert{}\vert{}_1 \leq 1$), the following inequality holds symmetrically:

$$Pr[\mathcal{M}(\chi) = \mathbb{E}] \leq e^\epsilon Pr[\mathcal{M}(\mathcal{Y}) = \mathbb{E}] + \delta$$

| Mechanism | DP Type | Noise Distribution | Characteristics |
| :--- | :--- | :--- | :--- |
| **Laplace** | Pure $(\epsilon, 0)$-DP | $Lap(0, \frac{\sqrt{2}}{\epsilon})$ | Strictly bounds the likelihood ratio. Ensures privacy loss never exceeds $\epsilon$. |
| **Gaussian** | Approximate $(\epsilon, \delta)$-DP | $\mathcal{N}(0, \frac{2\ln(1.25/\delta)}{\epsilon^2})$ | Allows a small probability ($\delta$) of privacy bound failure. Often analyzed for tighter bounds in high-dimensional vector queries. |

## Simulation Methodology

1. **Synthetic Database Formulation:** The notebook accepts user input to generate a 3-category histogram database $\chi = (x_1, x_2, x_3)$.
2. **Query Function:** Evaluates a 2D query $f(\chi) = (x_1 + x_2 + x_3, x_1 + x_3)$.
3. **Sensitivity Calculation:** Derives the $l_2$ global sensitivity for the specific query: $\Delta f = \max \vert{}\vert{}f(\chi) - f(\mathcal{Y})\vert{}\vert{}_2 = \sqrt{2}$.
4. **Noise Injection:** Iterates through 1000 evenly spaced $\epsilon$ values $\in (0, 20]$ with a fixed $\delta = 1/9$, applying both Laplace and Gaussian noise to the true output components.

## Key Visualizations & Analysis

The notebook generates two primary visualization sets to evaluate mechanism behavior:

### 1. Confidence Intervals vs. Privacy Budget ($\epsilon$)
A scatter plot tracks the perturbed query outputs against the true outputs, overlaying the theoretical confidence intervals ($c = 0.95$) for both mechanisms. As $\epsilon$ increases (representing weaker privacy), the interval widths decrease inversely proportional to $\epsilon$:

* **Gaussian Half-Width:** $\frac{W_G}{2} = 2 \frac{\Delta f}{\epsilon} erf^{-1}(c) \sqrt{\ln(\frac{1.25}{\delta})}$
* **Laplacian Half-Width:** $\frac{W_L}{2} = \frac{\Delta f}{\epsilon} \ln(\frac{1}{1 - c})$

### 2. Mechanism Accuracy Decision Boundary
To determine which mechanism provides tighter confidence bounds, the simulation visualizes the ratio $\frac{W_G}{W_L}$. This ratio is independent of $\epsilon$ and parameterized strictly by confidence $c$ and failure probability $\delta$.

* **3D Surface Plot:** Maps the ratio across the parameter space $c \in (0, 1)$ and $\delta \in (0, 1)$.
* **2D Contour Map:** Projects the intersection boundary at $z = 1$. The blue region ($z > 1$) indicates where the Laplace mechanism is tighter and more accurate, which covers the majority of the standard $(c, \delta)$ parameter space. The red region ($z < 1$) shows where the Gaussian mechanism dominates.

## Requirements and Usage

**Dependencies:**
* `numpy`
* `matplotlib`
* `scipy`

**Execution:**
Run the Jupyter Notebook cells sequentially. The environment will prompt you to input integer values for the dataset categories:
```text
Enter no. of samples in category 1: [Integer]
Enter no. of samples in category 2: [Integer]
Enter no. of samples in category 3: [Integer]
