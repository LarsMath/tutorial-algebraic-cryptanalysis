# MCE with side channel hints Exercise

In this exercise we will algebraically cryptanalyse MCE with side channel info.

## The problem statement

The problem is, given two matrix codes, find a transformation between them:
> Given matrix codes $\mathcal{C}, \mathcal{D}$.
> I.e. subspaces of the matrix space $\mathbf{M}_{n\times m}(\mathbb{F}_q)$ given by $\mathcal{C} = \langle \mathbf{C}_1, \ldots, \mathbf{C}_k \rangle_{\mathbb{F}_q}$ and $\mathcal{D} = \langle \mathbf{D}_1, \ldots, \mathbf{D}_k \rangle_{\mathbb{F}_q}$ with $\mathbf{C}_i, \mathbf{D}_i$ matrices of size $n \times m$ with entries in $\mathbb{F}_q$. 
> Find invertible matrices $(\mathbf{A}, \mathbf{B}) \in GL_n \times GL_m$ such that:
> $$ \mathbf{A} \mathbf{C}_i \mathbf{B} \in \mathcal{D} \qquad \text{ for all } 1 \leq i \leq k$$

Using side-channels it can happen, especially in recent optimizations, that the implementation leaks a specific $\mathbf{C} \in \mathcal{C}$ that maps to a specific $\mathbf{D} \in \mathcal{D}$.
In other words $\mathbf{A} \mathbf{C} \mathbf{B} = \mathbf{D}$
This information is then given in terms of coordinate vectors of $\mathbf{C}, \mathbf{D}$ given by $(v_1, \ldots, v_k)$ and $(w_1, \ldots, w_k)$ such that $\mathbf{C} = \sum_i v_i \mathbf{C}_i$ and likewise $\mathbf{D} = \sum_i w_i \mathbf{D}_i$.

## Implementing a solution

In the notebook [MCE.ipynb](./MCE.ipynb) you can find a setup that generates a random MCE problem with hint and a method for testing your solution.
Try to solve the problem by designing the secret $(\mathbf{A}, \mathbf{B})$ as solutions to a system of polynomial equations.
Afterwards, you can either use your FXL algorithm or Sage's built-in [variety()](https://doc.sagemath.org/html/en/reference/polynomial_rings/sage/rings/polynomial/multi_polynomial_ideal.html#sage.rings.polynomial.multi_polynomial_ideal.MPolynomialIdeal_singular_repr.variety).
In case you use your own FXL implementation, the systems are likely not semi-regular, so play around with your $D, k$ FXL parameters to see what works.

Try to see how large you can get $n,m,k$ while still being able to break the problem.
If you reach the limit of your model, try to see if you can alter your model for better results.
For MCE there are a lot of different algebraic models.
Currently, most applications of this problem use $n \approx m \approx k$, but you can play around with different values and see its effect.

Hint: The MCE problem is equivalent to 3-TI. So be sure to check out the 3-TI exercise as well for valuable intuition.

Hint2: The expectation value for the amount of solutions is $q^{n^2 + m^2 - kf(nm-k)}$.
For low $n, m, k$ this can be much bigger than 1.

## Guiding questions

Answering these questions can guide you in designing an initial model and refining it over time.
If you have an idea for a model, try it! 
Implement it, test it, and see what happens.
You can always revisit the questions later.
We don’t expect you to tackle all of them today, focus on what sparks your curiosity and build from there.

Algebraic cryptanalysis is an iterative process: alternating between thinking about the problem and experimenting with new models.
There’s no such thing as a bad model. 
Even if it doesn’t perform well, it will help you build intuition that aid future approaches.

- Look at the problem statement, what are the unknowns. Can we model these as variables in a polynomial system?
- In this problem we have an equality of vector spaces $\mathcal{D}$ and $\langle \mathbf{A} \mathbf{C}_1 \mathbf{B}, \ldots, \mathbf{A} \mathbf{C}_k \mathbf{B} \rangle_{\mathbf{F}_q}$. How can we model this algebraically? Maybe by introducing new variables?
- What can we do with the side channel information? Does it give us new equations? What is the degree of these equations? Quadratic? Can we make linear equations out of these?
- If $(\mathbf{A}, \mathbf{B})$ is a solution, then $(\lambda \mathbf{A}, \lambda^{-1} \mathbf{B})$ is a solution as well for $\lambda \in \mathbb{F}_q^*$. Can we use this to our advantage?
- With high probability a random $\mathbf{C}$ is full rank, can we somehow use $\mathbf{C}^{-1}$?