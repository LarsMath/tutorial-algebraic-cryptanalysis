# 3-TI Exercise

In this exercise we will algebraically cryptanalyse 3-Tensor Isomorphism.
The problem is, given two tensors, find a transformation between them:
> Given $\mathcal{C}, \mathcal{D} \in \mathbb{F}_q^{n\times m \times k}$, find invertible matrices $(\mathbf{A}, \mathbf{B}, \mathbf{T}) \in GL_n \times GL_m \times GL_k$ such that:
> $$ \sum_{i,j,l} \mathcal{C}_{ijk} \mathbf{A}_{ii'} \mathbf{B}_{jj'} \mathbf{T}_{ll'} = \mathcal{D}_{i'j'l'}$$

In the notebook [3TI.ipynb](./3TI.ipynb) you can find a setup that generates a random 3-TI problem and a method for testing your solution.
Try to solve the problem by designing the secret $(\mathbf{A}, \mathbf{B}, \mathbf{T})$ as solutions to a system of polynomial equations.
Afterwards, you can either use your FXL algorithm or Sage's built-in [variety()](https://doc.sagemath.org/html/en/reference/polynomial_rings/sage/rings/polynomial/multi_polynomial_ideal.html#sage.rings.polynomial.multi_polynomial_ideal.MPolynomialIdeal_singular_repr.variety).
In case you use your own FXL implementation, the systems are likely not semi-regular, so play around with your $D, k$ parameters to see what works.

Try to see how large you can get $n,m,k$ while still being able to break the problem.
If you reach the limit of your model, try to see if you can alter your model for better results.
For 3-TI there are a lot of different algebraic models.
Currently, most applications of this problem use $n \approx m \approx k$, but you can play around with different values and see its effect.

Hint: The 3-TI problem is equivalent to MCE. So be sure to check out the MCE side-channel exercise as well for valuable intuition.

Hint2: The expectation value for the amount of solutions is $q^{n^2 + m^2 + k^2 - nmk}$, for low $n, m, k$ this can be much bigger than 1.

## Guiding questions

Answering these questions can guide you in designing an initial model and refining it over time.
If you have an idea for a model, try it! 
Implement it, test it, and see what happens.
You can always revisit the questions later.
We don’t expect you to tackle all of them (or even one) today, focus on what sparks your curiosity and build from there.

Algebraic cryptanalysis is an iterative process: alternating between thinking about the problem and experimenting with new models.
There’s no such thing as a bad model. 
Even if it doesn’t perform well, it will help you build intuition that aid future approaches.

- Look at the problem statement, what are the unknowns. Can we model these as variables in a polynomial system?
- What degree do the equations have? What degree do we like them to have?
- If $(\mathbf{A}, \mathbf{B}, \mathbf{T})$ is a solution, then $(\lambda \mathbf{A}, \mu \mathbf{B}, \lambda^{-1}\mu^{-1}\mathbf{T})$ is a solution as well for $\lambda, \mu \in \mathbb{F}_q^*$. Can we use this to our advantage?
- What happens if apply $\mathbf{T}^{-1}$ to both sides of the equation? What happens to the amount of variables and equations? Also, what degree would these new equations be?
- For these new equations, what is the degree of the $\mathbf{T}^{-1}$ variables in these equations? Can we somehow remove these variables?
- Can we do the same trick for $\mathbf{A}^{-1}$ and $\mathbf{B}^{-1}$?