Operator  {#tutorial_operator}
=============================================================================

[TOC]

@prev_tutorial{tutorial_quantum_visualization}
@next_tutorial{tutorial_table_of_content_PauliOperator}

-------------------------------------------------------------------------------------------------------------------------------

# Operator

In the quantum field, Operator is a core concept, which is used to represent the mathematical operation of measuring or transforming physical quantities. Operators are not only important tools for interpreting and calculating the properties of quantum systems, but also the cornerstone of the formation and development of quantum mechanics theory.

## PauliOperator

As of this version, qpanda3 uses the PauliOperator class to provide simulation of basic Pauli operators and their combinations. PauliOperator supports qpanda2-style constructor and qiskit-style constructor. PauliOperator also supports addition, subtraction, scalar multiplication, multiplication, and tensor products. In addition, it also supports the expectation value calculation of quantum programs for quantum systems with Pauli operators.


- @subpage tutorial_table_of_content_PauliOperator - %QPanda3 PauliOperator