---
title: quantum agenda
---

$\def\Ket#1{|#1\rangle}
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
\def\Oo#1{\frac 1 {\sqrt{#1}}}$


Most of the following is taken from Ronald de Wolf, Quantum Computing: Lecture Notes,  [arXiv:1907.09415v5](https://arxiv.org/abs/1907.09415) [quant-ph]. Another source is Sanjeev Arora, Boaz Barak, Computational Complexity, Chapter 10, Cambridge University Press, New York, 2009.

# Intro

**Activity** What do you expect from this course?%%

**Instruction** Explain what to expect from this course and what not:
- quantum algorithms (Grover, Shor)
- quantum complexity
- no quantum physics, quantum computers, quantum computing technology, ...%%

![Zach Montague, The Race to Save Our Secrets From the Computers of the Future
Quantum technology could compromise our encryption systems. Can America replace them before it’s too late?, The New York Times, Online version from Oct. 22, 2023, https://www.nytimes.com/2023/10/22/us/politics/quantum-computing-encryption.html](https://estragon.theorie.informatik.uni-kiel.de/uploads/upload_8bfbb80c84e0a13cb3d27b51b340359e.png)

---

# PART I --- Algorithms

---

**Instruction** Explain how quantum computing roughly works:
1. Initialize a quantum register with a state determined from a given boolean input.
2. Perform a sequence of quantum operations, which make the initial state evolve.
3. Measure the final quantum state, which results in a boolean output.%%


# Real Quantum Bits

**Convention** We denote 
- the standard two-dimensional $\mathbf R$ vector space, which consists of column vectors with two rows and entries from $\mathbf R$, by $\RS 2$, 
- the standard inner product on this vector space by $\IP \cdot \cdot$, 
- the standard basis vectors by $\Ket 0$ and $\Ket 1$, and
- the norm of a vector denoted $\boldsymbol u$ by $\Norm {\boldsymbol u}$.%%

**Instruction** Explain
- how vectors are expressed as linear combinations,
- how the inner product is defined,
- how coefficients of vectors can be determined by the inner product,
- how the norm is defined and which properties it has.%%

**Def** A state of a quantum bit (qubit) is a vector of norm $1$ in $\RS 2$.%%

**Notation** The so-called Ket-Notation, such as in $\Ket \varphi$, indicates quantum states.

**Rem** The set of states of a qubit is given by $$\{\alpha_0 \Ket 0 + \alpha_1 \Ket 1 \colon \alpha_0^2 + \alpha_1^2 = 1 \text{ and } \alpha_0, \alpha_1 \in \mathbf R\}\enspace.$$

**Def** For a given state $\alpha_0 \Ket 0 + \alpha_1 \Ket 1$, the unique values $\alpha_0$ and $\alpha_1$ are called amplitudes of $\Ket 0$ and $\Ket 1$, respectively.%%

**Def** The states $\Ket +$ and $\Ket -$ are defined by \begin{align}\Ket + & = \Oo 2 (\Ket 0 + \Ket 1) \enspace,\\
\Ket - & = \Oo 2 (\Ket 0 -  \Ket 1) \enspace.\end{align}%%

**Activity** Visualize the set of states of a qubit, in particular, the states $\Ket +$ and $\Ket -$.%%

**Def** With every quantum state $\Ket \varphi = \alpha_0 \Ket 0 + \alpha_1 \Ket 1$ a probability distribution on $\{0,1\}$ is associated: \begin{align}\Prv {\Ket \varphi} 0 & = \alpha_0^2 \enspace,\\ \Prv {\Ket \varphi} 1 & = \alpha_1^2\enspace.\end{align} The operation of measuring a quantum state $\Ket \varphi$ returns $0$ or $1$, with a probability according to the above distribution.%%

**Activity** What are the probability distributions associated with
- $\Ket 0$, 
- $\Ket 1$, 
- $\Ket +$, 
- $\Ket -$, and
- $\sin \alpha \: \Ket 0 + \cos \alpha \: \Ket 1$?%%

**Def** A quantum state transformation for qubits is an orthogonal transformation on $\RS 2$.%%

**Math input** [The]{ortho} following are equivalent for a linear transformation $T$:
- $T$ is orthogonal.
- $\IP u v = \IP {T(u)}{T(v)}$ for all $u, v \in \RS 2$.
- $\Norm {T(u)} = 1$ for all $u \in \RS 2$ with $\Norm u = 1$.
- The image of an orthonormal basis under $T$ is an orthonormal basis.%%

**Cor** If $\Ket \varphi$ is a state of a qubit and $T$ is a quantum state transformation for a qubit, then $T(\Ket \varphi)$ is a state of a quantum bit.%%

**Activity** Try to find as many quantum state transitions for a qubit as you can. Search the web if you feel like.%%

**Math input** Every quantum state transition on a qubit is either a rotation or a reflection.

![](https://estragon.theorie.informatik.uni-kiel.de/uploads/upload_d4bbca44695aac1d9b090d849f3987c1.png)

The corresponding matrices are
$$\begin{bmatrix}\cos \alpha & \sin \alpha \\ - \sin \alpha & \cos \alpha \end{bmatrix} \qquad \text{ and } \qquad \begin{bmatrix}\cos \alpha & \sin \alpha\\ \sin \alpha & - \cos \alpha \end{bmatrix}\enspace,$$ where $0 \leq \alpha < 2\pi$.%%

**Problem** 
1. Determine the matrix that corresponds to a reflection at a line with $\beta = 0$ followed by a reflection at a line with $\beta = \alpha/2$. Simplify this matrix as much as you can.%%
2. Determine the matrix that corresponds two the composition of two reflections at lines at given angles $\beta$ and $\beta'$.

**Def** The state transitions $I$, $X$, $Z$, and $H$ are defined by matrices:
\begin{align}
I & = \begin{bmatrix}1 & 0\\ 0 & 1 \end{bmatrix} \qquad \text{identity gate}\enspace,\\
X & = \begin{bmatrix}0 & 1\\ 1 & 0 \end{bmatrix} \qquad \text{bit-flip gate}\enspace,\\
Z & = \begin{bmatrix}1 & 0\\ 0 & - 1 \end{bmatrix} \qquad \text{phase-flip gate}\enspace,\\
H & = \frac 1 {\sqrt 2} \begin{bmatrix} 1 & 1\\ 1 & - 1\end{bmatrix} \qquad \text{Hadamard gate/transform}\enspace.
\end{align}%%

**Problem** Express each of the above as a rotation or reflection.%%

**Problem** Is the reflection through the origin a quantum state transformation?---Reason.

**Problem**
1. Show that every quantum state transformation for a qubit is invertible. 
1. Show that the composition of two quantum state transformations for a qubit is a quantum state transformation.
1. Consider the [enumeration of alternative definition of orthogonal transformation](#ortho). Can the last two conditions be combined as follows?---Reason. $\Norm{T(u)} = 1$ for every vector $u$ of an orthonormal basis.

**QAlg** CoinFlip()
1. Initialize a qubit with $\Ket 0$.
2. Apply the Hadamard transform $H$.
3. Measure the state.%%

**Activity** Determine the probability that CoinFlip() outputs $0$.%%

**Instruction** Show a solution with assertions.

**Problem** Let $p \in [0,1]$. Adapt CoinFlip() in such a way that it outputs $0$ with probability $p$ (and $1$ with probability $1-p$.)

**Outcome** 

# Real Quantum Registers

**Convention** We denote 
- the standard $2^n$-dimensional $\mathbf R$ vector space, which consists of column vectors with $2^n$ rows and entries from $\mathbf R$, by $\RS {2^n}$, 
- the standard inner product on this vector space by $\IP \cdot \cdot$, and 
- the standard basis vectors by $\Ket {0 \dots 0}$ and $\Ket {1 \dots 1}$ (in lexicographic ordering). 

**Instruction**
Explain
- how vectors are expressed as linear combinations,
- how the inner product is defined,
- how coefficients of vectors can be determined by the inner product,
- how the norm is defined and which properties it has,
- how base vectors are denoted by integers.

**Def** A state of a $n$ bit quantum register ($n$-qureg) is a vector of norm $1$ in $\RS {2^n}$. The set of all such states is denoted $\RQS n$.

**Rem** We have \begin{align}\RQS n = \{\sum_{b \in \{0,1\}^n} \alpha_b \Ket b \colon \sum_{b \in \{0,1\}^n} \alpha_b^2 = 1 \text{ and } \alpha_{b_0 \dots b_{n-1}} \in \mathbf R\}\end{align} or \begin{align}\RQS n = \{\sum_{i < 2^n} \alpha_i \Ket i \colon \sum_{i < 2^n} \alpha_i^2 = 1 \text{ and } \alpha_i \in \mathbf R\}\end{align} For a state given as above, the unique values $\alpha_b$ are called amplitudes of the $\Ket b$s.

**Activity** Come up with a definition of the uniform state and visualize it for $n=1$ (qubit).

**Def** For every $n$, the uniform state $\Ket {{+^n}} \in \RQS n$ is defined by $$\Ket  {{+^n}} = \Oo {2^n} \sum_{i < 2^n} \Ket i \enspace.$$

**Def** With every quantum state $\Ket \varphi = \sum_{b_0 \dots b_{n-1}} \alpha_{b_0 \dots b_{n-1}} \Ket{b_0 \dots b_{n-1}}$ a probability distribution on $\{0,1\}$ is associated: $$\Prv {\Ket \varphi} {b_0 \dots b_{n-1}} = \alpha_{b_0 \dots b_{n-1}}^2 \enspace.$$ The operation of measuring a quantum state $\Ket \varphi$ returns $b_0 \dots b_{n-1}$, with a probability according to the above distribution.

**Activity** What are the probability distributions associated with $\frac 1 {\sqrt 2}(\Ket{00} + \Ket{11})$, the so-called EPR state, denoted also $\Ket{\text{EPR}}$?

**Def** A quantum state transition $T$ on an $n$-bit qureg is an orthogonal transformation on $\RS {2^n}$. 

**Cor** $T(\Ket \varphi) \in \RQS n$ for every quantum state transition $T$ on $n$ qubits.

**Exm** For every permutation $\pi \colon \Bool n \to \Bool n$, there is a unique quantum state transition such that $$\Ket {b_0 \dots b_{n-1}} \mapsto \Ket {\pi(b_0 \dots b_{n-1})}$$ for all $b_0 \dots b_{n-1} \in \Bool n$.

**Problem** Express $X$ as in the previous example.

**Def** The (tensor) product of $u \in \RS {2^m}$ and $v \in \RS {2^n}$ is denoted by $u \otimes v$. It is (uniquely) defined by the following rules:
- [Basis vectors] $$\Ket{b_0 \dots b_{m-1}} \otimes \Ket{c_0 \dots c_{n-1}} = \Ket{b_0 \dots b_{m-1} c_0 \dots c_{n-1}} \enspace.$$
- [Bilinearity] \begin{align}(\alpha u) \otimes v & = \alpha (u \otimes v) & u \otimes (\beta v) & = \beta (u \otimes v)  \\ (u + u') \otimes v & = (u \otimes v) + (u' \otimes v) & u \otimes (v + v') & = (u \otimes v) + (u \otimes v')\end{align} for $\alpha, \beta \in \mathbf R$, $u, u' \in \RQS m$, $v, v' \in \RQS n$. 

**Problem** Show properties of $\otimes$.

**Remark** For all $\alpha_i^b \in R$ with $i < n$ and $b < 2$, we have
\begin{align}
\left(\alpha_0^0 \Ket 0 + \alpha_0^1 \Ket 1\right) \otimes
\left(\alpha_1^0 \Ket 0 + \alpha_1^1 \Ket 1\right) \otimes \dots \otimes
\left(\alpha_{n-1}^0 \Ket 0 + \alpha_{n-1}^1 \Ket 1\right) = 
\sum_{b_0 \dots b_{n-1}} \alpha_0^{b_0} \alpha_1^{b_1} \dots \alpha_{n-1}^{b_{n-1}} \Ket {b_0 \dots b_{n-1}}
\end{align}

**Lem** For all $\Ket \varphi \in \RQS m$ and $\Ket \psi \in \RQS n$, $\Ket \varphi \otimes \Ket \psi \in \RQS{m+n}$.

**Instruction** Prove this.

**Activity**
- Determine $\Ket{0} \otimes \Ket{1}$.
- Determine $\Ket + \otimes \Ket +$.
- Determine $\Ket + \otimes \Ket + \otimes \Ket +$.
- Find a 2-bit quantum state which is not the product of two 1-bit quantum states. Such states are called entangled.

**Notation** For states $\Ket \varphi$ and $\Ket \psi$, we also write $\Ket \varphi \Ket \psi$ instead of $\Ket \varphi \otimes \Ket \psi$.

**Def** 
1. When $S$ and $T$ are linear transformations on $\RS {2^m}$ and $\RS {2^n}$, respectively, then the linear transformation $S \otimes T$ on $\RS {2^{m+n}}$ is defined by $$(S \otimes T)\Ket {b_0 \dots b_{m-1}c_{0} \dots c_{n-1}} = S\Ket {b_0 \dots b_{m-1}} \otimes T\Ket{c_0 \dots c_{n-1}} \enspace.$$ 
2. Iterated products are denoted by $T^{\otimes n}$.

**Activity**  
- Determine $H^{\otimes n}\Ket {0 \dots 0}$.
- Determine $H^{\otimes n}\Ket i$ for every $i < 2^n$.

**Lem** For every $i < 2^n$, $$H^{\otimes n}\Ket i = \Oo {2^n} \sum_{j < 2^n} (-1)^{\IPB i j} \Ket j\enspace,$$ where $i$ and $j$ in the exponent are interpreted as bit vectors of length $n$.

**Proof** By definition 
\begin{align}
H^{\otimes n}\Ket {c_0 \dots c_{n-1}} 
& = \left(\Ket 0 + (-1)^{c_0} \Ket 1\right) \otimes \left(\Ket 0 + (-1)^{c_1} \Ket 1 \right) \otimes \dots \otimes \left(\Ket 0 + (-1)^{c_{n-1}} \Ket 1\right) 
\end{align}
So $\alpha_i^0 = 1$ and $\alpha_i^1 = (-1)^{b_i}$, or $\alpha_i^{b_i} = (-1)^{b_i c_i}$ in the above remark. Thus,
\begin{align}
H^{\otimes n}\Ket {c_0 \dots c_{n-1}} 
& = \sum_{b_0 \dots b_{n-1}} (-1)^{b_0c_0}(-1)^{b_1c_1}\dots (-1)^{b_{n-1}c_{n-1}} \Ket {b_0 \dots b_{n-1}}\\
& = \sum_{b_0 \dots b_{n-1}} (-1)^{\sum_{i<n} b_ic_i} \Ket {b_0 \dots b_{n-1}}\\
& = \sum_{b_0 \dots b_{n-1}} (-1)^{\sum_{i<n} b_ic_i \text{ mod }2} \Ket {b_0 \dots b_{n-1}}\\
& = \sum_{b_0 \dots b_{n-1}} (-1)^{\sum_{i<n} (b_ic_i \text{ mod }2) \text{ mod }2} \Ket {b_0 \dots b_{n-1}}\\
& = \sum_{b_0 \dots b_{n-1}} (-1)^{\IPB{(b_0, \dots, b_{n-1})}{(c_0, \dots, c_n)}} \Ket {b_0 \dots b_{n-1}}
\end{align} where $\IPB \cdot \cdot$ denotes the inner product on $\mathbf Z_2$.


# Deutsch-Josza and Bernstein-Vazirani Algorithms

**Def** Given $n >0$, a bit string $x = x_0 \dots x_{2^n-1}$ is called 
- balanced if the number of $i$s with $x_i = 0$ is the same as the number of $i$s with $x_i = 1$ (which is then equal to $2^{n-1}$),
- constant if all the $x_i$s are the same.

**Def** The Deutsch-Josza problem is: given a balanced or constant $x$, determine wether $x$ is balanced (output $1$) or not (output $0$). 

**Acitivity**
- Design a deterministic algorithm that solves the Deutsch-Josza problem.
- Design a probabilistic algorith that solves the Deutsch-Josza problem with probability $> 3/4$.

**Remark** 
- Every deterministic algorithm needs $2^{n-1}+1$ steps in the worst case.
- There is a probabilistic algorithm which inspects three bits of $x$ and success probability $> 3/4$.

**Def** Given $x$ as above, the oracle for $x$, denoted $O_x$, is the $n+1$ bit quantum state transition defined by $$\Ket {b_0 \dots b_{n-1}c} \mapsto \Ket {b_0 \dots b_{n-1} (c \oplus x_{b_0 \dots b_{n-1}})}$$ or
$$\Ket {i}\Ket c \mapsto \Ket i \Ket{c \oplus x_i}\enspace.$$

**Activity** Prove that $O_x$ is a quantum state transition.

**Remark** 
1. If $x_i = 0$, then $O_x(\Ket i \Ket -) = \Ket i \Ket -$.
2. If $x_i = 1$, then $O_x(\Ket i \Ket -) = \Ket i \otimes \Oo 2 (\Ket 1 - \Ket 0) = (-1) \Ket i \otimes \Oo 2 (\Ket 0 - \Ket 1) = (-1) \Ket i \Ket-$.
3. $O_x(\Ket i \Ket -) = (-1)^{x_i}\Ket i \Ket -$.

**Def** The quantum Deutsch-Josza problem is: Given $O_x$ for $x$ balanced or constant, determine whether $x$ is constant (output $1$) or not (output $0$).

**Alg** DeutschJosza$(O_x)$
1. Initialize with $\Ket {0^n1}$.
1. Apply $H^{\otimes (n+1)}$.
3. Apply $O_x$.
4. Apply $H^{\otimes (n+1)}$.
5. Measure $b_0 \dots b_{n-1}c$.
6. Output $b_0 \vee \dots \vee b_{n-1}$. 

**Instruction** Add assertions and thus prove correctness.

**Alg** DeutschJosza$(O_x)$
1. Initialize with $\Ket {0^n1}$.
  $\Ket {0^n1}$
1. Apply $H^{\otimes (n+1)}$.
  $\Ket {+^n}\Ket -$ 
3. Apply $O_x$.
  $O_x\left(\Oo {2^{n}}\displaystyle \sum_{i < 2^n} \Ket i \Ket -\right) = \Oo {2^{n}}\displaystyle \sum_{i < 2^n} (-1)^{x_i} \Ket i \Ket -$ 
4. Apply $H^{\otimes (n+1)}$.
  $H^{\otimes (n+1)}\left(\Oo {2^{n}}\displaystyle \sum_{i < 2^n} (-1)^{x_i} \Ket i \Ket -\right)$ 
  $= \Oo {2^{n}}\displaystyle \sum_{i < 2^n} (-1)^{x_i} \Oo {2^n} \sum_{j < 2^n} (-1)^{\IPB i j}\Ket j \Ket 1$ 
  $\displaystyle = \frac 1 {2^n} \sum_{i,j}(-1)^{x_i}(-1)^{\IPB i j}\Ket j\Ket 1$
  $\displaystyle = \frac 1 {2^n} \sum_{j< 2^n} \left(\sum_{i < n} (-1)^{x_i}(-1)^{\IPB i j}\right)\Ket j\Ket 1$
  The amplitude of $\Ket 0$ is $\displaystyle \frac 1 {2^n} \sum_{i < 2^n} (-1)^{x_i}$, which is 
    - $1$ if $x = 1^n$,
    - $-1$ if $x = 0^n$, and
    - $0$ if $x$ is balanced. 
5. Measure $b_0 \dots b_{n-1}c$.
6. Output $b_0 \vee \dots \vee b_{n-1}$. 

**Problem\*** Given $n > 0$ and a bit string $a$ of length $n$, the bit string $x^a = x_0 \dots x_{2^n-1}$ is determined by $x_i = \IPB i a$. The plain Bernstein-Vazirani problem is: given $x^a$ for some $a$, determine $a$. (This is well-defined.) Show that one only needs to change step 6 in DeutschJosza to solve the plain Bernstein-Vazirani problem.

# Grover's Algorithm

**Instruction** Explain what NP-problems look like; remind of SAT and search problems in general.

**Def** A boolean function $f \colon \Bool n \to \Bo$ is unique if there is exactly one $a \in \Bool n$ such that $f(a) = 1$. This value $a$ is called *the* solution of $f$.

**Def** Grover's problem is: Given a unique boolen function, find its solution.

**Remark** In the worst case, $2^n$ evaluations of $f$ are necessary. Randomization does not help.

**Def** Grover's quantum problem is: Given $O_f$ for a unique boolean function, find the solution of $f$.

**Instruction** Explain the double reflection principle.

![](https://estragon.theorie.informatik.uni-kiel.de/uploads/upload_674885c7cfd1f8fb8d959faf3b1e259c.png)

**Instruction** Explain main idea.

![](https://estragon.theorie.informatik.uni-kiel.de/uploads/upload_1630782b7a7c0ec41854f71a10882586.png)

\begin{align}
\Ket u & = \Oo {2^n - 1} \sum_{b \in \Bool n - a} \Ket b\\
\Ket v & = \Oo {2^n} \sum_{b \in \Bool n} \Ket b\\
\theta & = \arccos(\cos(\Ket u, \Ket v)) = \arccos \sqrt{1- \frac 1 {2^n}}
\end{align}

**AlgSketch** Grover
// The function $f \Bool n \to \Bo$ is unique.
1. // Determine number of rounds needed.
   // The angle after $r$ rounds is $(2r+1) \theta$ and we want to get close to $\frac \pi 2$.
   Let $r = \lfloor \frac \pi {4 \theta}-\frac 1 2 \rceil$.
   // Then $\cos(\Ket a, \Ket {x_r}) \geq \cos \theta = \sqrt{1- \frac 1 {2^n}}$, which implies that $a$ is measured with probability $\geq 1 - \frac 1 {2^n}$.
1. Inititalize with $\Ket v$ and denote this state by $\Ket {x_0}$.
1. For $i = 0$ to $r-1$:
   Reflect $\Ket{x_i}$ first around $\Ket u$ and then around $\Ket v$ to obtain state $\Ket{x_{i+1}}$.
1. Measure $b_0 \dots b_{n-1}$.
1. If $f(b) = 1$, then output $b$ else output "fail".
   // "fail" is output with probability $< \frac 1 {2^n}$.
   
**Remark** Since $x \geq \sin x$ for $x \in [0, \pi]$ and $\sin \theta = \Oo {2^n}$ (because $\sin^2 \theta + \cos^2 \theta = 1$), we have $\theta \geq \Oo {2^n}$, which implies $r \in O(\sqrt{2^n})$.

**Math input** Reflection through hyperplane $v^\perp$ for a non-zero vector $v$.

**Math input** Reflection around a non-zero vector $v$.

**Lemma** 
1. $O_f (I^{\otimes n} \otimes Z) O_f\Ket {x0} = \Ket{x0}$ for $x \in \Bool n - a$.
2. $O_f (I^{\otimes n} \otimes Z) O_f \Ket {a0} = - \Ket {a0}$.

So $U = O_f (I^{\otimes n} \otimes Z) O_f$ is a reflection through $\Ket a^\perp$ and restricted to the $\Ket {x_i}$s a reflection around $\Ket v$.

**Lemma** 
1. A reflection $V$ around $\Ket v = \Ket {+^n}$ is given by
\begin{align}
  V(H^{\otimes n} \Ket {0^n}) & = H^{\otimes n} \Ket {0^n}\\
  V(H^{\otimes n} \Ket {b}) & = -H^{\otimes n} \Ket {b} && \text{ for $b \in \Bool n - 0^n$}
\end{align}
1. Let $g \colon \Bool n \to \Bo$ be defined by $f(0^n) = 0$ and $f(b) = 1$ for $b \in \Bool n - 0^n$. Then $$(H^{\otimes n} \otimes I) O_g (H^{\otimes n} \otimes I) \Ket {b0} = V\Ket b \Ket 0$$ for all $b \in \Bool n$.

**Alg** Grover$(O_f)$ 
// The function $f \Bool n \to \Bo$ is unique.
1. // Determine number of rounds needed.
   Let $r = \lfloor \frac \pi {4 \theta}-\frac 1 2 \rceil$.
1. Inititalize with $\Ket{0^{n+1}}$.
1. For $i = 0$ to $r-1$:
   Apply $O_f (I^{\otimes n} \otimes Z) O_f$ and then $(H^{\otimes n} \otimes I) O_g (H^{\otimes n} \otimes I)$.
1. Measure $b_0 \dots b_{n-1}$.
1. If $f(b) = 1$, then output $b$ else output "fail".
   // "fail" is output with probability $< \frac 1 {2^n}$.
   
**Theorem** Grover$(O_f)$ outputs the solution to $f$ with probability $\geq 1 - \frac 1 {2^n}$ after $O(\sqrt{2^n})$ queries to $O_f$.

# Simon's Algorithm

**Def** A boolean function $f \colon \Bool n \to \Bool n$ is periodic if there exists some $a \in \Bool n - 0^n$ such that $f(x) = f(x')$ if and only if $x' = x$ or $x' = x \oplus a$. The $a$ is unique and it is called the period of $f$.

**Def** Simon's problem is: Given a periodic boolean function, determine its period.

**Def** Simon's quantum problem is: Given $O_f$ for a periodic boolean function $f$, determine $a$.

**AlgSketch** Simon
1. Run a quantum algorithm to produce a random $y$ with $\IPB y a = 0$ several times, say $y_0, \dots, y_{r-1}$ are the values produced.
2. Solve the following linear system of equations for $a$. $$\IPB {y_i}  a = 0 \qquad \text{ for $i< r$}$$
3. If there is a unique solution $b \neq 0^n$, output $b$, else output "fail".

**Questions**
- What is the quantum algorithm?
- How large must $k$ be so as to obtain a unique solution with a reasonable probability?

**Def** Let $\Ket x = \sum_{b \in \Bool k} \alpha_b \Ket b$ be an $n$-bit quantum state and $i_0, \dots, i_{k-1}$ and $j_0, \dots, j_{l-1}$ two sequences of indices partitioning $0, \dots, n-1$ (which implies $k + l = n$). The following probability distribution on $\Bool k$ is assoicated with $\Ket x$:
$$\Pr {b_{i_0} \dots b_{i_{k-1}}} = \sum_{b_{j_0} \dots b_{j_{l-1}}} \alpha_b^2 \enspace.$$
The operations of measuring the bits $i_0, \dots, i_{k-1}$ of a quantum state $\Ket x$ returns $b_{i_0} \dots b_{i_{k-1}}$ with probability $\Pr {b_{i_0} \dots b_{i_{k-1}}}$ and if $b_{i_0} \dots b_{i_{k-1}}$ is returned, then the remaining state is $$\frac 1 {\Norm v} v$$ where $$v = \sum_{b_{j_0} \dots b_{j_{l-1}}} \alpha_{b_{j_0} \dots b_{j_{l-1}}} \Ket{b_{j_0} \dots b_{j_{l-1}}} \enspace.$$

**Activity** 
- Determine the possible outcomes and remaining states when measurung bit $1$ of $\Ket {\text{EPR}}$.
- Determine the possible outcomes and remaining states when measuring $\Ket x \otimes \Ket y$.

**Alg** SimonIteration$(O_f)$
// The boolean function $f \colon \Bool n \to \Bool n$ is periodic with period $a$.
1. Initialize with $\Ket 0^{2n}$.
  State: $\Ket {0^{2n}}$.
1. Apply $H^{\otimes n}\otimes I^n$. (So $H$ on the first $n$ bits.)
  State: $\Ket{+^n}\Ket{0^n}$.
1. Apply $O_f$.
  State: $\displaystyle \Oo {2^n} \sum_{b \in \Bool n} \Ket b \Ket {f(b)} = \Oo {2^{n+1}} \displaystyle\sum_{b \in \Bool n} (\Ket b + \Ket{b\oplus a}) \Ket {f(b)}$.
1. Measure bits $n, \dots, 2n-1$.
  Output with equal probability: $d \in \text{Im}(f)$, say, $d = f(c)$.
  State: $\Oo 2(\Ket c + \Ket{c \oplus a})$.
5. Apply $H^{\otimes n}$.
  State: $\displaystyle \Oo 2 \left(\Oo {2^n}\sum_{b \in \Bool n} (-1)^{\IPB b c } \Ket b + \Oo {2^n} \sum_{b \in \Bool n} (-1)^{\IPB {b} {(c \oplus a)} } \Ket b\right)$ 
  $= \displaystyle \Oo {2^{n+1}} \sum_{b \in \Bool n} \left((-1)^{\IPB b c } + (-1)^{\IPB {b} {(c \oplus a)} }\right) \Ket b$ 
  $= \displaystyle \Oo {2^{n+1}} \sum_{b \in \Bool n} \left((-1)^{\IPB b c } + (-1)^{\IPB {b} {c}} (-1)^{\IPB {b} {a}  }\right) \Ket b$
  $= \displaystyle \Oo {2^{n+1}} \sum_{b \in \Bool n} \left((1 + (-1)^{\IPB {b} {a}  }) (-1)^{\IPB b c } \right) \Ket b$
  $= \displaystyle \Oo {2^{n-1}} \sum_{\IPB b a = 0} (-1)^{\IPB b c } \Ket b$
6. Measure $b$ and output $b$.

// All $b$ with $\IPB b a = 0$ are output with equal probability.

**Lemma** For every $n \geq 1$, the probability that $n$ vectors chosen uniformly and independent from $(\mathbf Z_2)^n$ are linear independent is $\geq \frac 1 4$.

**Instruction** Prove lemma.

**Alg** Simon$(O_f)$
// The boolean function $f \colon \Bool n \to \Bool n$ is periodic with period $a$.
1. For $i = 0$ to $n-2$: Run SimonIteration$(O_f)$ with output $y_i$.
1. Solve the following linear system of equations  for $a$. $$\IPB {y_i}  a = 0 \qquad \text{ for $i< n-1$}$$
1. If there is a unique solution $b \neq 0^n$, output $b$, else "fail".

**Theorem** The success probability of Simon is $\frac 1 4$.

# Complex Quantum Bits and Registers

**Instruction** Explain that in general quantum bits and registers are vectors in $\CS{2^n}$.

**Math Input** Complex numbers, $\mathbf C$ vector spaces, inner product, unitary transformations.

**Def** Complex quantum bits and registers are defined just as their real counterparts, only that $\mathbf R$ vector spaces are replaced by $\mathbf C$ vector spaces. The same applies to quantum state transitions: orthogonal transformations are replaced by unitary transformations.

**Instruction** Explain that we can not easily visualize a state of a complex quantum bit.

**Def** The state transitions $Y$, $P_\theta$, $S$, and $T$ are defined by matrices:
\begin{align}
Y & = \begin{bmatrix}0 & -i\\ i & 0 \end{bmatrix} \enspace,\\
P_\theta & = \begin{bmatrix}1 & 0\\ 0 & e^{\theta i} \end{bmatrix} \qquad \text{// phase shift gates}\enspace,\\
S & = \begin{bmatrix}1 & 0\\ 0 & i \end{bmatrix} = P_{\frac \pi 2} \enspace,\\
T & = \begin{bmatrix} 1 & 0\\ 0 & e^{\frac \pi 4 i}\end{bmatrix} = P_{\frac \pi 4}\enspace.
\end{align}

**Math Input** A complex number $\omega$ is called a root of unity if $\omega^N = 1$ for some $N > 0$. It is then called an $N$th root of unity. The smallest number $j > 0$ such that $\omega^j = 1$ is called the order of $\omega$. If $N$ is the order of $\omega$, then $\omega$ is called a primitive $N$th root of unity. 

**Instruction** Explain the following lemma.

**Lemma** Let $N > 0$. 
1. The number of $N$th roots of unity is $N$.
1. The number $\E {\frac {2\pi} N}$ is a primitive $N$th root of unity.
1. If $\omega$ is an $N$th root of unity, then $\omega^j = \omega^{j \text{ mod }N}$ for every $j \in \mathbf Z$.
1. If $\omega$ is a primitive $N$th root of unity, then $\omega^j = \omega^{j'}$ iff $j \Mod N = j' \Mod N$ for all $j,j' \in  \mathbf Z$.
1. If $\omega$ is an $N$th root of unity $\neq 1$, then $\displaystyle \sum_{j < N} \omega^j = 0$.
1. If $\omega$ is a primitive $N$th root of unity, then, for every $j \in \mathbf Z$, the number $\omega^j$ is a primitive $\frac N{\text{gcd}(N,j)}$th root of unity.

**Instruction** Explain that we need one more fact about complex number with norm $1$.

**Lemma** Let $\theta$ and $K \geq 3$ be such that $0 \leq s \theta \leq \pi$ for all $s < K$. Then $$\left|\sum_{s < K} e^{s\theta i}\right| \geq \frac K {3 \sqrt 2}\enspace.$$

**Proof** Let $a,b \in \mathbf R$ such that $a + bl = \displaystyle \sum_{s < K} e^{s\theta i}$. It is enough to show that $a \geq \frac K {3 \sqrt 2}$ or $b \geq \frac K {3 \sqrt 2}$. 

![](https://estragon.theorie.informatik.uni-kiel.de/uploads/upload_6c7b1c5267cdac0870a18a4cb7b51b28.png)

First case, $K > 1$ and $(K-1) \theta \leq \pi/2$. Then 
\begin{align}
a & = \sum_{s < K} \cos(s\theta) && \qquad \text{definition of $a$}\\
& \geq \sum_{s\theta \leq \pi/2} \cos(s \theta) && \qquad \cos(s\theta) \geq 0 \text{ for each }s\\
& \geq \sum_{s\theta \leq \pi/2} \frac 1 {\sqrt 2} && \qquad \cos(\alpha) \geq \frac 1 {\sqrt 2} \text{ for } 0 \leq \alpha \leq \frac 1 4 \pi\\
& \geq \frac K {2 \sqrt 2} && \qquad (K-1)\theta \leq \frac \pi 2 \enspace.
\end{align}

Second case, $K < 1$ and $(K-1) \theta \geq \pi/2$. Then 
\begin{align}
b & = \sum_{s < K} \sin(s\theta) && \qquad \text{definition of $b$}\\
& \geq \sum_{\frac 1 4 \pi \leq s \theta \leq \frac 3 4 \pi} \sin(s \theta) && \qquad \sin(s\theta) \geq 0 \text{ for each }s\\
& \geq \sum_{\frac 1 4 \pi \leq s \theta \leq \frac 3 4 \pi} \frac 1 {\sqrt 2} && \qquad \sin(\alpha) \geq \frac 1 {\sqrt 2} \text{ for } \frac 1 4 \pi \leq \alpha \leq \frac 3 4 \pi\\
& \geq \frac 1 {3\sqrt 2} && \qquad (K-1)\theta \geq \frac \pi 2 \enspace.
\end{align}


**Def** Let $N = 2^n$. The Fourier transform $F_n$ is the linear transformation given by 
\begin{align}
F_n \colon \Ket j & \mapsto \Oo {N} \sum_{k < N}\omega^{jk} \Ket k\\
& = \Oo{N}\sum_{j < N}\omega^{jk \text{ mod }N} \Ket k\\
& = \Oo N \begin{bmatrix} 1 & \omega^j & (\omega^j)^2 & (\omega^j)^3 & \dots & (\omega^j)^{N-1}\end{bmatrix}^T
\end{align} for every $j < N$ and where $\omega = \E {\frac{2 \pi} {N}}$ (or any other primitive $N$th root of unity).

**Instruction** 
- Mention: If $F_n \Ket j = \displaystyle \sum_{k<N} \alpha_k \Ket k$, then $$\alpha_{k \Mod N} = \alpha_{k' \Mod N} \qquad \text{iff} \qquad r \mid k' - k$$ for $r = N/\text{gcd}(N,j)$ and all $k,k' \in \mathbf Z$.
- Prove that $F_n$ is unitary.
- Compare with $H^{\otimes n}$.

# Factoring -- Discrete Part

**Problem** Primality: Given a positive integer, determine whether it is prime.

**(Highly non-trivial) Fact** There is a deterministic polynomial-time primality test.

**Instruction** Explain that the problem size is $\log n$.

**Problem** Factoring: Given a positive composite number, determine one of its non-trivial factors. A number $n$ is composite if $n > 2$ and there exists a number $a$ with $1 < a < n$ such that there exists a number $b$ with $ab = n$. Such numbers $a$ (and $b$) are called non-trivial factors.

**Fact** No polynomial-time factoring algorithm is known, not even a probabilistic one.

**Math Input** Every positive integer $n$ can be written as a product $\Pi_{i < k} p_i$ where every $p_i$ is a prime number. The multiset $\{p_i \mid i < k\}$ is unique and called the prime factorization of $n$ and $k \leq \log n$.

**Activity** Assume you are given an efficient factoring algorithm. Use this to obtain an efficient algorithm that determines the prime factorization of every non-negative integer.

**Alg** PrimeFactorization$(n)$
// $n$ is a non-negative integer
Let $L$ be the list with $n$ as its single element. // These numbers are factors of $n$ still to be factored into primes.
Let $F$ be the empty list. // These are the prime factors already determined.
If $n = 1$ then return $F$ (the empty list).
while $L$ is non-empty
 Remove the first element from $L$, say $m$.
 If $m$ is prime, then
  Add $m$ to $F$.
 else
  Determine a non-trivial factor $a$ of $m$.
  Add $a$ and $m/a$ to $L$.
return $F$

**Remark** An invariant of this algorithm is:
- The product of the elements of $L$ and $F$ is $n$.
- Every element of $L$ and $F$ is $\geq 2$, in particular, the sum of the length of $L$ and $F$ is at most $\log n$.

**Instruction** Explain polynomial running time.

Each run of the algorithm can be identified with a binary tree where the root is labelled $n$, each leaf is labelled with a prime, and the product of the labels of the children of a node is the label of its parent with both labels being non-trival factors. The number of iterations of the while loop corresponds exactly to the number of vertices. The number of leaves is the size of the prime factorization, that is, the number of leaves is at most $\log n$. This means the number of nodes is at most $2 \log n$ and therefore the number of iterations of the while loop is at most $2 \log n$.

![](https://estragon.theorie.informatik.uni-kiel.de/uploads/upload_eb14f052fd614c780fd2378a452ec32d.png)

**Instruction** Explain that for finding a non-trivial factor, easy cases are dealt with first.

**Fact** There is a deterministic polynomial-time algorithm that checks whether a composite number $n$ is of the form $p^m$ for some prime $p$ and $m > 1$ (and outputs $p$ and $m$).

**Alg** PrimePower($p$)
for $m = 2$ to $\lfloor \log n\rfloor$
 Deterime $m' = \lfloor \sqrt[m] n\rfloor^m$.
 if $m = m'$ and $p$ is prime
  output $p$ and $m$
output "no prime power"

**Instruction** Explain that in the following we focus on composite numbers which are not prime powers.

**Convention** In the following $n$ denotes a composite number which is not a prime power and $a$ denotes a number such that $1 < a < n - 1$.

**Remark** If $\text{gcd}(a,n) > 1$, then $\text{gcd}(a,n)$ is a non-trivial factor of $n$.

**Math Input** If $a$ is such that $a^2 \Mod 1 = 1$, then $\text{gcd}(a-1, n)$ is a non-trivial factor of $n$.

**Instruction** Explain the proof.

**Proof** From $a^2 \Mod n = 1$, we obtain $a^2 - 1 \Mod n = 0$, which implies $n \mid (a - 1)(a + 1)$. If we had $\text{gcd}(a-1,n)=1$, then we would have $n \mid a + 1$, which would imply $n = a + 1$ and thus $a = n-1$. A contradiction. So $\text{gcd}(a-1,1) > 1$, which means that $\text{gcd}(a-1,n)$ is a non-trivial factor of $n$.

**Math Input** For every $a$ with $\text{gcd}(a,n)$ there is a number $m > 1$ such that $a^m \Mod n = 1$ and all numbers $a^j \Mod n$ for $1 < j \leq m$ are distinct. Moreover, $a^j = a^k$ iff $j \Mod m = k \Mod m$. The number $m$ is called the order of $a$ and denoted $o_n(a)$.

**Math Input** The probability that a random $a$ with $\text{gcd}(a,n) = 1$ is such that $o_n(a) = 2r$ (even order!) and $a^r \Mod n \neq n-1$ is $\geq 1/4$. (For such an $a$s, the number $\text{gcd}(a^r \Mod n - 1, n)$ is a non-trivial factor of $n$.)

**Activity** Let $n = 45$. Go through all $a$s and determine the respective case: not coprime with $n$; coprime with $a$ and odd order; coprime with $n$ and even order and $a^{o_n(a)/2} \Mod n = n-1$; coprime with $n$ and even order and $a^{o_n(a)/2} \Mod n = n-1$.

**Instruction** Explain what the proof involves and prove this for $n = pq$ with $p$ and $q$ distinct prime numbers $>2$.

**Activity** Devise a probabilistic polynomial-time algorithm for factoring assuming that $o_n(a)$ can be determined in polynomial time.

**Alg** FactoringWithOracle($n$)
// $n$ is a composite number.
Check if $n$ is a prime power and if so output the prime.
Choose $a$ with $1 < a < n-1$ at random.
Let $b = \text{gcd}(a, n)$.
if $b \neq 1$ then
 output $b$
else 
 Determine $m = o_n(a)$.
 Determine $b = a^{m} \Mod n$.
 if $b = n-1$
  Output "fail".
 else
  Output $\text{gcd}(b - 1, n)$.
// With probability $\geq 1/4$, the output is a non-trivial factor of $n$.

**Instruction** Explain what quantum computing is good for: determininig $o_n(a)$.

# Finding Periods -- Simple Case

**Def** A function $f \colon \mathbf N \to S$ with $S$ finite is periodic if there exists some $r$ such that $$f(k) = f(k') \qquad \text{iff} \qquad k \Mod r = k' \Mod r$$ for all $k, k' \in \mathbf N$. The value $r$ is unique and is called the period of $f$. 

**Example** Every injective function $\mathbf N \to \mathbf N$ is periodic with period $0$.

**Example** If $\omega$ is a primitive $N$th root of unity then $k \mapsto \omega^k$ is periodic with period $N$.

**Example** Let $a < M \leq N$ and $a$ and $M$ coprime. Then $$s \mapsto a^s \text{ mod } M$$ is a periodic function.

**Def** The period problem is: Given a periodic function $f$ restricted to $\{0, \dots, N-1\}$ and period $< N$, determine its period.

**Def** The quantum period problem is: Given $O_f$ for a periodic function $f$ restricted to $\{0, \dots, N-1\}$ with period $< N$, determine its period.

**Instruction** Explain similarity to Simon's problem: $\mathbf Z_2^n$ vs $\mathbf Z_N$.

**Instruction** Describe the "simple case" where $r \mid 2^n$, which is trivial for discrete algorithms. We then build on this.

**Instruction** Explain why we are using $N$ instead of $2^n$.

**Alg** ShorPeriodCore$(O_f)$
// The function $f \colon \mathbf \{0, \dots, N-1\} \to \{0, \dots, N-1\}$ is the restriction of a periodic function with period $r < N$ and $r \mid N$ where $N = 2^n$.
1. // Initialize with $\Ket 0^{2n}$.
  State: $\Ket {0^{2n}}$.
1. Apply $H^{\otimes n} \otimes I^{\otimes n}$ (or $F_n \otimes I^{\otimes n}$). 
  State: $\Ket{+^n}\Ket{0^n}$.
1. Apply $O_f$.
  State: $\displaystyle \Oo N \sum_{j < N} \Ket j \Ket {f(j)} = \Oo {N} \sum_{j < r} \left(\sum_{s < N/r} \Ket{j + rs} \right)\Ket{f(j)}$. 
1. // Measure bits $n, \dots, 2n-1$.
  Output: $f(j)$ for some $j < r$.
  State: $\displaystyle \sqrt {\frac r N}\sum_{s < N/r} \Ket {j + rs}$.
5. Apply $F_n$.
  State: $\displaystyle \sqrt {\frac r N} \sum_{s < N/r} \Oo N \sum_{k < N} \omega^{(j + sr)k} \Ket k$
  $\displaystyle = \frac {\sqrt r} N \sum_{k < N} \sum_{s < N/r} \omega^{(j + rs)k} \Ket k$
  $\displaystyle = \frac {\sqrt r} N \sum_{k < N} \omega^{jk} \sum_{s < N/r}  \omega^{rsk} \Ket k$
  $\displaystyle = \frac {\sqrt r} N \sum_{k < N} \omega^{jk} \sum_{s < N/r}  (\omega^{rk})^s \Ket k\qquad$ // $\omega^{rk}$ is $\frac N r$th root of unit 
  $\displaystyle = \frac {\sqrt r} N \sum_{k < N \text{ where } N \mid rk} \omega^{jk}\frac N r \Ket k$
  $\displaystyle = \Oo r \sum_{k < N \text{ where } N \mid rk}  \omega^{jk} \Ket k$.
  $\displaystyle = \Oo r \sum_{t < r \text{ and }Nt = rk} \omega^{jk} \Ket k. \qquad$ // Denoted $\Ket u$ in the following.
1. Measure $k$.

**Lemma** 
1. If $N \mid kr$ for $k < N$, then  $\Norm{\IP u k} = \frac 1 {\sqrt r}$.
2. If $N \not\mid kr$ for $k < N$, then $\Norm{\IP u k} = 0$.

**Remark**
1. Only $\Ket k$ satisfying $Nt = rk$ for $t < r$ are measured and those are measured with equal probability.
1. If $r$ and $t$ are coprime, then $r$ is the nominator of $\frac k N$ when written as a fraction in lowest terms.
1. If $r$ and $t$ are not coprime, then the nominator from above, say $r'$, is less than $r$ and thus $f(0) \neq f(r')$. 



**Lemma 2** Let $p_r$ be the probability that a random $t < r$ is coprime with $r$. Then $p_r \in \Omega\left(\frac 1 {\log  r}\right)$.

**Alg** ShorPeriodPost
1. Write $\frac k N$ as a fraction in lowest terms, say $\frac k N = \frac {t'}{r'}$. 
1. If $f(0) = f(r')$, then output $r'$, else output "fail".

**Alg** ShorPeriodPre
1. Check whether the period is $r$ for values of $r < C$ for some constant $C$.

**Alg** ShorPeriod$(O_f)$
ShorPeriodPre
ShorPeriodCore$(O_f)$
ShorPeriodPost

**Theorem** The success probability of ShorPeriod is $\Omega\left(\frac 1 {\log r}\right)$.

# Finding Periods -- General Case

**Instruction** Explain why things get complicated in the general case and define the general case. 

**Instruction** Explain the general case with $r \ll N$.

**Alg** ShorPeriodCore$(O_f)$
// The function $f \colon \mathbf \{0, \dots, N-1\} \to \{0, \dots, M-1\}$ is a restriction of a periodic function with period $r < M$, $N = 2^n$, and $M^5 < N$.
1. // Initialize with $\Ket 0^{2n}$.
  State: $\Ket {0^{2n}}$.
1. Apply $H^{\otimes n} \otimes I^{\otimes n}$ (or $F_n \otimes I^{\otimes n}$). 
  State: $\Ket{+^n}\Ket{0^n}$.
1. Apply $O_f$.
  State: $\displaystyle \Oo N \sum_{j < N} \Ket j \Ket {f(j)} = \Oo {N} \sum_{j < r} \left(\sum_{s < K_j} \Ket{j + rs} \right)\Ket{f(j)}$
  where $N \leq j + r K_j < N + r$ for some integer $K_j$. 
1. Measure bits $n, \dots, 2n-1$.
  Output: $f(j)$ for some $j < r$. Let $K = K_j$.
  State: $\displaystyle \Oo {K}\sum_{s < K} \Ket {j + rs}$.
5. Apply $F_n$.
  State: $\displaystyle \Oo K \sum_{s < K} \Oo N \sum_{k < N} \omega^{(j + sr)k} \Ket k$
  $\displaystyle = \Oo{NK} \sum_{s < K} \omega^{(j + rs)k} \Ket k$
  $\displaystyle = \Oo {NK} \sum_{k < N} \omega^{jk} \sum_{s < K}  \omega^{rsk} \Ket k$
  $\displaystyle = \Oo {NK} \sum_{k < N} \omega^{jk} \sum_{s < K}  (\omega^{rk})^s \Ket k.$ // Denoted $\boldsymbol u$ in the following.
  
**Lemma** With the above notation, if $k < N$ and $t$ are such that $|kr - Nt| \leq r/2$, then  $|\boldsymbol u_k| \in \Omega\left(\frac 1 {\sqrt r}\right)$.

**Proof** Let $\theta \in [-\pi,\pi]$ be such that $e^{\theta i} = \omega^{kr} = \omega^{kr - Nt}$. So $|\theta| \leq \frac{\pi r} N$. This implies $s \theta \leq \pi$ for all $s < K$, because $K < \frac N r + 1$. From the above lemma, we obtain $|\sum_{s < K}  (\omega^{rk})^s| \geq \frac K {3 \sqrt 2}$, so 
\begin{align}
\left|\boldsymbol u_k\right| 
& = \left|\Oo {NK} \omega^{jk} \sum_{s < K}  (\omega^{rk})^s\right|\\
& = \Oo {NK} \left|\sum_{s < K}  (\omega^{rk})^s\right|\\
& \geq \Oo {NK} \frac K {3 \sqrt 2}\\
& = \sqrt {\frac K N} \frac 1 {3 \sqrt 2}\\
& \geq \frac 1 {3 \sqrt {2 r}} \enspace.  
\end{align}

**Lemma** There are $\Omega\left(\frac r {\log r}\right)$ integer $k$ with $0 \leq k < N$ such that $|kr - Nt| \leq r/2$ and $t$ and $r$ are coprime. 

**Proof** Let $p_0, \dots, p_{s-1}$ be an ascending list of all prime numbers $< r$ which are not prime factors of $r$. Then $s \in \Omega\left(\frac r {\log r}\right)$. For each $j < s$ there is some $k_j$ such that $\left|\frac{k_j} N - \frac {t_j} r\right| \leq \frac 1 {2N}$. These $k_j$s satisfy the required condition.

We need to show that all the $k_j$s are distinct. So suppose this is not the case. Then there is $j$ such that $k_j \geq k_{j+1}$. This implies $t_j + \frac 1 {2N} \geq t_{j+1} - \frac 1 {2N}$, from which we conclude $1 \leq \frac 1 N$, which is a contradiction. 

**Corollary** With probability $\Omega\left(\frac 1 {\log r}\right)$, the algorithm outputs $k$ such that there exists $t$ with $|kr - tN| \leq r/2$.

**Lemma** Assume $|kr - tN| \leq r/2$. 

**Proof** Rearranging the assumption, we obtain $$\left|\frac k N - \frac t r\right| \leq \frac 1 {2N} \enspace.$$ From $r^2 \leq M^2 < N$, we conclude $$\left|\frac k N - \frac t r\right| < \frac 1 {4r^2} \enspace.$$ 

If $\frac a b$ is a best approximation of $\frac k N$ with $b \geq 2r$ and in lowest terms, then $$\left|\frac k N - \frac a b\right| \leq \frac 1 {4r^2}\enspace,$$ which implies $$\left|\frac t r - \frac a b\right| < \frac 1 {2r^2}\enspace.$$ Hence, $a = t$ and $b = r$.

# Rational Approximation

**Instruction** Explain the problem: We know that a rational number is close to a given real number and we want to find a representation of the rational number in lowest terms. This is not fully specified; details to follow.

**Math Input** For every positive real number $x$ there is at most one pair $\langle a, b\rangle$ such that $0 < b \leq N$ and  $|x - \frac a b| < \frac 1 {2N^2}$.

**Proof** Assume $\langle a', b'\rangle$ with $b' \leq N$ is such that $|x - \frac {a'}{b'}| < \frac 1 {2N^2}$ and $\frac a b \neq \frac {a'}{b'}$. Then $|\frac a b - \frac {a'}{b'}| < \frac 1 {N^2}$. The difference can be written as, $\frac {ab' - a'b}{bb'}$, which implies $|\frac a b - \frac {a'}{b'}| \geq \frac 1 {bb'} \geq \frac 1 {N^2}$, which is a contradiction.

**Activity** Show that $|x - \frac a b| \leq \frac 1 {2N^2}$ is not strong enough as an assumption.

**Example** $1 < \frac 3 2 < 2$.

**Def** Best approximation ...

**Theorem** There is an algorithm (see below) which for a given rational number $x$ outputs a finite sequence of pairs $\langle p_0, q_0\rangle, \dots, \langle p_m, q_m\rangle$ with the following properties.
1. $q_0 \leq q_1 \leq \dots$ .
1. $\frac {p_0}{q_0} < \frac {p_2}{q_2} < \dots < x = \frac {p_m}{q_m} < \dots < \frac {p_3}{q_3} < \frac {p_1}{q_1}$.
2. $p_n q_{n-1} - q_n p_{n-1} = (-1)^n$ for $-1 \leq n \leq m$.
3. $|x - \frac{p_n}{q_n}| < \frac{1}{q_n q_{n+1}}$ for $0 \leq n < m$.

**Alg** Approx$(x)$
let $p_{-2} = 0$, $p_{-1} = 1$, $q_{-2} = 1$, $q_{-1} = 0$
let $n = -1$
while $q_n < N$ and $x \neq \lfloor x \rfloor$
 let $n = n + 1$
 let $a_n = \lfloor x \rfloor$
 let $x = \frac 1 {x - a_n}$
 $p_n = a_n p_{n-1} + p_{n-2}$
 $q_n = a_n q_{n-1} + q_{n-2}$
 output $\langle p_n, q_n\rangle$
 
**Remark** 
1. The $q_n$s grow exponentially fast.
1. Each $\frac {p_n}{q_n}$ is in lowest terms.

**Lemma** If $|x - \frac a b| < \frac 1 {4b^4}$, then $\frac a b$ is among the $\frac{p_n}{q_n}$s.
 
**Proof** If $q_m < b$, then $\frac {p_m}{q_m} = \frac a b$. If not, let $n$ be such that $q_n < b$ and $q_{n+1} \geq b$. 

First case, $q_{n+1} \leq 2b^2$. Sind $q_{n+1} \geq b$ and $\frac {p_{n+1}}{q_{n+1}}$ is a best approximation of $x$ (at least as good as $\frac a b$, we have $|x - \frac {p_{n+1}}{q_{n+1}}| < \frac 1 {4b^4}$. So by the above lemma, $\frac a b = \frac {p_{n+1}}{q_{n+1}}$.

Second case, $q_{n+1} > 2b^2$. Then 4. from above implies $|x - \frac {p_n}{q_n}| < \frac 1 {2 b^2}$. From the above lemma, we obtain $\frac a b = \frac {p_n}{q_n}$.


# Shor's Algorithm

# Quantum Support Vector Machines???

---

# PART II - Complexity

---

# Boolean and Real Quantum Circuits

**Instruction** Explain that we want each computational step (state transition) only involve a few (at most three) quantum bits. 

**Reminder** A boolean function is a function $\Bool m \to \Bool n$.

**Reminder** A boolean circuit $C$ of size $s$ with $n$ inputs is a (straight-line) program of the following form:

$y_0 = E_0$
$y_1 = E_1$
$\dots$
$y_{s-1} = E_{s-1}$

The requirements are: Every $E_i$ is an expression of the form 
- $v \vee v'$, 
- $v \wedge v'$, 
- $\neg v$ 

with $v, v' \in \{x_0, \dots, x_{n-1}, y_0, \dots, y_{i-1}\}$. It is straightforward to define the semantics of such circuits. For every $i < n$, one sets $\text{eval}_C(b_0 \dots b_{n-1}, x_i) = b_i$, and then inductively defines $\text{eval}_C(b_0 \dots b_{n-1}, y_j)$ for every $j < m$.

A boolean function $f \colon \Bool n \to \Bool m$ is computed by the circuit if there is a sequence $v_0, \dots, v_{m-1}$ of variables from $\{x_0, \dots, x_{-1}, y_0, \dots, y_{m-1}\}$ such that for every $b_0 \dots b_{n-1} \in \Bool n$, we have $$f(b_0 \dots b_{n-1}) = \text{eval}_C(b_0 \dots b_{n-1}, v_0) \dots \text{eval}_C(b_0 \dots b_{n-1}, v_{m-1}) \enspace.$$

**Activity** Find a circuit that adds two one-bit numbers, that is, $f \colon \Bool 2 \to \Bool 2$ where $f(b_0c_0) = d_1d_0$ with $b_0 + c_0 = 2 \times d_1 + d_1$.

**Lemma** If $f \colon \Bool m \to \Bool n$ is a boolean function, then there is a unique orthogonal transformation $O_f$ with $$O_f(\Ket {b_0 \dots b_{m-1} c_0 \dots c_{n-1}}) = \Ket{b_0 \dots b_{m-1} (c_0 \oplus f(b_0 \dots b_{m-1})_0) \dots (c_{n-1} \oplus f(b_0 \dots b_{m-1})_{n-1})}\enspace.$$

**Activity**
- Let $\text{id}(b) = b$ for $b \in \Bo$. What is $O_\text{id}\Ket {b0}$ for $b \in \Bo$? 
- Determine $O_fO_f$ for any boolean function $f$.

**Def** A quantum gate is a quantum state transition on a qureg with at most three bits. In particular, 
- $O_\text{id}$ is called CNOT (controlled not) and
- $O_\wedge$ is called Toffoli gate.

**Def** A quantum circuit of size $s$ on $n$ qubits is a sequence of $s$ instructions of the form $$T \text{ on } j_0, \dots, j_{m_0-1}$$ where $T$ is a quantum gate on $m$ qubits and the $j_i$s are pairwise distinct indices $< n$. This instruction is interpreted as the unique orthogonal transformation $T'$ defined as follows. Let $b \in \Bool n$ and assume $$T(\Ket{b_{j_0} b_{j_1} \dots b_{j_{m-1}}}) = \sum_{c_{j_0} c_{j_1} \dots c_{j_{m-1}}  \in \Bool m} \gamma_{c_{j_0} c_{j_1} \dots c_{j_{n-1}}} \Ket{c_{j_0} c_{j_1} \dots c_{j_{m-1}}} \enspace.$$ Then $$T'\Ket b = \sum_{c_{j_0} c_{j_1} \dots c_{j_{m-1}}}  \gamma_{c_{j_0} c_{j_1} \dots c_{j_{m-1}}} \Ket{\{b_j\}_{j \in n \setminus J} \{c_j\}_{j \in J}}$$ where $J = \{j_0, \dots, j_{m-1}\}$.

The orthogonal transformation computed by the circuit is the composition of all the individual orthogonal transformations.

**Example** The core of CoinFlip, written as a circuit, is as follows.

$H \text{ on } 0$

**Activity** Rewrite the core of the Deutsch-Josza algorithm as a circuit.

**Instruction** 
- Explain graphical notation.
- Explain parallelism.

**Problem** Show that consecutive program lines can be switched if they operate on disjoint sets of qubits.

**Problem** Suppose you would have to restrict the bits that quantum gates are allowed to operate on in the following way. All quantum gates except for $T$ operate only on the bit $0$, the bits $0,1$, or the bits $0,1,2$. How would you choose $T$? Explain?

**Theorem** For every boolean circuit on $n$ inputs of size $s$ computing $f \colon \Bool n \to \Bool m$, there is a quantum circuit $Q$ of size ... computing a transformation $T$ such that $$T(\Ket {x}\Ket{0^{2m+s}}) = O_f(\Ket{x})\Ket{0^{s+m}}$$ for all $\Ket x$.

**Sketch of proof** Let $v_0 \dots v_{m-1}$ be the sequence of variables that denotes the value of the function. We denote the qubits by $x_0 \dots x_{n-1} v_0 \dots v_{m-1} y_0 \dots y_{s-1}$.  A high-level description of the quantum circuit: 

1. // Evaluate the circuit.
  For $i = 0$ to $s-1$, mimick line $i$. Apply $O_f$ on the respective two or three qubits, where $f$ is one of $\wedge$, $\vee$, and $\neg$.
  // $\Ket x \Ket{0^m} \Ket{y_0 \dots y_{s-1}}$
1. // Copy the values into the right places. 
  For $i = 0$ to $m-1$, apply CNOT to copy the respective $x_j$ or $y_j$ to $v_i$.
  // $\Ket x \Ket{f(x)} \Ket{y_0 \dots y_{s-1}}$
1. // Reverse step 1.
  For $i = s-1$ to $0$, mimick line $i$. Apply $O_f$ on the respective two or three qubits, where $f$ is one of $\wedge$, $\vee$, and $\neg$.
  // $\Ket x \Ket{f(x)} \Ket{0^s}$
  
**Remark** 
1. Every boolean circuit of size $s$ with $n$ inputs can be represented by an $(n + s) \times (n + s)$ matrix with entries from $\{-, \vee, \neg\}$, or, alternatively, by $2 (s+n)^2$ bits. Such a representation is called an encoding of the circuit.
1. There is a polynomial $p$ such that for every $n$ and $s$ there is a circuit of size $p(n,s)$ which computes the boolean function $u_{n,s} \colon \Bool {n + 2(n + s)^2} \to \Bo$ which satisfies $$u_{n,s}(xe_C) = f_C(x)$$ for every $x \in \Bool n$, circuit $C$ of size $s$, and $f_C$ the function the boolean function which is computed by $C$ (value of $y_{s-1}$).  


# 

---
---
---

<!-- 

# Math Input

**Def**
\begin{align}
  \Ket u & = \Ket v - \Oo {2^n} \Ket a & \Ket{u'} & = \Ket a\\
  \Ket v & = \sum_{b \in \Bool n} \Ket b 
  & \Ket {v'} & = \Ket a - \frac 1 {2^n} \Ket v
\end{align}

\begin{align}
  \Ket a - \Oo{2^n} \Ket v & \mapsto -(\Ket a - \Oo{2^n} \Ket v)\\
\end{align}


# Other Stuff

## The EPR Game

**Def** EPR game, where Alice and Bob play against nature:
1. $x,y \in \Bo$ are chosen at random.
2. Alice is given $x$, Bob is given $y$.
3. Alice and Bob choose $a \in \Bo$ and $b \in \Bo$, respectively, without communicating with each other.

It is a win for Alice and Bob if $a \oplus b = x \wedge y$, or, equivalently, if
- $x = y = 1$ and $a \neq b$, or
- $x = 0$ or $y = 0$ and $a = b$.

We set $W(x,y,a,b) = 1$ if $a$ and $b$ are a win for Alice and Bob for the given values $x$ and $y$.

**Def** A mixed strategy for Alice and Bob is given by $\Bo$-valued random variables $\boldsymbol f_0$, $\boldsymbol f_1$. $\boldsymbol g_0$, and $\boldsymbol g_1$. The advantage of Alice and Bob is given by $$\text{adv}[\boldsymbol f_0, \boldsymbol f_1, \boldsymbol g_0, \boldsymbol g_1] = \Pr{W(\boldsymbol x, \boldsymbol y, \boldsymbol f_{\boldsymbol x}, \boldsymbol g_{\boldsymbol  y}) = 1}$$ for random variables $\boldsymbol x$, $\boldsymbol y$, distributed uniformly at random.

**Lem** 
- $\text{adv}[f,g] = \frac 3 4$ for $f \equiv g \equiv 0$.
- For all $f,g$, $\text{adv}[f,g] \leq \frac 3 4$.

**Instruction** Explain idea of quantum realization, in particular, motivation from the point of quantum mechanics.

1. A 2-bit qureg is initialized with the EPR state $(\frac 1 {\sqrt 2} (\Ket {00} + \Ket {11}))$.
2. Alice takes bit $0$ of the register; Bob takes bit $1$ of the register.
3. Alice receives $x$; Bob receives $y$.
4. If $x=1$, then Alice applies to her bit a rotation of $\alpha$.
5. If $y=1$, then Bob applies to his bit a rotation of $- \alpha$.
6. The register is measured.
5. Alice and Bob choose $a$ and $b$ according to the bit they obtain.

**Lem** In the above implementation of the game, Alice and Bob win with probability $\geq 0.85$.








## general case

**Math Input** For $0 \leq x \leq 1$ we have $\frac 1 6 x \leq \sin x \leq x$ (Taylor series).

![](https://estragon.theorie.informatik.uni-kiel.de/uploads/upload_5f2f35d8bd68fc75ba2a71ad994acb09.png)

**Math Input** For $\nu = e^{2 \theta}$ with $\theta \leq \pi/2$, we have $|1 - \nu| = 2 \sin \theta$. 


**Proof** Let $\theta \in (- \pi/2, \pi/2]$ be such that $\omega^{rk} = e^{2\theta i}$. Then:
1. $|\theta| \leq \frac{2\pi} N \frac r 2= \frac{\pi r} {N}$.
2. $|K \theta| \leq (\frac{N} r + 1) \frac{\pi r} {N} \leq \pi + \frac{\pi r} {N} < 1$. 

So $\left|\frac{1-\nu^K}{1-\nu}\right| = \frac{|2 \sin K\theta|}{|2 \sin \theta|} \geq \frac{\frac 1 6 K \theta}{\theta} \geq \frac 1 6 K$. Hence, $$|\IP u k| \geq \frac 1 6 \sqrt{\frac K N} \geq \frac 1 6 \sqrt \frac{\frac N r - 1}N = \frac 1 6 \sqrt{\frac{1}{r} - \frac 1 N} \geq \frac 1 6 \sqrt{\frac{1}{r} - \frac 1 {r^5}} \in \Omega\left(\Oo r\right)\enspace.$$ 

**Def** An integer $k$ is said to be almost divisible by $N$ (wrt $r$) if $0 < k \Mod N < r/10$.

**Lemma** 
1. If for $k < N$ the number $kr$ is almost divisible by $N$, then  $|\IP u k| \in \Omega\left(\frac 1 {\sqrt r}\right)$.
2. Let $Q_r$ be the number of $k < N$ such that (i) $kr$ is almost divisible by $N$ and (ii) $r$ is coprime with $\lfloor kr/N\rfloor$. Then $Q_r \in \Omega\left(\frac r {\log r}\right)$.

**Proof of 1.** We use:
1. For $0 \leq x \leq 1$ we have $\frac 1 6 x \leq \sin x \leq x$ (Taylor series).
2. For $\nu = e^{2 \theta}$ with $\theta \leq \pi/2$, we have $|1 - \nu| = 2 \sin \theta$.  

Assume $kr$ is almost divisble by $N$. Then $\nu = e^{2\theta i}$ with $0 < \theta < \frac{2\pi}N \frac r {10}= \frac{\pi r} {5N}$ and $\nu^K = e^{2 K \theta}i$ with $0 < K \theta < (\frac{N}r + 1) \frac{\pi r} {5N} \leq \frac \pi 5 + \frac{\pi r} {5N} < 1$. So $\left|\frac{1-\nu^K}{1-\nu}\right| = \frac{2 \sin K\theta}{2 \sin \theta} \geq \frac{\frac 1 6 K \theta}{\theta} \geq \frac 1 6 K$. Hence, $$|\IP u k| \geq \frac 1 6 \sqrt{\frac K N} \geq \frac 1 6 \sqrt \frac{\frac N r - 1}N = \frac 1 6 \sqrt{\frac{1}{r} - \frac 1 N} \geq \frac 1 6 \sqrt{\frac{1}{r} - \frac 1 {r^5}} \in \Omega\left(\Oo r\right)\enspace.$$ 

**Proof of 2.** We use:
1. There are $\Omega(x/\log x)$ prime numbers $< x$. (Prime number theorem.)
1. Every $x \in \mathbf N$ with $n >1$ is divisible by at most $\log x$ prime numbers.



We first prove this for $r$ and $N$ coprime. In this case, there is a multiplicative inverse for $r$ in $\mathbf Z_n$, that is, there is some $s < N$ such that $sr \Mod N = 1$. 

Let $p_0, \dots, p_{t-1}$ be an enumeration of all prime numbers $p$ such that $p < r/10$ and $p \not\mid r$. Then $$(p_is) r \Mod N = p_i (sr \Mod N) \Mod N = p_i$$ for every $i < s$, which means that every $(p_is)r$ is almost divisible by $N$. By definition of $\text{mod}$, we have $$(p_is)r \Mod N = (p_is)r - \lfloor (p_is)r/N \rfloor N\enspace.$$ Combining the two displayed equations, we get $$p_i = (p_is)r - \lfloor (p_is)r/N \rfloor N\enspace.$$ Now, if $r$ and $\lfloor (p_is)r/N \rfloor$ are not coprime, then there is some prime number $q$ which divides the two numbers. Hence, by the above equation, $q \mid p_i$, which means $q = p_i$. This cannot be the case, because we stipulated $p_i \not\mid r$. In other words, $r$ and $\lfloor (p_is)r/N \rfloor$ are coprime. So we have $Q_r \geq s$.

Altogehter, using 1. and 2., we have $$q \geq s \in \Omega\left(\frac r {\log r} - \log r\right) = \Omega\left(\frac r {\log r}\left(1  - \frac{(\log  r)^2}r\right)\right) = \Omega\left(\frac r {\log r}\right)\enspace.$$

When $r$ and $N$ are not coprime, we proceed as follows. We let $d = \text{gcd}(r,N)$ and set $r' = r/d$ and $N' = N/d$. Then $r'$ and $N'$ are coprime and from the first part we obtain that there are $\Omega\left(\frac{r/d}{\log(r/d)}\right)$ numbers $k < N'$ such that $kr'$ is almost divisible by $N'$ and $r'$ and $\lfloor kr'/N' \rfloor$ are coprime. 

We observe:
1. For every $k < N'$ and $c < d$, the number $k+cN'$ is a number $< N$.
1. For every $k < N'$ and $c < d$, we have $$(k + cN')r \Mod N = kr + cN'dr' \Mod N = kr \Mod N = kdr' \Mod dN' = d (kr' \Mod N' \leq d \frac{r'}{10} \leq \frac r {10}\enspace.$$
1. Let $q$ be a prime such that $q \mid r$ and $q \mid \lfloor (k + cN')r/N\rfloor$. Then $q \mid dr'$ and $q \mid \lfloor kr/N + cN'r/N\rfloor = \lfloor kr'/N' + cr'\rfloor = \lfloor kr'/N'\rfloor + cr'$. If $q \mid r'$, then $q \mid \lfloor kr'/N'\rfloor$---a contradiction. So $q \mid d$. So 
