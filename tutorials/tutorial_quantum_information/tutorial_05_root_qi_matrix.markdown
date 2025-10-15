Matrix  {#tutorial_table_of_content_QI_Matrix}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_Unitary}
@next_tutorial{tutorial_quantum_information}

-------------------------------------------------------------------------------------------------------------------------------


Abstraction of complex matrix

# Constructing a Matrix object

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Matrix.__init__)

## Default constructor

@add_toggle_python
    @snippet samples\python\QuantumInformation\Matrix.py Default constructor
@end_toggle

Output
```bash
mat []
```

## From numpy.ndarray
Construct based on the passed numpy.ndarray object storing a two-dimensional matrix


@add_toggle_python
    @snippet samples\python\QuantumInformation\Matrix.py Construct based on the passed numpy.ndarray object storing a two-dimensional matrix
@end_toggle

Output
```bash
m [[0.6650874 +0.j 0.68517404+0.j 0.98889109+0.j 0.04471197+0.j]
 [0.64401864+0.j 0.39456866+0.j 0.96256409+0.j 0.53353689+0.j]
 [0.72914706+0.j 0.01258831+0.j 0.46879808+0.j 0.41446852+0.j]]
```

# Obtain internal data

## Get numpy.ndarray
Get internal data, the result is returned as a numpy.ndarray object

[Here is API doc for Matrix.ndarray](@ref pyqpanda3.quantum_info.quantum_info.Matrix.ndarray)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Matrix.py Get internal data, the result is returned as a numpy.ndarray object
@end_toggle

Output
```bash
data_ [[0.03511921+0.j 0.75430961+0.j 0.19238195+0.j 0.19057673+0.j]
 [0.61574108+0.j 0.89369063+0.j 0.81701418+0.j 0.73896602+0.j]
 [0.21010302+0.j 0.35964433+0.j 0.41305091+0.j 0.44447268+0.j]]
```

## Get row total and col total
Get the total number of rows and columns of the matrix

[Here is API doc for Matrix.row_total](@ref pyqpanda3.quantum_info.quantum_info.Matrix.row_total)

[Here is API doc for Matrix.col_total](@ref pyqpanda3.quantum_info.quantum_info.Matrix.col_total)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Matrix.py Get the total number of rows and columns of the matrix
@end_toggle

Output
```bash
row total: 3
col total: 4
```

## Get Element
Get the value of the matrix element by row index and column index

[Here is API doc for Matrix.at](@ref pyqpanda3.quantum_info.quantum_info.Matrix.at)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Matrix.py Get the value of the matrix element by row index and column index
@end_toggle

Output
```bash
m:
(0.5387399873890723+0j),(0.5946435956404229+0j),(0.8662388940809534+0j),(0.7465917268170127+0j),
(0.6650096219471038+0j),(0.10914316657637124+0j),(0.5999827262878416+0j),(0.5121762859881978+0j),
(0.43882836608920894+0j),(0.1639091992188828+0j),(0.4683573326093392+0j),(0.9735070812593357+0j),
```


# Application

## Hermitian
Determine whether the current matrix is Hermitian 
using [is_hermitian()](@ref pyqpanda3.quantum_info.quantum_info.Matrix.is_hermitian)

## Get transpose matrix
Get the transpose matrix of the current matrix 
using [transpose()](@ref pyqpanda3.quantum_info.quantum_info.Matrix.transpose) or [T()](@ref pyqpanda3.quantum_info.quantum_info.Matrix.T)

## Get adjoint matrix
Get the adjoint matrix of the current matrix 
using [hermitian_conjugate()](@ref pyqpanda3.quantum_info.quantum_info.Matrix.hermitian_conjugate) or [adjoint()](@ref pyqpanda3.quantum_info.quantum_info.Matrix.adjoint)

## Get L2
Get the L2 norm of the current matrix using [L2()](@ref pyqpanda3.quantum_info.quantum_info.Matrix.L2)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Matrix.py Application
@end_toggle

Output
```bash
is_hermitian False
transpose [[0.9341786 +0.j 0.2134688 +0.j 0.40507989+0.j]
 [0.49122518+0.j 0.45268175+0.j 0.10207822+0.j]
 [0.062246  +0.j 0.65736666+0.j 0.15927297+0.j]
 [0.        +0.j 0.        +0.j 0.        +0.j]]
T [[0.9341786 +0.j 0.2134688 +0.j 0.40507989+0.j]
 [0.49122518+0.j 0.45268175+0.j 0.10207822+0.j]
 [0.062246  +0.j 0.65736666+0.j 0.15927297+0.j]
 [0.        +0.j 0.        +0.j 0.        +0.j]]
hermitian_conjugate [[0.9341786 +0.j 0.2134688 +0.j 0.40507989+0.j]
 [0.49122518+0.j 0.45268175+0.j 0.10207822+0.j]
 [0.062246  +0.j 0.65736666+0.j 0.15927297+0.j]
 [0.46341251+0.j 0.80009271+0.j 0.31558019+0.j]]
adjoint [[0.9341786 +0.j 0.2134688 +0.j 0.40507989+0.j]
 [0.49122518+0.j 0.45268175+0.j 0.10207822+0.j]
 [0.062246  +0.j 0.65736666+0.j 0.15927297+0.j]
 [0.46341251+0.j 0.80009271+0.j 0.31558019+0.j]]
L2 (1.5595857384665406+0j)
```


# Boolean function

## Equal
Determine whether the internal data of two Matrix objects are equal

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Matrix.__eq__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Matrix.py Determine whether the internal data of two Matrix objects are equal
@end_toggle

Output
```bash
True
```