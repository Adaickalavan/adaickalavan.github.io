---
title: "Quantum Computing"
excerpt: "Qiskit, Python"
header:
  teaser: /assets/images/quantum/quantum_computer.jpg
date: "2024-09-25" 
---

## Introduction
This project explores quantum computing and Qiskit. We present several foundational quantum computing algorithms. Each file is a standalone example. The [Dancing with Qubits - Second edition](https://www.packtpub.com/en-ca/product/dancing-with-qubits-9781837636754) book by Robert S. Sutor is an excellent comprehensive book, suitable for beginners, which teaches quantum computing beginning from the basics.

The following tools will be used in this project:
+ Qiskit
+ Python

## Code
Find the source code in the [repository](https://github.com/Adaickalavan/quantum).

## Learning Outcome
At the end of this project, we should understand and be comfortable using Qiskit to implement quantum agorithms.

## Installation
First, perform generic setup as follows.  
```bash
cd <path>/quantum
python3.10 -m venv ./.venv
source ./.venv/bin/activate
pip install --upgrade pip
pip install qiskit[visualization]
pip install qiskit-ibm-runtime
pip install matplotlib pylatexenc ipykernel
```

Users are recommended to run each of this repository's code files inside the interactive window in VSCode.

Alternatively, users may run the code inside JupyterLab. Follow the additional steps below to install and launch JupyterLab.
```bash
cd <path>/quantum
pip install jupyterlab
# Once installed, launch JupyterLab with:
jupyter lab
```

Optional libraries for code formatting.
```bash
cd <path>/quantum
pip install black[jupyter] isort
# Execute to format code.
make format
```

## Quantum programs
1. [Swap test](https://github.com/Adaickalavan/quantum/blob/main/swap_test.py)

    [![swap test](/assets/images/quantum/swap_test.png)](/assets/images/quantum/swap_test.png)

    Compare the states of two single-qubit registers. If the two input states are equal, the output register results in $\|1⟩$ state. An useful interpretation is to see that the probability of a $\|1⟩$ outcome is a measure of just how identical the two inputs are.

    Explanation of the circuit is as follows. Circuit state at the beginning is $\|\phi,\psi,0⟩$. After Hadamard gate, the circuit state is $\frac{1}{\sqrt 2}\left(\|\phi,\psi,0⟩ + \|\phi,\psi,1⟩\right)$. After the controlled SWAP gate, the circuit state becomes $\frac{1}{\sqrt 2}\left(\|\phi,\psi,0⟩ + \|\psi,\phi,1⟩\right)$. After second Hadamard gate, the circuit state becomes 

    $$
    \begin{align*}
    & \frac{1}{2}\left(|\phi,\psi,0⟩ + |\phi,\psi,1⟩ + |\psi,\phi,0⟩ - |\psi,\phi,1⟩ \right) \\
    & = \frac{1}{2}|0⟩\left(|\phi,\psi⟩ + |\psi,\phi⟩\right) + \frac{1}{2}|1⟩\left(|\phi,\psi⟩ - |\psi,\phi⟩ \right) \\
    \end{align*}
    $$

    After the $x$ gate, the circuit state becomes $\frac{1}{2}\|0⟩\left(\|\phi,\psi⟩ - \|\psi,\phi⟩ \right) + \frac{1}{2}\|1⟩\left(\|\phi,\psi⟩ + \|\psi,\phi⟩\right)$. The measurement gate outputs $\|1⟩$ with a probability of

    $$
    \begin{align*}
    & \left|\frac{1}{2}\left(|\phi,\psi⟩ + |\psi,\phi⟩\right)\right|^2 \\
    & = \frac{1}{2}\left(⟨\phi|⟨\psi| + ⟨\psi|⟨\phi|\right)\frac{1}{2}\left(|\phi⟩|\psi⟩ + |\psi⟩|\phi⟩\right) \\
    & = \frac{1}{2} + \frac{1}{2}|⟨\phi|\psi⟩|^2 \\
    \end{align*}
    $$

    Here, $\|⟨\phi\|\psi⟩\|^2 = 1$ if the qubits are identical, else $\|⟨\phi\|\psi⟩\|^2 = 0$ if they are orthogonal.

1. [Teleport](https://github.com/Adaickalavan/quantum/blob/main/teleport.py)

    [![teleport](/assets/images/quantum/teleport.png)](/assets/images/quantum/teleport.png)

    Alice teleports the quantum state of her payload qubit using an entangled pair of qubits shared with Bob. Only two classical bits are needed to transmit Alice’s qubit state (i.e., magnitudes and relative phase) and Bob's retrieved qubit state will be correct to a potentially infinite number of classical bits of precision. Because a traditional channel is needed to convey the two classical bits from Alice to Bob, the speed of teleportation can be no faster than the speed of light. <br>

    To verify successful teleportation, Bob applies the gates, which Alice applied on $\|0⟩$ to prepare her payload, to his retrieved qubit in reverse. If Bob's retrieved qubit matches that sent by Alice, the final measurement result after verification gates should always be `0` in a perfect quantum circuit.

1. [Arithmetic](https://github.com/Adaickalavan/quantum/blob/main/arithmetic.py)

    [![arithmetic](/assets/images/quantum/arithmetic.png)](/assets/images/quantum/arithmetic.png)

    Create two quantum registers and initialize them to $a=\sqrt{0.5}\|1⟩+\sqrt{0.5}\|5⟩$ and $b=\sqrt{0.5}\|1⟩+e^{i\pi/4}\sqrt{0.5}\|3⟩$. Decrement register $a$ by 3. Then, increment register $b$ conditional on register $a<0$. Here, register $a$ is assumed to be in two’s-complement, where the highest-order bit indicates the sign. Finally, increment register $a$ by 3.

1. [Scratch qubit](https://github.com/Adaickalavan/quantum/blob/main/scratch_qubit.py)

    [![scratch qubit](/assets/images/quantum/scratch_qubit.png)](/assets/images/quantum/scratch_qubit.png)

    Scratch qubits play a temporary role in enabling quantum operations. A specific example of an otherwise irreversible operation that can be made reversible with a scratch qubit is `abs(a)`. The `abs()` function computes the absolute value of a signed integer. We assume two’s-complement notation here. <br>

    In this example, `abs` of a quantum register `a` is computed. Then, add `abs(a)` to another quantum register `b`. Finally, uncompute (i.e., reverse the operations on) the scratch qubit and quantum register `a` to return them to their initial states.

1. [Amplitude amplification](https://github.com/Adaickalavan/quantum/blob/main/amplitude_amplification.py) <a id="amplitude-amplification"></a>

    [![amplitude amplification](/assets/images/quantum/amplitude_amplification.png)](/assets/images/quantum/amplitude_amplification.png)

    Amplitude amplification converts inaccessible phase differences inside a quantum processor into measurable magnitude differences. Amplitue amplification consists of iterative `flip` followed by `mirror` subroutines. Subroutine `flip` marks the desired state by a phase-flip. Subroutine `mirror` reflects each state about the average overall state. This results in the marked state having a larger read probability than nonmarked states. In Grover search algorithm, the `flip` and `mirror` are known as the `oracle` and `diffuser`, respectively.

1. [Quantum Fourier Transform](https://github.com/Adaickalavan/quantum/blob/main/quantum_fourier_transform.py)

    [![QFT](/assets/images/quantum/quantum_fourier_transform.png)](/assets/images/quantum/quantum_fourier_transform.png)

    Use Quantum Fourier Transform to deduce the frequencies present in a quantum register.

1. [Phase estimation](https://github.com/Adaickalavan/quantum/blob/main/phase_estimation.py)

    [![phase estimation](/assets/images/quantum/phase_estimation.png)](/assets/images/quantum/phase_estimation.png)

    We build a phase estimation circuit to compute the eigenphase $\theta$, given a unitary quantum operation $U$ and its eigenstate. Acting an $U$ on its eigenstate produces the same eigenstate but with the eigenphase applied to its global phase. That is to say $U\|\psi⟩=e^{i2\pi\theta}\|\psi⟩$. These eigenphase rotations are kicked-back into the $m$ qubits in the counting register, creating a frequency modulation. Inverse QFT is applied to the counting register to decode the frequency present and to obtain its count value $v$. Then, $\theta = v2\pi / 2^m$.<br>

    A superposition of eigenstates as an input to the phase estimation results in a superposition of the associated eigenphases in the output. The magnitude for each eigenphase in the output superposition will be precisely the magnitude that its corresponding eigenstate had in the input register.

1. [Phase logic](https://github.com/Adaickalavan/quantum/blob/main/phase_logic.py)

    [![phase logic](/assets/images/quantum/phase_logic.png)](/assets/images/quantum/phase_logic.png)

    Phase-logic circuits flip the relative phases of all input states for which the circuit operation evaluate to true. Note that phase-logic circuits take in magnitude values such as $\|1⟩$ or $\|2⟩$, but not phase values, as inputs and encode output values in the relative phases.<br>

    Here, we solve a satisfiable 3-SAT problem by finding the combination of boolean inputs `a`, `b`, `c` which  produce a 1 output from the boolean statement `(a OR b) AND ((NOT a) OR c) AND ((NOT b) OR (NOT c)) AND (a OR c)`.<br>

    General recipe to solve satisfiability problems using phase logic is as follows.
    + Initialize the quantum register in a uniform superposition.
    + Transform the boolean statement into a number of clauses that have to be satisfied simultaneously, such that the final statement is the `AND` of a number of independent clauses.
    + Represent each individual clause using magnitude logic, storing the output in scratch qubits.
    + Perform a phase-logic `AND` between all the scratch qubits to combine the different clauses.
    + Uncompute all of the magnitude-logic operations, returning the scratch qubits to their initial states.
    + Finally, perform `mirror` subroutine, from the [amplitude amplification](#amplitude-amplification) algorithm.

    Perform the above phase `flip` (i.e., magnitude-logic and phase-logic operations) and `mirror` subroutines, as many times as necessary before reading out the quantum register.

1. [Transpile](https://github.com/Adaickalavan/quantum/blob/main/transpile.py)

    Quantum circuit must be transpiled for it to run on real devices. Qiskit's transpiler converts circuit operations to those supported by the device, maps qubits according to the device's coupling map, and performs some optimization of circuit’s gate count.

    <figure><a href="/assets/images/quantum/transpile.png">
    <img src="/assets/images/quantum/transpile.png"></a><figcaption>Circuit before (left) and after (right) transpiling.</figcaption></figure>

    Here, we transpile a quantum circuit containing `[cx, y]` gates onto a `GenericBackendV2` device which only supports `[id, rz, sx, x, cx]` gates. We may select `optimization_level`$\in [0,1,2,3]$ as input argument to the transpiler. Level 0 does the aboslute minimum necessary, level 1 is the default setting, level 2 tries to avoid qubit swaps, and level 3 is the highest which  uses smarter algorithms to cancel out gates.
