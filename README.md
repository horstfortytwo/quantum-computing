$$\def\Ket#1{|#1\rangle}
\def\KKet#1{|\!| #1\rangle\!\rangle}
\def\Bra#1{\langle #1|}
\def\Pr#1{\text{Pr}[#1]}
\def\Prv#1#2{\text{Pr}_{#1}[#2]}
\def\RQS#1{\mathbf Q^{#1}}
\def\Bool#1{\{0,1\}^{#1}}
\def\Bo{\{0,1\}}
\def\IP#1#2{\langle #1 | #2\rangle}
\def\IPB#1#2{#1 \odot #2}
\def\Norm#1{||#1||}
\def\RS#1{\mathbf R^{#1}}
\def\CS#1{\mathbf C^{#1}}
\def\E#1{e^{#1 i}}
\def\Mod{\text{ mod }}
\def\Oo#1{\frac 1 {\sqrt{#1}}}$$


Most of the following is taken from Ronald de Wolf, Quantum Computing: Lecture Notes,  [arXiv:1907.09415v5](https://arxiv.org/abs/1907.09415) [quant-ph]. Another source is Sanjeev Arora, Boaz Barak, Computational Complexity, Chapter 10, Cambridge University Press, New York, 2009.

# Intro

**Activity** What do you expect from this course?

**Instruction** Explain what to expect from this course and what not.

![Zach Montague, The Race to Save Our Secrets From the Computers of the Future
Quantum technology could compromise our encryption systems. Can America replace them before it’s too late?, The New York Times, Online version from Oct. 22, 2023, https://www.nytimes.com/2023/10/22/us/politics/quantum-computing-encryption.html](https://estragon.theorie.informatik.uni-kiel.de/uploads/upload_8bfbb80c84e0a13cb3d27b51b340359e.png)

---

# PART I - Algorithms

---

**Instruction** Explain how quantum computation works:
1. Initialize a quantum register with a state determined from a given boolean input.
2. Perform a sequence of quantum state transitions.
3. Measure the quantum state, which results in a boolean output.


# Real Quantum Bits

**Convention** We denote 
- the standard two-dimensional $\mathbf R$ vector space, which consists of column vectors with two rows and entries from $\mathbf R$, by $\RS 2$, 
- the standard inner product on this vector space by $\IP \cdot \cdot$, and 
- the standard basis vectors by $\Ket 0$ and $\Ket 1$. 

##### Instruction
Explain
- how vectors are expressed as linear combinations,
- how the inner product is defined,
- how coefficients of vectors can be determined by the inner product,
- how the norm is defined and which properties it has.

###### Def  {#asfdasfd}
A state of a quantum bit (qubit) is a vector of norm $1$ in $\RS 2$. The set of all such states is denoted $\RQS 1$.

**Rem** We have $$\RQS 1 = \{\alpha_0 \Ket 0 + \alpha_1 \Ket 1 \colon \alpha_0^2 + \alpha_1^2 = 1 \text{ and } \alpha_0, \alpha_1 \in \mathbf R\}\enspace.$$ For a given state $\Ket \varphi = \alpha_0 \Ket 0 + \alpha_1 \Ket 1$, the unique values $\alpha_0$ and $\alpha_1$ are called amplitudes of $\Ket 0$ and $\Ket 1$, respectively. We also set \begin{align}\Ket + & = \Oo 2 (\Ket 0 + \Ket 1) & \Ket - & = \Oo 2 (\Ket 0 -  \Ket 1) \enspace.\end{align}

**Activity** Visualize the set of states of a qubit.
