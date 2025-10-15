Quantum Channel  {#tutorial_table_of_content_QI_QuantumChannel}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_StateVector}
@next_tutorial{tutorial_table_of_content_QI_Kraus}

-------------------------------------------------------------------------------------------------------------------------------

# Quantum Channel

In the field of quantum information and quantum computing, quantum channels are mathematical representations of the transformations that a quantum system can undergo. They are crucial for describing the evolution of quantum states, the transmission of quantum information, and various quantum operations. In the current version, QPanda3 realizes five quantum channel representations—Kraus, Choi, SuperOp, Chi, and PTM—through five classes.

## Kraus

The Kraus representation, also known as the operator-sum representation, is a useful way to describe the noise phenomenology in quantum systems

## Choi

The Choi representation, also known as the Choi-Jamiolkowski isomorphism, is a way to represent a quantum channel as a matrix. 

## SuperOp

The SuperOp representation, also known as the Liouville superoperator representation, is a way to represent a quantum channel as a linear map acting on the vectorized density matrices.

## Chi

The Chi representation is another way to represent a quantum channel as a matrix. In this representation. The Chi representation is useful for analyzing certain properties of quantum channels, such as their entanglement-breaking nature, and for performing quantum process tomography in certain experimental setups.

## PTM

PTM is a matrix that describes the impact of a quantum channel on the input quantum state. Specifically, it represents how the quantum channel maps the Pauli operator of the input quantum state to the Pauli operator of the output quantum state.

- @subpage tutorial_table_of_content_QI_Kraus - %QPanda3 Kraus
- @subpage tutorial_table_of_content_QI_Choi - %QPanda3 Choi
- @subpage tutorial_table_of_content_QI_SuperOp - %QPanda3 SuperOp
- @subpage tutorial_table_of_content_QI_Chi - %QPanda3 Chi
- @subpage tutorial_table_of_content_QI_PTM - %QPanda3 PTM