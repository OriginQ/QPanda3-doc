Kraus  {#tutorial_table_of_content_QI_Kraus}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_StateVector}
@next_tutorial{tutorial_table_of_content_QI_Choi}

-------------------------------------------------------------------------------------------------------------------------------
# Introduction
Kraus representation of a quantum channel.

For a quantum channel
\f[
    \mathcal{E}(\rho)=\sum_{i=0}^{K-1} A_{i} \rho A_{i}^{\dagger}
\f]
    
, the Kraus representation is given by a set of matrices 
\f[
    \left[A_{0}, \ldots, A_{K-1}\right]
\f]
such that the evolution of a DensityMatrix \f$ \rho\f$ is given by

\f[
\mathcal{E}(\rho)=\sum_{i=0}^{K-1} A_{i} \rho A_{i}^{\dagger}
\f]

A general operator map \f$\mathcal{G}\f$ can also be written using the generalized Kraus representation which is given by two sets of matrices 
\f$
\left[A_{0}, \ldots, A_{K-1}\right]
\f$ , \f$\left[B_{0}, \ldots, B_{B-1}\right]
\f$
such that
\f[
 \mathcal{G}(\rho)=\sum_{i=0}^{K-1} A_{i} \rho B_{i}^{\dagger}
\f]

Please refer: C.J. Wood, J.D. Biamonte, D.G. Cory, Tensor networks and graphical calculus for open quantum systems, Quant. Inf. Comp. 15, 0579-0811 (2015).[arXiv:1111.6950 [quant-ph]](https://arxiv.org/abs/1111.6950)

# In QPanda3 Quantum Information

## Constructing a Kraus object

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Kraus.__init__)

### Default constructor
Generate a Kraus object without elements

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Generate a Kraus object without elements
@end_toggle

Output
```bash
Kra1: [Kraus:
[Kraus_left:
]
[Kraus_right:
]
]
```

### From 2D matrix array

Construct a Kraus object from the array of the 2-dimensional matrix of the operator list. 

The left operator list of the generated Kraus object is constructed from the input matrix array, and the right operator list is empty

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 2
@end_toggle

Output
```bash
Kra2: [Kraus:
[Kraus_left:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j -0+-1j]
[0+1j 0+0j]
] 
]
[Kraus_right:
]
]
```

### From 2 operator lists

Construct a Kraus object from the array of two operator lists in a 2-dimensional matrix

The left operator list of the generated Kraus object is constructed from the first input array

The right operator list of the generated Kraus object is constructed from the second input array

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 3
@end_toggle

Output
```bash
Kra3: [Kraus:
[Kraus_left:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j -0+-1j]
[0+1j 0+0j]
] 
]
[Kraus_right:
[ [0+0j -0+-1j]
[0+1j 0+0j]
]  
[ [1+0j 0+0j]
[0+0j -1+0j]
]  
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
]
```

### From 2D matrix

Construct Kraus objects from a 2-dimensional matrix of operators

The left operator list of the generated Kraus object is constructed from the input

The right operator list of the generated Kraus object is empty

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 4
@end_toggle

Output
```bash
Kra4: [Kraus:
[Kraus_left:
[ [0+0j ]
[1+0j ]
] 
[ [1+0j ]
[0+0j ]
] 
]
[Kraus_right:
]
]
```

### From Kraus

Generate another Kraus object from a Kraus object using the copy constructor

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 5
@end_toggle

Output
```bash
Kra5: [Kraus:
[Kraus_left:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j -0+-1j]
[0+1j 0+0j]
] 
]
[Kraus_right:
[ [0+0j -0+-1j]
[0+1j 0+0j]
]  
[ [1+0j 0+0j]
[0+0j -1+0j]
]  
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
]
```

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 5_2
@end_toggle

Output
```bash
Kra5: [Kraus:
[Kraus_left:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j -0+-1j]
[0+1j 0+0j]
] 
]
[Kraus_right:
[ [0+0j -0+-1j]
[0+1j 0+0j]
]  
[ [1+0j 0+0j]
[0+0j -1+0j]
]  
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
]
```

### From Choi

Generate another Kraus object from a Choi object

Please refer to [Choi](#tutorial_table_of_content_QI_Choi)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 6
@end_toggle

Output
```bash 
Kra4: [Kraus:
[Kraus_left:
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j 1.4142135623731+0j]
[0+0j 0+0j]
] 
[ [0+0j 0+0j]
[1.4142135623731+0j 0+0j]
] 
]
[Kraus_right:
]
]
```

### From Chi

Generate another Kraus object from a Chi object

Please refer to [Chi](#tutorial_table_of_content_QI_Chi)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 7
@end_toggle

Output
```bash
Kra4: [Kraus:
[Kraus_left:
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j 1.4142135623731+0j]
[0+0j 0+0j]
] 
[ [0+0j 0+0j]
[1.4142135623731+0j 0+0j]
] 
]
[Kraus_right:
]
]
```

### From SuperOp

Generate another Kraus object from a SuperOp object

Please refer to [SuperOp](#tutorial_table_of_content_QI_SuperOp)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 8
@end_toggle

Output
```bash
Kra4: [Kraus:
[Kraus_left:
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j 1.4142135623731+0j]
[0+0j 0+0j]
] 
[ [0+0j 0+0j]
[1.4142135623731+0j 0+0j]
] 
]
[Kraus_right:
]
]
```

### From PTM

Generate another Kraus object from a PTM object
  
Please refer to [PTM](#tutorial_table_of_content_QI_PTM)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Constructing a Kraus object 9
@end_toggle

Output
```bash
Kra4: [Kraus:
[Kraus_left:
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j 1.4142135623731+0j]
[0+0j 0+0j]
] 
[ [0+0j 0+0j]
[1.4142135623731+0j 0+0j]
] 
]
[Kraus_right:
]
]

```
## Obtain internal data

### Input and output dim

Get the input dimension input_dim and output dimension output_dim corresponding to the Kraus object

The output dimension should be equal to the number of rows in the matrix

The input dimension should be equal to the number of columns in the matrix

Currently, the implementation requires that the matrix must be a square matrix, so the input and output dimensions should be equal

[Here is API doc for Kraus.get_input_dim](@ref pyqpanda3.quantum_info.quantum_info.Kraus.get_input_dim)

[Here is API doc for Kraus.get_output_dim](@ref pyqpanda3.quantum_info.quantum_info.Kraus.get_output_dim)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Obtain internal data
@end_toggle

Output
```bash
Kra3: [Kraus:
[Kraus_left:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j -0+-1j]
[0+1j 0+0j]
] 
]
[Kraus_right:
[ [0+0j -0+-1j]
[0+1j 0+0j]
]  
[ [1+0j 0+0j]
[0+0j -1+0j]
]  
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
]

input_dim: 2
output_dim: 2
```

### Left operators

Get the list of left operators inside the Kraus object

The result is returned in the form of list[pyqpanda3.quantum_info.Matrix]

[Here is API doc for Kraus.left](@ref pyqpanda3.quantum_info.quantum_info.Kraus.left)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Get the list of left operators inside the Kraus object
@end_toggle

Output
```bash
[[0.+0.j 1.+0.j]
 [1.+0.j 0.+0.j]]
[[ 1.+0.j  0.+0.j]
 [ 0.+0.j -1.+0.j]]
[[ 0.+0.j -0.-1.j]
 [ 0.+1.j  0.+0.j]]
```
### Right operators

Get the list of right operators inside the Kraus object

The result is returned in the form of list[pyqpanda3.quantum_info.Matrix]

[Here is API doc for Kraus.right](@ref pyqpanda3.quantum_info.quantum_info.Kraus.right)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Get the list of right operators inside the Kraus object
@end_toggle

Output
```bash
[[ 0.+0.j -0.-1.j]
 [ 0.+1.j  0.+0.j]]
[[ 1.+0.j  0.+0.j]
 [ 0.+0.j -1.+0.j]]
[[0.+0.j 1.+0.j]
 [1.+0.j 0.+0.j]]
```

## Modify internal data

### Clear internal data

[Here is API doc for Kraus.clear](@ref pyqpanda3.quantum_info.quantum_info.Kraus.clear)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Clear internal data
@end_toggle

Output
```bash
Kra3: [Kraus:
[Kraus_left:
]
[Kraus_right:
]
]
```
### Append Kraus

Append the internal data of another Kraus object to the end of the internal data of the current Kraus object

The matrix inside the other Kraus object should have the same shape as the current Kraus object's internal matrix

[Here is API doc for Kraus.append](@ref pyqpanda3.quantum_info.quantum_info.Kraus.append)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Append the internal data of another Kraus object to the end of the internal data of the current Kraus object
@end_toggle

Output
```bash
Kra3: [Kraus:
[Kraus_left:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j -0+-1j]
[0+1j 0+0j]
] 
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
[ [1+0j 0+0j]
[0+0j -1+0j]
] 
[ [0+0j -0+-1j]
[0+1j 0+0j]
] 
]
[Kraus_right:
[ [0+0j -0+-1j]
[0+1j 0+0j]
]  
[ [1+0j 0+0j]
[0+0j -1+0j]
]  
[ [0+0j 1+0j]
[1+0j 0+0j]
]  
[ [0+0j -0+-1j]
[0+1j 0+0j]
]  
[ [1+0j 0+0j]
[0+0j -1+0j]
]  
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
]
```

### Add Matrix object to left

Add the data in the Matrix object to the end of the internal left operator list of the Kraus object

The Matrix object should have the same shape as the internal matrix of the current Kraus object

[Here is API doc for Kraus.left_push_back](@ref pyqpanda3.quantum_info.quantum_info.Kraus.left_push_back)


@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Add the data in the Matrix object to the end of the internal left operator list of the Kraus object
@end_toggle

Output
```bash
Kra3: [Kraus:
[Kraus_left:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
[Kraus_right:
]
]
```

### Add Matrix object to right

Add the data in the Matrix object to the end of the internal right operator list of the Kraus object

The Matrix object should have the same shape as the internal matrix of the current Kraus object

[Here is API doc for Kraus.right_push_back](@ref pyqpanda3.quantum_info.quantum_info.Kraus.right_push_back)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Add the data in the Matrix object to the end of the internal right operator list of the Kraus object
@end_toggle

Output
```bash
Kra3: [Kraus:
[Kraus_left:
]
[Kraus_right:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
]
```

### Append numpy.ndarray to left

Append the data from the numpy.ndarray object representing a two-dimensional array to the end of the internal left operator list within the Kraus object

The numpy.ndarray object representing a two-dimensional array should have the same shape as the internal matrix of the current Kraus object

[Here is API doc for Kraus.left_push_back](@ref pyqpanda3.quantum_info.quantum_info.Kraus.left_push_back)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Append the data from the numpy.ndarray object representing a two-dimensional array to the end of the internal left operator list within the Kraus object
@end_toggle

Output
```bash
Kra3: [Kraus:
[Kraus_left:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
[Kraus_right:
]
]
```

### Append numpy.ndarray to right

Append the data from the numpy.ndarray object representing a two-dimensional array to the end of the internal right operator list within the Kraus object

The numpy.ndarray object representing a two-dimensional array should have the same shape as the internal matrix of the current Kraus object

[Here is API doc for Kraus.right_push_back](@ref pyqpanda3.quantum_info.quantum_info.Kraus.right_push_back)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Append the data from the numpy.ndarray object representing a two-dimensional array to the end of the internal right operator list within the Kraus object operator list within the Kraus object
@end_toggle

Output
```bash
Kra3: [Kraus:
[Kraus_left:
]
[Kraus_right:
[ [0+0j 1+0j]
[1+0j 0+0j]
] 
]
]
```

## Evolution of quantum states

[Here is API doc for Kraus.evolve](@ref pyqpanda3.quantum_info.quantum_info.Kraus.evolve)

### DensityMatrix

Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object

The dimension of the density matrix is obtained by the member method dim() and should be equal to the input dimension of the Kraus object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix).

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Evolution of quantum states
@end_toggle

Output
```bash
res: [[1.49794191+0.j         0.19272168+0.17772383j]
 [0.19272168-0.17772383j 1.50205809+0.j        ]]
```
### StateVector

Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object

The dimension of the StateVector object is obtained by the member method dim() and should be equal to the input dimension of the Kraus object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix).

Please refer to [StateVector](#tutorial_table_of_content_QI_StateVector)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object
@end_toggle

Output
```bash
res: [[0.5+0.j 0.5+0.j]
 [0.5+0.j 2.5+0.j]]
```

## Boolean function

### Equal

Determine whether the internal data of two Kraus objects are equal

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Kraus.__eq__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Kraus.py Determine whether the internal data of two Kraus objects are equal
@end_toggle

Output
```bash
True
```