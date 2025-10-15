Quantum Circuit and Quantum Program {#tutorial_circuit_and_program}
=============================================================

[TOC]

@prev_tutorial{tutorial_quantum_gate}
@next_tutorial{tutorial_quantum_simulator}

-------------------------------------------------------------------------------------------------------------------------------
# Quantum Programs and Circuits Tutorial Introduction

Quantum circuits, or quantum logic circuits, serve as the foundational model for quantum computing. Unlike traditional circuits that utilize metal wires to transmit signals, quantum circuits operate on quantum bits (qubits) and are defined by the evolution of these qubits over time. This evolution is governed by the Hamiltonian operator, which directs the quantum state until it interacts with a logic gate.

## Quantum Circuits

At their core, quantum circuits are collections of various quantum logic gates, each represented as a unitary matrix. This means that every individual gate can be expressed mathematically as a matrix that preserves the norm of the quantum state. When these gates are combined, they form a larger unitary matrix that describes the overall operation of the circuit. Quantum circuits not only facilitate operations on qubits but also enable the measurement of their states, allowing us to read out results after computations.

## Quantum Programs

Quantum programming involves writing and constructing sequences of operations that manipulate qubits within these circuits. As quantum algorithms often incorporate classical computing elements, the future of quantum computing is expected to be hybrid. This hybrid architecture will blend classical computing, which handles control and non-quantum tasks, with quantum devices that perform quantum calculations.

In this context, QPanda3 views the programming of quantum circuits as part of classical program execution. This means that the process of creating and manipulating quantum programs is integrated into a broader classical framework, ensuring a seamless interaction between classical and quantum computing elements.

## Dynamic Quantum Circuits

Dynamic circuits introduce process control features such as branching and looping into classical quantum programs, enabling quantum programs to exhibit more complex computational logic.

- @subpage tutorial_quantum_circuit - Quantum Circuit
- @subpage tutorial_quantum_program - Quantum Program
- @subpage tutorial_dynamic_circuit - Dynamic Quantum Circuits