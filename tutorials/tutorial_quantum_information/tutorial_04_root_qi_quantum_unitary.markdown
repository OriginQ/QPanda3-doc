Unitary  {#tutorial_table_of_content_QI_Unitary}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_KL_divergence}
@next_tutorial{tutorial_table_of_content_QI_Matrix}

-------------------------------------------------------------------------------------------------------------------------------

# Application

## Unitary matrix of QCircuit
Obtain the unitary matrix of the quantum circuit QCircuit

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Unitary.__init__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Unitary.py Obtain the unitary matrix of the quantum circuit QCircuit
@end_toggle

Output
```bash
[[0.        +0.j         1.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [1.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         1.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  1.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         1.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         1.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.53761923-0.84318774j 0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.53761923-0.84318774j]]
```

# Obtain internal data

## Get numpy.ndarray
Get internal data, the result is returned as a numpy.ndarray object

[Here is API doc for Unitary.ndarray](@ref pyqpanda3.quantum_info.quantum_info.Unitary.ndarray)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Unitary.py Obtain internal data
@end_toggle

Output
```bash
U [[0.        +0.j         1.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [1.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         1.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  1.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         1.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         1.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.53761923-0.84318774j 0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.53761923-0.84318774j]]
arr: [[0.        +0.j         1.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [1.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         1.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  1.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         1.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         1.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.53761923-0.84318774j 0.        +0.j        ]
 [0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.        +0.j         0.        +0.j
  0.        +0.j         0.53761923-0.84318774j]]
```

# Equality comparison function

Determine whether the internal data of two Unitary objects are equal

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Unitary.__eq__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Unitary.py Determine whether the internal data of two Unitary objects are equal
@end_toggle

Output
```bash
U2==U: True
```