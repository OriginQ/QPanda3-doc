PTM  {#tutorial_table_of_content_QI_PTM}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_Chi}
@next_tutorial{tutorial_table_of_content_QI_HellingerDistance }

-------------------------------------------------------------------------------------------------------------------------------

# Introduction

Pauli Transfer Matrix (PTM) representation of a Quantum Channel.

The PTM representation of an 
n-qubit quantum channel  \f$\mathcal{E}\f$ is an 
n-qubit SuperOp \f$\mathcal{R}\f$ defined with respect to vectorization in the Pauli basis instead of column-vectorization. The elements of the PTM 
\f[
\mathcal{R}
\f]
are given by
\f[
R_{i, j}=\frac{1}{2^{n}} \operatorname{Tr}\left[P_{i} \mathcal{E}\left(P_{j}\right)\right]
\f]

where 
\f[
\left[P_{0}, P_{1}, \ldots, P_{4^{n}-1}\right]
\f]
is the \f$\mathcal{n}\f$ -qubit Pauli basis in lexicographic order.

Evolution of a DensityMatrix \f$\rho\f$

ρ with respect to the PTM is given by
\f[
    \left.|\mathcal{E}(\rho)\rangle_{P}=S_{P}|\rho\rangle\right\rangle_{P}
\f]

Please refer: C.J. Wood, J.D. Biamonte, D.G. Cory, Tensor networks and graphical calculus for open quantum systems, Quant. Inf. Comp. 15, 0579-0811 (2015).[arXiv:1111.6950 [quant-ph]](https://arxiv.org/abs/1111.6950)

# In QPanda3 Quantum Information

## Constructing a PTM object

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.PTM.__init__)

### From Choi

Generate another PTM object from a Choi object

Please refer to [Choi](#tutorial_table_of_content_QI_Choi)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Generate another PTM object from a Choi object
@end_toggle

Output
```bash
ptm: [[ 3.+0.j  0.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  0.+0.j -1.+0.j]]
```


### From Chi

Generate another PTM object from a Chi object

Please refer to [Chi](#tutorial_table_of_content_QI_Chi)


@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Generate another PTM object from a Chi object
@end_toggle

Output
```bash
ptm: [[ 3.+0.j  0.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  0.+0.j -1.+0.j]]
```

### From SuperOp

Generate another PTM object from a SuperOp object

Please refer to [SuperOp](#tutorial_table_of_content_QI_SuperOp)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Generate another PTM object from a SuperOp object
@end_toggle

Output
```bash
ptm: [[ 3.+0.j  0.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  0.+0.j -1.+0.j]]
```

### From Kraus

Generate another PTM object from a Kraus object

Please refer to [Kraus](#tutorial_table_of_content_QI_Kraus)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Generate another PTM object from a Kraus object
@end_toggle

Output
```bash
ptm: [[ 3.+0.j  0.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  0.+0.j -1.+0.j]]
```

### From PTM
Generate another PTM object from a PTM object

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Generate another PTM object from a PTM object
@end_toggle

Output
```bash
ptm2: [[ 3.+0.j  0.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  0.+0.j -1.+0.j]]
```

## Obtain internal data

### Input and output dim

Obtain the input dimension input_dim and output dimension output_dim of the quantum channel

[Here is API doc for PTM.get_input_dim](@ref pyqpanda3.quantum_info.quantum_info.PTM.get_input_dim)

[Here is API doc for PTM.get_output_dim](@ref pyqpanda3.quantum_info.quantum_info.PTM.get_output_dim)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Obtain the input dimension input_dim and output dimension output_dim of the quantum channel
@end_toggle

Output
```bash 
input_dim: 2
output_dim: 2
```

## Evolution of quantum states

[Here is API doc for PTM.evolve](@ref pyqpanda3.quantum_info.quantum_info.PTM.evolve)
### DensityMatrix

Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object

The dimension of the density matrix is obtained by the member method dim() and should be equal to the input dimension of the PTM object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix).

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object
@end_toggle

Output
```bash
res: [[ 1.30727832+0.j         -0.38338527+0.17772383j]
 [-0.38338527-0.17772383j  1.69272168+0.j        ]]
```
### StateVector

Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object

The dimension of the StateVector object is obtained by the member method dim() and should be equal to the input dimension of the PTM object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix)

Please refer to [StateVector](#tutorial_table_of_content_QI_StateVector)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object
@end_toggle

Output
```bash
res: [[1.+0.j 0.+0.j]
 [0.+0.j 2.+0.j]]
```

## Boolean function

### Equal
Determine whether the internal data of two PTM objects are equal

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.PTM.__eq__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\PTM.py Determine whether the internal data of two PTM objects are equal
@end_toggle

Output
```bash
True
```

