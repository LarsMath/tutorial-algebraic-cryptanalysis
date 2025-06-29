# MQ-Sign Exercise

In this exercise we will algebraically cryptanalyse the MQ-Sign digital signature algorithm. This is a UOV-based cryptosystem with additional structure in the central map that results in more compact private and also public keys. As is often the case, the additional structure can also be exploited from a cryptanalysis point of view. You will be guided to develop a model that allows you to recover the secret key of this cryptosystem in polynomial time. 

## Problem statement
Since MQ-Sign is a UOV-based cryptosystem, it relies on the *Extended Isomorphism of Polynomials with One Secret* problem, which is defined as follows.

> **Input:** An $m$-tuple of multivariate polynomials $p = (p^{(1)} , \dots, p^{(m)} ) \in \mathbb{F}_q[x_1, \dots, x_n]$
and a special class of $m$-tuples of multivariate polynomials $\mathcal{C} \subset \mathbb{F}_q[x_1, \dots, x_n]^m$.

> **Question:** Find - if any - $\mathbf{S} \in \operatorname{GL}_n(\mathbb{F}_q)$, and $f = (f^{(1)} , \dots, f^{(m)} ) \in \mathcal{C}$ such that $p=f \circ \mathbf{S}$.

Note that finding either one of $\mathbf{S}$ or $f$ is enough to recover the other part of the secret. This problem does not have a known polynomial time solution in general, and how hard it is depends on the structure of the special class $\mathcal{C}$. When $\mathcal{C}$ describes a UOV central map, the best algorithms we have to solve this problem are all exponential. In MQ-Sign, we can describe this special class with the following two characteristics. 

- $f$ is a UOV central map. The shape of this map can be described by splitting the variables into vinegar variables $(x_1, \ldots, x_v)$ and oil variables $(x_{v+1}, \ldots, x_{v+o})$ where $o=m=n-v$.
Then the quadratic maps are given as
$$
    f^{(k)} = \sum_{\substack{1 \leq i \leq v \\ i \leq j \leq n}} \alpha^{(k)}_{ij} x_i x_j %+ \sum_i \beta^{(k)}_i x_i + \gamma^{(k)}.
$$
- The central map can be written as $\mathcal{f}^{(k)}=\mathcal{f}_V^{(k)}+\mathcal{f}_{OV}^{(k)}$ where $\mathcal{f}_V^{(k)}=\sum_{i \in V, j \in V} \gamma_{ij}^{(k)} x_i x_j$ and $\mathcal{f}_{OV}^{(k)}=\sum_{i \in V, j \in O} \gamma_{ij}^{(k)} x_i x_j$. These can alternatively be referred to as the vinegar-vinegar quadratic part and the vinegar-oil quadratic part, and they are created as sparse systems, specifically with the following structure
$$
\mathcal{f}_{V}^{(k)}=\sum_{i=1}^{v} \alpha_i^k x_i x_{(i+k-1 (\bmod { v}))+1} \\
\mathcal{f}_{OV}^{(k)}=\sum_{i=1}^{v} \beta_i^k x_i x_{(i+k-2 (\bmod{m}))+v+1}.
$$

Another piece of information we have about the problem statement is that this cryptosystem uses the UOV equivalent keys optimization, according to which the secret linear transformation $\mathbf{S}$ has the following form
$$
S = 
	\begin{pmatrix}	
		I_{v \times v} & S_1  \\
		0_{m \times v} & I_{m \times m}
	\end{pmatrix}.
$$

## Implementing a solution
In the notebook [MQ-Sign.ipynb](./MQ-Sign.ipynb) you can find a setup that generates a random MQ-Sign key pair with the structure described above. If the system you obtain is nonlinear, you can either use your FXL algorithm or Sage's built-in [variety()](https://doc.sagemath.org/html/en/reference/polynomial_rings/sage/rings/polynomial/multi_polynomial_ideal.html#sage.rings.polynomial.multi_polynomial_ideal.MPolynomialIdeal_singular_repr.variety).
For this exercise however, the goal is to obtain a modeling that results in a linear system of equations. Then, you can solve it by building the corresponding matrix $\mathbf{A}$ (where each column holds the coefficients of one variable in all of the polynomials) and target vector $\mathbf{b}$ (this contains the constant part of each polynomial) describing the system, and solve the problem $\mathbf{A}\mathbf{x}=\mathbf{b}$.

## Guiding questions

Answering these questions can guide you in designing an initial model and refining it over time.
If you have an idea for a model, try it! 
Implement it, test it, and see what happens.
You can always revisit the questions later.
We don’t expect you to tackle all of them today, focus on what sparks your curiosity and build from there.

Algebraic cryptanalysis is an iterative process: alternating between thinking about the problem and experimenting with new models.
There’s no such thing as a bad model. 
Even if it doesn’t perform well, it will help you build intuition that aid future approaches.

- Write out the matrix representation of a UOV central map. Then, write the matrix representation of the MQ-Sign central map (for the first few equations).
- Write out the UOV key generation (the relation between $p$ and $f$) in block matrix form, meaning $\mathbf{F}^{(k)}_1$ and $\mathbf{P}^{(k)}_1$ represent the vinegar-vinegar part of the maps, $\mathbf{F}^{(k)}_2$ and $\mathbf{P}^{(k)}_2$ represent the vinegar-oil part and $\mathbf{P}^{(k)}_3$ represents the oil-oil part of the public map (the corresponding block in the central map is the zero matrix).
- Describe and implement a modeling from the constraints obtained by equating the two upper blocks of the matrices.
- Is the system you obtained in the previous question a linear system of equations? If not, try to rewrite your two main constraints in such a way that you obtain a linear system.
- Why are we not able to solve this linear system uniquely? **Hint:** Think about the number of variables and number of equations that we get.
- Up until now, you have modeled this problem in the generic UOV setting. Now think about how the specific structure (sparsness) of the MQ-Sign central map can be used to solve the system.
- **Bonus:** Look deeper into the structure of the system and notice how certain variables appear only in certain equations. Try to use this observation to make the modeling and solving part of your attack as fast as you can. For reference, the paper describing this attack reports 0.6 seconds for
the proposed parameters for security level I, but this is in MAGMA, and the SageMath implementation should be a bit slower.

You can look at the accompanying slides, and ultimately [2023/432](https://eprint.iacr.org/2023/432.pdf), for more hints, but try not to spoil it too soon.