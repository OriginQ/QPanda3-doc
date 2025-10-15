Quantum Simulator  {#tutorial_quantum_simulator}
=============================================================================

[TOC]

@prev_tutorial{tutorial_circuit_and_program}
@next_tutorial{tutorial_noise}

-------------------------------------------------------------------------------------------------------------------------------

# Quantum Simulators in Quantum Computing

Quantum simulators play an important role in the field of quantum computing by providing researchers and developers with effective tools to simulate and analyze the behavior of quantum systems. These tools help not only in understanding quantum algorithms and the characteristics of quantum systems, but also in algorithm design and verification, especially when actual quantum computers are not yet widely available.
When simulating quantum circuits, selecting the appropriate simulation backend is crucial. Different quantum circuit simulators are suited for various scenarios.

## Full-Amplitude Quantum Simulator
The full-amplitude simulator can simulate and store all the amplitudes of a quantum state at once. However, it is limited by the machine's memory, and simulating more than 50 qubits becomes impractical. This type of simulator is suitable for low-qubit, high-depth quantum circuits, such as Google's random quantum circuits for low qubits, and scenarios that require obtaining the full simulation results.

Classical simulation of quantum circuits is crucial for better understanding the operations and behaviors of quantum computing. Such simulations allow researchers and developers to assess the complexity of new quantum algorithms and verify the effectiveness of quantum devices.

Full-amplitude simulation is one of the most classic simulation schemes in quantum computing, used to simulate small-scale quantum systems. In this simulation approach, the entire quantum system's state can be simulated accurately, providing insights into the system's evolution at different time points. The core idea behind this method is to represent and track the complete amplitude information of quantum states, using density matrices or pure state vectors to describe the quantum system's state.

For a single qubit, it can be considered as a two-level system. The quantum state \f$|\psi\rangle\f$can be represented as:

\f[ 
|\psi\rangle = a_0|0\rangle + a_1|1\rangle 
\f]

where \f$a_0\f$,\f$a_1\f$ are complex amplitudes, and:

\f[ 
|a_0|^2 + |a_1|^2 = 1 
\f]

In this equation, \f$∣0 \rangle\f$ and \f$∣1\rangle \f$are the two computational basis states. The quantum state can also be expressed as:

\f[
|\psi\rangle = a_0 \begin{bmatrix} 1 \\ 0 \end{bmatrix} + a_1 \begin{bmatrix} 0 \\ 1 \end{bmatrix} = \begin{bmatrix} a_0 \\ a_1 \end{bmatrix} 
\f]

For a n-qubit quantum system, the state can be represented by \f$2^n\f$ amplitudes:

\f[ 
|\psi\rangle = a_{0\ldots00}|0\ldots00\rangle + a_{0\ldots01}|0\ldots01\rangle + \ldots + a_{1\ldots11}|1\ldots11\rangle 
\f]

All amplitudes must satisfy the normalization condition:

\f[ 
\sum_i |a_i|^2 = 1 
\f]

During quantum computation, all quantum logic gates are applied to the system in matrix form. The application of a single gate 
U to the K-th qubit can be expressed as:

\f[ 
A = I^{\otimes (n-k-1)} \otimes U \otimes I^{\otimes k} 
\f]

where I is the 2×2 identity matrix, and U is a 2×2 unitary matrix. For two-qubit gates, the process is similar.

As the number of qubits increases, the number of quantum state amplitudes required grows exponentially. The following table illustrates this issue, which is known as Quantum Supremacy.

| **Number of Qubits Simulated** | **Minimum Memory Required for Storing All Quantum States (GB)** |
|:------------------------------:|:-------------------------------------------------------------:|
| 26                             | 1                                                             |
| 27                             | 2                                                             |
| 28                             | 4                                                             |
| 29                             | 8                                                             |
| 30                             | 16                                                            |
| ...                            | ...                                                           |
| 48                             | \f$ 2^{22} \f$                                                |
| 49                             | \f$ 2^{23} \f$                                                |
| 50                             | \f$ 2^{24} \f$                                                |


When the number of qubits reaches 50 or more, the required memory becomes astronomically large. This issue is known as Quantum Supremacy, which refers to the ability of quantum computers to outperform classical computers on certain specific tasks—tasks that classical computers would take years, hundreds of years, or even more to complete.

This phenomenon highlights the immense potential of quantum computers in certain fields, as they can solve problems that classical computers would be unable to solve within a feasible time frame.

We use [CPUQVM](@ref pyqpanda3.core.core.CPUQVM) to run the full-amplitude simulation algorithm. First, we construct the circuit, then run it using [run(prog, shots, noise_model)](@ref pyqpanda3.core.core.CPUQVM.run), and finally obtain the result via [result()](@ref pyqpanda3.core.core.CPUQVM.result).

```python
from pyqpanda3.core import QCircuit,H,P,RX,RY,QProg,measure,CPUQVM
circuit = QCircuit(3)
circuit << H(0)
circuit << P(2, 0.2)
circuit << RX(1, 0.9)
circuit << RX(0, 0.6)
circuit << RX(1, 0.3)
circuit << RY(1, 0.3)
circuit << RY(2, 2.7)
circuit << RX(0, 1.5)

prog = QProg()
prog << circuit
prog << measure(0, 0)
prog << measure(1, 1)
prog << measure(2, 2)

machine = CPUQVM()
machine.run(prog, 1000)
measure_result = machine.result().get_counts()
print(measure_result)
```

The result is as follows.

```python
{'000': 16, '101': 330, '110': 129, '111': 168, '010': 10, '100': 322, '001': 15, '011': 10}
```

Additionally, noise model parameters can be passed in.

```python
from pyqpanda3.core import QCircuit,H,P,RX,RY,QProg,measure,CPUQVM
from pyqpanda3.core import NoiseModel,pauli_x_error,GateType
circuit = QCircuit(3)
circuit << H(0)
circuit << P(2, 0.2)
circuit << RX(1, 0.9)
circuit << RX(0, 0.6)
circuit << RX(1, 0.3)
circuit << RY(1, 0.3)
circuit << RY(2, 2.7)
circuit << RX(0, 1.5)

prog = QProg()
prog << circuit
prog << measure(0, 0)
prog << measure(1, 1)
prog << measure(2, 2)

model = NoiseModel()
model.add_all_qubit_quantum_error(pauli_x_error(0.5),GateType.RX)
model.add_all_qubit_quantum_error(pauli_x_error(0.9),GateType.H)

machine = CPUQVM()
machine.run(prog, 1000, model)
print(machine.result().get_counts())
```

The result is as follows.

```python
{'000': 12, '101': 339, '001': 15, '100': 303, '110': 169, '011': 8, '111': 143, '010': 11}
```

QResult's member function [get_prob_dict](@ref pyqpanda3.core.core.QResult.get_prob_dict) and [get_prob_list](@ref pyqpanda3.core.core.QResult.get_prob_list) can retrieve the probabilities of the results as a dictionary or a list. If there are measurement operations in the circuit, they return the transformed measurement results (which are non-deterministic and may vary each time). Otherwise, they return the results corresponding to the ideal probabilities (which are deterministic and remain consistent across runs).

```python
from pyqpanda3.core import QCircuit,H,CNOT,RX,RY,QProg,measure,CPUQVM

circuit = QCircuit(3)
circuit << H(0)
circuit << RY(1, 0.3)
circuit << RY(2, 2.7)
circuit << CNOT(0, 1)
circuit << CNOT(1, 2)

prog = QProg()
prog.append(circuit)

machine = CPUQVM()
machine.run(prog, 1000)
prob_list_result = machine.result().get_prob_list()
prob_dict_result = machine.result().get_prob_dict()
print(prob_list_result)
print(prob_dict_result)

prog.append(measure(0, 0))
prog.append(measure(1, 1))

machine.run(prog, 1000)
prob_list_result = machine.result().get_prob_list()
prob_dict_result = machine.result().get_prob_dict()
print(prob_list_result)
print(prob_dict_result)
```

If there are measurement nodes, then output the measurement results in the form of a dictionary or list. If there are no measurement nodes, output the ideal probability distribution in the form of a dictionary or list.

```python
[0.023446405129712404, 0.0005355593660222884, 0.010630318352576206, 0.46538771715168914, 0.46538771715168914, 0.01063031835257.010630318352576206, 0.0005355593660222884, 0.023446405129712404]

{'000': 0.023446405129712404, '001': 0.0005355593660222884, '010': 0.010630318352576206, '011': 0.46538771715168914, '100': 0.8914, '100': 0.46538771715168914, '101': 0.010630318352576206, '110': 0.0005355593660222884, '111': 0.023446405129712404}

[0.481, 0.496, 0.011, 0.012]

{'00': 0.481, '01': 0.011, '10': 0.012, '11': 0.496}

```


## Full-Amplitude Quantum Simulator With GPU

The full-amplitude simulation algorithm internally uses a large amount of parallel computation for acceleration, achieving greater performance improvements on multi-core machines.
We can use the [GPUQVM](@ref pyqpanda3.core.core.GPUQVM) to accelerate quantum circuit simulation for the same circuit, and the usage is the same as with CPUQVM. However, CUDA must be installed and the GPU environment must be configured on the computer beforehand.

```python
from pyqpanda3.core import QCircuit,H,P,RX,RY,QProg,measure,GPUQVM
circuit = QCircuit(3)
circuit << H(0)
circuit << P(2, 0.2)
circuit << RX(1, 0.9)
circuit << RX(0, 0.5)
circuit << RX(1, 0.2)
circuit << RY(1, 0.3)
circuit << RY(2, 2.5)
circuit << RX(0, 1.5)

prog = QProg()
prog << circuit
prog << measure(0, 0)
prog << measure(1, 1)
prog << measure(2, 2)

machine = GPUQVM()
machine.run(prog, 1000)
measure_result = machine.result().get_counts()
print(measure_result)
```

The output is as follows:

```python
{'000': 19, '101': 327, '110': 122, '111': 175, '010': 9, '100': 323, '001': 14, '011': 11}
```

## Partial-Amplitude Quantum Simulator
Partial-amplitude simulators rely on results from other simulators that provide the amplitudes of low-qubit quantum circuits. They can simulate more qubits but with reduced circuit depth. These simulators are typically used to obtain partial amplitude simulation results of quantum states.

In recent years, significant progress has been made in semiconductor quantum chips. Quantum supremacy claims that if a device with 50 qubits is built, it will surpass the limits of classical computers (i.e., directly simulating 50 qubits would require approximately 16 PB of RAM to store the complete vector).

The Google and IBM teams have proposed effective methods to simulate low-depth circuits with more than 49 qubits (e.g., delayed entangling gates)

Here, we propose a method to optimize the classical simulation of low-depth quantum circuits with large sampling numbers, and we have simulated circuits with 64 qubits.

Specifically, the approach is to map the circuit onto multiple additional sub-circuits by transforming several controlled-Z (CZ) gates into measurement gates and single-qubit gates.

These sub-circuits consist of two blocks that are not entangled with each other, thereby transforming an N-qubit simulation problem into a set of N/2 circuit solving problems.

Our method is similar to the balanced cut of a two-dimensional grid: for a CZ gate, it is split into four sub-circuits, and then the results of all the sub-circuits are summed to reconstruct the final state.

Simulation Scheme Details
The basic idea behind the Partial-Amplitude Quantum Virtual Machine is to split a large qubit quantum program into several smaller qubit quantum programs, with each small quantum circuit being computed using a full-amplitude algorithm. The rule for splitting the quantum program is as follows: when a two-qubit gate crossing nodes appears in the quantum circuit, the quantum program is split. Specifically, the circuit is divided near the halfway point of the total number of qubits. For example, if the total number of qubits is 10, then the boundary is between qubit 4 and qubit 5.

When a two-qubit gate's control qubit and target qubit are located on opposite sides of the boundary, it is called a "cross-node." For example, if the total number of qubits is 10, a CNOT(1,5) is a cross-node, but CNOT(1,4) is not.

A CZ gate can be converted into a combination of two measurement gates and single-qubit gates as follows:

\f[ 
\mathrm{CZ} = P_0 \otimes I + P_1 \otimes Z 
\f]

where I is the identity matrix, and Z is the Pauli-Z matrix.

\f[ 
P_0 = \begin{bmatrix} 1 & 0 \\ 0 & 0 \end{bmatrix}, \quad P_1 = \begin{bmatrix} 0 & 0 \\ 0 & 1 \end{bmatrix}
\f]

For other two-qubit gates such as CR, iSWAP, and SQiSWAP, they can be converted into a combination of single-qubit gates and two-qubit gates that can be split, and the two-qubit gates are then split similarly.

We use [PartialAmplitudeQVM](@ref pyqpanda3.core.core.PartialAmplitudeQVM) to run the full-amplitude simulation algorithm. First, we construct the circuit, then run it using [run(prog)](@ref pyqpanda3.core.core.PartialAmplitudeQVM.run), and finally obtain the result via [get_state_vector(list)](@ref pyqpanda3.core.core.PartialAmplitudeQVM.get_state_vector).

```python
from pyqpanda3.core import QCircuit,H,RX,RY,CNOT,QProg,PartialAmplitudeQVM
circuit = QCircuit(3)
circuit << H(0)
circuit << RY(1, 0.3)
circuit << RY(2, 2.7)
circuit << RX(0, 1.5)
circuit << CNOT(0, 1)
circuit << CNOT(1, 2)

prog = QProg()
prog << circuit

machine = PartialAmplitudeQVM()

machine.run(prog)
measure_result = machine.get_state_vector(["0","1","2"])
print(measure_result)
```

The result is as follows.

```python
[(0.11203780214230272-0.1043740198556836j), (0.01693285765754904-0.015774590250509563j), (0.07543963588749045-0.0702792977322559j)]
```

### Density Matrix Simulator

For mixed states, it is difficult to fully represent the quantum state of the system with state vectors. Instead, we use the density matrix to describe it:

\f[
\rho = \sum_{i}^{n} p_i |\varphi_i\rangle \langle \varphi_i|
\f]

For pure states, this simplifies to:

\f[
\rho = |\varphi\rangle \langle \varphi|
\f]

Returning to the mixed state described earlier, its density matrix is:

\f[
\rho = \frac{1}{2} |0\rangle \langle 0| + \frac{1}{2} |1\rangle \langle 1| 
= \frac{1}{2} 
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
\f]

For the pure state\f$ |\psi\rangle = \frac{1}{\sqrt{2}}|0\rangle + \frac{1}{\sqrt{2}}|1\rangle \f$, its density matrix is:

\f[
\rho = |\psi\rangle \langle \psi| 
= \frac{1}{2} 
\begin{bmatrix}
1 & 1 \\
1 & 1
\end{bmatrix}
\f]

From the density matrices, we can see that the quantum states are completely different.

For pure states,\f$  \text{tr}(\rho^2) = 1 \f$, while for mixed states\f$  \text{tr}(\rho^2) < 1 . \text{tr}(A) \f$represents the trace of matrix\f$  A \f$, which is the sum of the diagonal elements in an\f$ n \f$-dimensional matrix.

### Unitary Matrix Logic Gates and Density Matrix Representation

For general unitary matrices, we can represent the evolution of the system's state using the density matrix:

\f[
\rho' = U \rho U^{\dagger}
\f]

Using the density matrix, we can represent the measurement result, where the probability of measuring outcome\f$  m  \f$is:

\f[
p(m) = \text{tr}(M_m^{\dagger} M_m \rho)
\f]

Here,\f$ M_m \f$is the measurement operator, also known as the projection operator. For example, if our computational basis is\f$ |0\rangle \f$, the projection operator onto\f$  |0\rangle \f$is \f$ ,|0\rangle \langle 0| \f$. Thus, the probability of measuring \f$ |0\rangle \f$is:

\f[
p(|0\rangle) = \text{tr}(|0\rangle \langle 0| |0\rangle \langle 0| \rho)
\f]

### Example Calculation for Mixed and Pure States

Using the earlier examples for the mixed state and pure state, we calculate the measurement results as follows:

#### Mixed State:

\f[
p(|0\rangle) = \text{tr}\left( 
\frac{1}{2} 
\begin{bmatrix}
1 & 0 \\
0 & 0
\end{bmatrix}
\begin{bmatrix}
1 & 0 \\
0 & 0
\end{bmatrix}
\begin{bmatrix}
1 & 0 \\
0 & 1
\end{bmatrix}
\right)
= \frac{1}{2}
\f]

#### Pure State:

\f[
p(|0\rangle) = \text{tr}\left( 
\frac{1}{2} 
\begin{bmatrix}
1 & 0 \\
0 & 0
\end{bmatrix}
\begin{bmatrix}
1 & 0 \\
0 & 0
\end{bmatrix}
\begin{bmatrix}
1 & 1 \\
1 & 1
\end{bmatrix}
\right)
= \frac{1}{2}
\f]

The density matrix provides an alternative way to represent quantum states. The density matrix simulator is used to solve the density matrix corresponding to quantum circuits, compute quantum state probability distributions, simulate noisy quantum circuits, and calculate Hamiltonian expectation values, among other tasks.

We use [DensityMatrixSimulator](@ref pyqpanda3.core.core.DensityMatrixSimulator) to simulate the density matrix of quantum states. First, we construct the circuit, then run it using [run(prog)](@ref pyqpanda3.core.core.DensityMatrixSimulator.run) as before.

```python
from pyqpanda3.core import QCircuit,H,RX,CP,CZ,QProg,DensityMatrixSimulator
circuit = QCircuit(2)
circuit << H(0)
circuit << H(1)
circuit << RX(0, 1.5)
circuit << CZ(0, 1)
circuit << CP(1, 0, 0.22)

prog = QProg()
prog << circuit

machine = DensityMatrixSimulator()

machine.run(prog)
density_matrix = machine.density_matrix()
print(density_matrix)

reduced_density_matrix = machine.reduced_density_matrix([0])
print(reduced_density_matrix)
```

density_matrix is used to obtain the density matrix of the entire system after evolution, while reduced_density_matrix is used to obtain the density matrix of the subsystem. The output is as follows:

```python

#density_matrix
[[ 0.25      +0.j          0.25      +0.j          0.25      +0.j
  -0.24397436+0.05455741j]
 [ 0.25      +0.j          0.25      +0.j          0.25      +0.j
  -0.24397436+0.05455741j]
 [ 0.25      +0.j          0.25      +0.j          0.25      +0.j
  -0.24397436+0.05455741j]
 [-0.24397436-0.05455741j -0.24397436-0.05455741j -0.24397436-0.05455741j
   0.25      +0.j        ]]

#reduced_density_matrix
[[0.5       +0.j         0.00602564+0.05455741j]
 [0.00602564-0.05455741j 0.5       +0.j        ]]
```

The density matrix also supports passing additional noise model parameters.

```python
from pyqpanda3.core import QCircuit,H,RX,CP,CNOT,QProg,DensityMatrixSimulator
from pyqpanda3.core import NoiseModel,pauli_x_error,pauli_z_error,GateType

circuit = QCircuit(2)
circuit << H(0)
circuit << CNOT(0, 1)
circuit << RX(0, 0.8)
circuit << RX(0, 1.2)
circuit << RX(0, 3.6)
circuit << CP(0, 1, 4.9)

prog = QProg()
prog << circuit

machine = DensityMatrixSimulator()

machine.run(prog)
density_matrix = machine.density_matrix()
print(density_matrix)

model = NoiseModel()
model.add_all_qubit_quantum_error(pauli_x_error(0.6),GateType.RX)
model.add_all_qubit_quantum_error(pauli_z_error(0.9),GateType.H)
model.add_all_qubit_quantum_error(pauli_z_error(0.66),GateType.RX)

machine.run(prog, model)
density_matrix = machine.density_matrix()
print(density_matrix)
```

The output is as follows:

```python

#density_matrix
[[ 0.44389147+0.00000000e+00j  0.        -1.57816659e-01j 
   0.        -1.57816659e-01j  0.08279125+4.36102334e-01j]
 [-0.        +1.57816659e-01j  0.05610853+0.00000000e+00j 
   0.05610853+0.00000000e+00j -0.15504739+2.94347591e-02j]
 [-0.        +1.57816659e-01j  0.05610853+0.00000000e+00j
   0.05610853+0.00000000e+00j -0.15504739+2.94347591e-02j]
 [ 0.08279125-4.36102334e-01j -0.15504739-2.94347591e-02j
  -0.15504739-2.94347591e-02j  0.44389147+1.38777878e-17j]]

#density_matrix with noise
[[ 4.26306051e-04+0.j          0.00000000e+00+0.01459354j
  -0.00000000e+00+0.01459354j  7.95113517e-05+0.00041883j]
 [-0.00000000e+00-0.01459354j  4.99573694e-01-0.j
   4.99573694e-01-0.j          1.43374574e-02-0.00272187j]
 [ 0.00000000e+00-0.01459354j  4.99573694e-01-0.j
   4.99573694e-01+0.j          1.43374574e-02-0.00272187j]
 [ 7.95113517e-05-0.00041883j  1.43374574e-02+0.00272187j
   1.43374574e-02+0.00272187j  4.26306051e-04+0.j        ]]
```

## Stabilizer

Superposition and entanglement are typical sources of quantum advantage. However, when the number of qubits  N  in the system increases, the number of quantum state coefficients grows exponentially with  N . This makes it impossible to use classical computers for traditional full-amplitude simulation, a problem known as the **exponential wall**.

Based on the **Gottesman-Knill theorem**, we know that for stabilizer circuits formed from a specific gate set, simulation can be performed in polynomial time. This means that, in certain circuits constructed from specific logic gates, it is possible to break the quantum exponential advantage and apply classical simulation to quantum circuits, enabling verification of quantum computer results. Furthermore, in future fault-tolerant quantum computers, redundancy will be necessary for error correction, which is currently unachievable for large-scale quantum circuits in existing quantum simulation frameworks.

To address this, we can take a different approach. Using **stabilizer** and the corresponding **Clifford** gate set simulators, we can efficiently exploit the polynomial-time simulation property to solve fault-tolerant quantum computing based on Pauli noise. Moreover, to extend this to general quantum computing, we can incorporate the theoretical properties of stabilizers into **Clifford+T** simulation. With a **Clifford+T** simulator, we can simulate large quantum circuits under the condition of few non-Clifford logic gates (since Clifford+T can approximate any logic gate).

### Principle Introduction

For a quantum state \f$  |\psi\rangle \f$ (typically referring to a pure state), if there exists a unitary matrix \f$ U \f$ such that\f$  U|\psi\rangle = |\psi\rangle \f$, then we say that \f$ |\psi\rangle \f$ is stabilized by \f$ U \f$, and \f$ U  \f$is a stabilizer of \f$ |\psi\rangle \f$, for example, \f$ Z|0\rangle = |0\rangle \f$.

It is clear that a quantum state can have multiple stabilizers. When there are multiple stabilizers, the product of these stabilizers is also a stabilizer:

\f[
Z_1 Z_2 X_1 X_2 |\psi\rangle = Z_1 Z_2 |\psi\rangle = |\psi\rangle
\f]

This multiplicative closure tells us that stabilizers form a **group**.

For a quantum state\f$  |\psi\rangle \f$, if each element in the unitary transformation group \f$ S  \f$is a stabilizer of \f$ |\psi\rangle \f$, then the entire unitary transformation group \f$ S  \f$is called the stabilizer group of \f$ |\psi\rangle \f$.

In general, we focus on the **Pauli** matrices \f$ \{ X, Y, Z, I \} \f$ as stabilizers. The unitary transformation group is thus formed from the Pauli group, defined as:

\f[
\text{Stab}(|\psi\rangle) = \{ P \in \mathcal{P}_n : P|\psi\rangle = |\psi\rangle \}
\f]

Here, the Pauli group \f$ \mathcal{P}_n \f$ is the set of Pauli operators acting on \f$ n  \f$qubits, where the phase coefficients are \f$ \pm 1 \f$ and \f$ \pm i \f$:

\f[
\mathcal{P}_n = \{ i^{\gamma} X(a) Z(b) : \gamma \in \{ 0, 1, 2, 3 \}, a, b \in \{0, 1\}^n \}
\f]

If \f$ P^{(1)}, \dots, P^{(m)} \in \mathcal{P}_n  \f$are independent elements of the Pauli group, then we can use the special properties of the Pauli group to obtain:

\f[
\begin{matrix}
\text{Stab}(|00\rangle) = \{ I, Z_1, Z_2, Z_1 Z_2 \} = \langle Z_1, Z_2 \rangle \\
\text{Stab}(|++\rangle) = \{ I, X_1, X_2, X_1 X_2 \} = \langle X_1, X_2 \rangle \\
\text{Stab}\left( \frac{|00\rangle + |11\rangle}{\sqrt{2}} \right) = \{ I, X_1 X_2, Z_1 Z_2, -Y_1 Y_2 \} = \langle X_1 X_2, Z_1 Z_2 \rangle \\
\text{Stab}(|0^n\rangle) = \{ Z(a) : a \in \{0, 1\}^n \} = \langle Z_1, \dots, Z_n \rangle
\end{matrix}
\f]

The problem is how to construct the **Stabilizer Group**. Here, we need to mention that when elements from the **Clifford Group** act on the Pauli group, they induce a transformation:

\f[
\mathbf{P|\psi\rangle = |\psi\rangle} \Longleftrightarrow \left( \mathbf{UPU^{\dagger}} \right) \mathbf{U|\psi\rangle = U|\psi\rangle}
\f]

When we write the group as \f$ P = i^{\gamma} X(a) Z(b) \f$, the action of the **Clifford Group** is as follows:

\f[
U_j P U_j^{\dagger} = i^{\gamma} X^{a_1} Z^{b_1} \otimes \ldots \otimes X^{a_{j-1}} Z^{b_{j-1}} \otimes U X^{a_j} Z^{b_j} U^{\dagger} \otimes X^{a_{j+1}} Z^{b_{j+1}} \otimes \ldots \otimes X^{a_n} Z^{b_n}
\f]

It is surprising to find that:

\f[
U_j P U_j^{\dagger} = P_{\text{new}}
\f]

This is illustrated in the following diagram:

| **Operation** | **Input**     | **Output**     | | **Operation** | **Input**     | **Output**     |
| :-----------: | :-----------: | :-----------:  | | :-----------: | :-----------: | :-----------:  |
| \f$ CNOT \f$  | \f$ X_1 \f$   | \f$ X_1X_2 \f$ | | \f$ X \f$     | \f$ X \f$     | \f$ X \f$      |
| ^             | \f$ X_2 \f$   | \f$ X_2 \f$    | | ^             | \f$ Z \f$     | \f$ -Z \f$     |
| ^             | \f$ Z_1 \f$   | \f$ Z_1 \f$    | | \f$ Y \f$     | \f$ X \f$     | \f$ -X \f$     |
| ^             | \f$ Z_2 \f$   | \f$ Z_1Z_2 \f$ | | ^             | \f$ Z \f$     | \f$ -Z \f$     |
| \f$ H \f$     | \f$ X \f$     | \f$ Z \f$      | | \f$ Z \f$     | \f$ X \f$     | \f$ -X \f$     |
| ^             | \f$ Z \f$     | \f$ X \f$      | | ^             | \f$ Z \f$     | \f$ Z \f$      |
| \f$ S \f$     | \f$ X \f$     | \f$ Y \f$      | | | | |
| ^             | \f$ Z \f$     | \f$ Z \f$      |^|^|^|^|

Here, we can see that the transformation of \f$ Y  \f$is removed because \f$ Y = IXZ \f$.

Thus, \f$ |\psi\rangle \rightarrow U|\psi\rangle  \f$is equivalent to tracking the evolution of the stabilizer. This allows us to obtain the complete dynamical information of the system.

\f[
S \rightarrow U S U^{\dagger}
\f]

By converting the problem of quantum gate evolution into the problem of updating the stabilizer group of a quantum state, the core idea of using **Stabilizer** to simulate quantum circuits is to represent quantum states using the **Stabilizer Group** rather than the amplitudes used in traditional simulators.

In other words, for stabilizer circuits formed from a specific gate set, we can simulate large quantum circuits in polynomial time, as long as the circuits are composed of gates from the Clifford quantum logic gate set and its derived set:\f$  H, S, X, Y, Z, CNOT, CY, CZ, \text{SWAP} \f$.

We use [Stabilizer](@ref pyqpanda3.core.core.Stabilizer) to run large-bit Clifford circuits. The usage is the same as with other simulators; after running with [run(prog, shots, noise_model)](@ref pyqpanda3.core.core.Stabilizer.run), we obtain the result via [result()](@ref pyqpanda3.core.core.Stabilizer.result).

```python
from pyqpanda3.core import random_qcircuit,measure,QProg,Stabilizer
prog = QProg()
prog << random_qcircuit([0,1,2,3,4,5 ], 20, ["H","X","Y","Z","S","CZ","CNOT","SWAP"])
prog << measure(0,0)
prog << measure(1,1)
prog << measure(2,2)

machine = Stabilizer()

machine.run(prog,1000)
measure_result = machine.result().get_counts()
print(measure_result)
```

Stabilizer can support Clifford circuit simulation for up to 5000 qubits. The output is as follows:

```python
{'011': 239, '001': 260, '010': 257, '000': 244}
```
