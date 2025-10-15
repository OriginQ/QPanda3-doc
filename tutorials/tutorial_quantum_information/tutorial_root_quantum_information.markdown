Quantum Information  {#tutorial_quantum_information}
=============================================================================

[TOC]

@prev_tutorial{tutorial_variational_quantum_circuit}
@next_tutorial{tutorial_hamiltonian}

-------------------------------------------------------------------------------------------------------------------------------

# Quantum Information

As of this version, qpanda3 provides the abstraction and simulation of quantum information such as quantum states and quantum channels, and provides some tools to facilitate researchers' analysis and processing of relevant information.

## Quantum State

In quantum mechanics, the quantum state is a fundamental concept that describes the state of a quantum system. The state of a quantum system can be represented in two primary ways: state vectors and density matrices.

## Quantum Channel

In the field of quantum information and quantum computing, quantum channels are mathematical representations of the transformations that a quantum system can undergo. They are crucial for describing the evolution of quantum states, the transmission of quantum information, and various quantum operations. In the current version, QPanda3 realizes five quantum channel representations—Kraus, Choi, SuperOp, Chi, and PTM—through five classes.

## Analysis Tools

In the current version, QPanda3 provides tools for calculating the Hellinger distance, Hellinger divergence, and KL divergence.

## Other

QPanda3 provides the Unitary class to obtain the unitary matrix corresponding to the quantum circuit QCircuit

QPanda3 utilizes the Matrix class to provide an abstraction for arrays of complex numbers, supporting functionalities for obtaining the transpose matrix, adjoint matrix, and L2 norm.

- @subpage tutorial_table_of_content_QI_QuantumState - %QPanda3 Quantum State
- @subpage tutorial_table_of_content_QI_QuantumChannel - %QPanda3 Quantum Channel
- @subpage tutorial_table_of_content_QI_QuantumAnalysis - %QPanda3 Quantum Analysis
- @subpage tutorial_table_of_content_QI_Unitary  - %QPanda3 Quantum Unitary
- @subpage tutorial_table_of_content_QI_Matrix  - %QPanda3 Matrix
