# Trivium Exercise

In this exercise we will algebraically cryptanalyse the internal state of the Trivium stream cipher. The purpose of stream ciphers is to emulate the functionality of the one-time-pad (OTP). In the OTP encryption scheme, the ciphertext is obtained by combining the plaintext with a keystream using, for instance, the binary XOR operator. The keystream is a random sequence that is at least as long as the plaintext. The greatest challenge in constructing an OTP encryption scheme is the requirement for the keystream to be random and of size at least equal to the size of the plaintext. Since the requirement is not practical, a stream cipher uses a smaller secret key to generate a pseudorandom keystream of the required size.



## Problem statement
Trivium is comprised of three Nonlinear-Feedback Shift Registers (NLFSR). NLFSR is a shift register whose input bit is a non-linear function of its previous state. In these registers are stored 288 bits representing Trivium's internal state, denoted $(s_1,\dots,s_{288})$. This internal state is initialized using an 80-bit secret key vector $K$ and an 80-bit vector IV holding the initial value. The initialization is defined as follows:
$$
    (s_1,\dots,s_{93}) \leftarrow (K_1,\dots,K_{80},0,\dots,0) \\
    (s_{94},\dots,s_{177}) \leftarrow (IV_1,\dots,IV_{80},0,\dots,0) \\
    (s_{178},\dots,s_{288}) \leftarrow (0,\dots,0,1,1,1). 
$$
At the end of each round, all registers are shifted by one bit and the first bit in each register is updated using the defined non-linear feedback function. The initialization process of Trivium consists in loading the state bits into the registers and performing 1155 rounds without producing an output. After this process, every next round produces one output bit obtained as a linear combination of six state bits. The iterative process of Trivium is shown in the algorithm below, where $Z$ denotes the number of generated keystream bits. The pseudo-code for the initialization process is similar, with the exception that line $z_{i} \leftarrow t_{1}+t_{2}+t_{3}$ is missing and $i$ goes from 1 to 1155.

### Trivium's iterative function for keystream generation.
		
> **Input:** The number of bits to be generated, denoted $Z$. \
> **Output:** Keystream vector $z$.
>> for $i=1$ to $Z$
		>>> $t_{1} \leftarrow s_{66}+s_{93}$ \
		>>> $t_{2} \leftarrow s_{162}+s_{177}$ \
		>>> $t_{3} \leftarrow s_{243}+s_{288}$ \
		>>> $z_{i} \leftarrow t_{1}+t_{2}+t_{3}$  \
		>>> $t_{1} \leftarrow t_{1}+s_{91}\cdot s_{92} + s_{171}$ \
		>>> $t_{2} \leftarrow t_{2}+s_{175}\cdot s_{176} + s_{264}$ \
		>>> $t_{3} \leftarrow t_{3}+s_{286}\cdot s_{287} + s_{69}$ \
		>>> $(s_{1},s_{2}\dots,s_{93}) \leftarrow (t_3,s_{1},\dots,s_{92})$ \
		>>> $(s_{94},s_{95}\dots,s_{177}) \leftarrow (t_1,s_{94},\dots,s_{176})$ \
		>>> $(s_{178},s_{179}\dots,s_{288}) \leftarrow (t_2,s_{178},\dots,s_{287})$ 

The goal of this algebraic attack will be to find the internal state of Trivium after a certain number of initialization rounds. We need to focus on a reduced-round version, since an algebraic attack (or any other) can not break Trivium in reasonable time for the purposes of the tutorial (or for invalidating the required level of security). We are working under the assumption that we are able to recover the first $m$ output bits of the keystream vector, for instance because we have encrypted a file with known fixed header data, or we have obtained an encryption on a given message that is known to us (we are dealing with a known-plaintext attack). 

## Implementing a solution
To generate the algebraic model for Trivium, we produce an equation for every output bit, as an output bit is a non-linear combination of internal state bits. Our model contains only 288 variables corresponding to the bits of the internal state after the initialization process is finished.
Hence, this constitutes an attack on the internal state, in contrast to attacks on the key.
 
There are two main ways to model this problem, and the resulting systems differ greatly between these two. In the first modeling, each input bit obtained from the non-linear feedback function is an additional variable, whereas, in the second model, these input bits are recursively replaced by a non-linear combination of state bits from previous rounds, until we obtain a clause containing only bits from the initial state. 

In the notebook [Trivium.ipynb](./Trivium.ipynb) you can first implement a setup that generates a Trivium keystream of a given length after a reduced-round initialization phase. You are also given one pair of of initial state value and the keystream output. Hence, you do not have to build an implementation of Trivium. However, the implementation of the algebraic modeling would be very similar to the implementation of Trivium, so this first step might help you understand the problem. To solve the systems that you modeled, you can either use your FXL algorithm or Sage's built-in [variety()](https://doc.sagemath.org/html/en/reference/polynomial_rings/sage/rings/polynomial/multi_polynomial_ideal.html#sage.rings.polynomial.multi_polynomial_ideal.MPolynomialIdeal_singular_repr.variety). However, for these sizes, we can not run a full algebraic attack on Trivium, so the best solution to test your modeling would be to add a partial solution to your system, which would help with running and trying the resolution.

## Guiding questions
Answering these questions can guide you in designing an initial model and refining it over time.
If you have an idea for a model, try it! 
Implement it, test it, and see what happens.
You can always revisit the questions later.
We don’t expect you to tackle all of them today, focus on what sparks your curiosity and build from there.

Algebraic cryptanalysis is an iterative process: alternating between thinking about the problem and experimenting with new models.
There’s no such thing as a bad model. 
Even if it doesn’t perform well, it will help you build intuition that aid future approaches.

- Let us analyse the second modeling first. How many variables do we have in this modeling? How many equations do we have if we were able to obtain $m$ output bits? What is the degree of the system?

- Now let us answer the same questions for the first modeling. What are the main differences between these two models?  

- Implement both modelings.

- For the first model, what is the highest number of initialization rounds for which you can recover the internal state (in a reasonable time during this tutorial) if you are given $100$ output bits?

- For the second model, what is the highest number of output bits that you can *model* (in a reasonable time during this tutorial)?

