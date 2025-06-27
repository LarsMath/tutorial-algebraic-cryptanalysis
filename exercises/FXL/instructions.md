# FXL Exercise

In this exercise we will finish an XL implementation and test it for random quadratic systems.
If everything works as we hoped, then we will go on to implement the XL-variant FXL, where we fix variables before running XL.
Finally, if time permits, and you are up to an additional challenge, there are some bonus exercises/optimizations to consider.

### Exercise 1: XL
In the notebook [XL.ipynb](./XL.ipynb) you can find the outline of an XL implementation.
Your task is to finish the missing functionality.
Study all the cells in the notebook closely before implementing.
You can use the slides to recap information.

Hint: Look at the methods `generate_Macaulay_matrix(R, fs, D)` and `XL(R, fs, D)` to see at what point in the algorithm your implementation is used.
Then, for inspiration, you can take a look at the function `generate_MQ_problem(R, m)` and the methods that are called therein.

Hint2: Sage has a lot of built-in functionality do not forget to check out the documentation. Especially [multivariate polynomials](https://doc.sagemath.org/html/en/reference/polynomial_rings/sage/rings/polynomial/multi_polynomial.html) is useful.

#### Testing your implementation
In the same notebook you can find a generator for random MQ instances. 
Test your implementation for various $n, m, D$.
With [operating-degree.ipynb](./operating-degree.ipynb) you can estimate the correct operating degree for different $n, m$.
What happens if you take $D \pm 1$?

Instead of random systems we can also consider online challenges where the state-of-the-art algorithms compete to solve the largest instance.
These can be found at [Fukuoka MQ challenge](https://www.mqchallenge.org/).
You can use [fukuoka.ipynb](./fukuoka.ipynb) to transform the challenges into sage polynomials.

### Exercise 2: FXL
Now that you have some experience with Sage and XL we can implement FXL:
```
FXL(R, fs, D, k)
```
Use your existing XL implementation and extend it to do partial guessing.
In this exercise there is no outline yet so think carefully what you will need.

Hint: You might want to change `assert K.dimension() == 1` from here on.

Hint2: To insert your guessed variables you can use `f.subs()` or `R.hom()`. Take care that you do not augment the polynomials by guessed variables.

#### Testing your implementation
To test your algorithm, use [operating-degree.ipynb](./operating-degree.ipynb) with $n-k, m$ to compute the correct operating degree $D$ again.

## Additional exercises

These exercises can help you get more feel for the algorithm and its complexity.
They are not meant to be ordered, so pick any you like.

### Exercise 3: Compute the optimal guessing amount for FXL

### Exercise 4: Use sparse matrices instead

### Exercise 5: Verify the rank predictions of random MQ systems by the Hilbert series using your XL implementation

### Exercise 6: Pre-process the Macaulay matrix

### Exercise 7: Generalize your algorithm to work for arbitrary degree input polynomials