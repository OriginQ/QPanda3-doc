Chi  {#tutorial_table_of_content_QI_Chi}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_SuperOp}
@next_tutorial{tutorial_table_of_content_QI_PTM}

-------------------------------------------------------------------------------------------------------------------------------

# Introduction

Pauli basis Chi-matrix representation of a quantum channel.

The Chi-matrix representation of an n-qubit quantum channel \f$\mathcal{E}\f$ is a matrix χ such that the evolution of a DensityMatrix \f$\rho\f$ is given by 
\f[
\mathcal{E}(\rho)=\frac{1}{2^{n}} \sum_{i, j} \chi_{i, j} P_{i} \rho P_{j}
\f]

where 
\f[
\left[P_{0}, P_{1}, \ldots, P_{4^{n}-1}\right]
\f]
is the n-qubit Pauli basis in lexicographic order. It is related to the Choi representation by a change of basis of the Choi-matrix into the Pauli basis. The 
\f[
\frac{1}{2^{n}}
\f]
 in the definition above is a normalization factor that arises from scaling the Pauli basis to make it orthonormal.

 Please refer: C.J. Wood, J.D. Biamonte, D.G. Cory, Tensor networks and graphical calculus for open quantum systems, Quant. Inf. Comp. 15, 0579-0811 (2015). [arXiv:1111.6950 [quant-ph]](https://arxiv.org/abs/1111.6950)


# In QPanda3 Quantum Information

## Constructing a Chi object

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Chi.__init__)

### From Choi
Generate another Chi object from a Choi object

Please refer to [Choi](#tutorial_table_of_content_QI_Choi)


@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Generate another Chi object from a Choi object
@end_toggle

Output
```bash
chi: [[0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 2.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 2.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 2.+0.j]]
```


### From Chi
Generate another Chi object from a Chi object

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Generate another Chi object from a Chi object
@end_toggle

Output
```bash
chi2: [[0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 2.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 2.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 2.+0.j]]
```

### From SuperOp
Generate another Chi object from a SuperOp object

Please refer to [SuperOp](#tutorial_table_of_content_QI_SuperOp)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Generate another Chi object from a SuperOp object
@end_toggle

Output
```bash
chi: [[0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 2.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 2.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 2.+0.j]]
```

### From Kraus
Generate another Chi object from a Kraus object

Please refer to [Kraus](#tutorial_table_of_content_QI_Kraus)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Generate another Chi object from a Kraus object
@end_toggle

Output
```bash
chi: [[0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 2.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 2.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 2.+0.j]]
```

### From PTM
Generate another SuperOp object from a PTM object

Please refer to [PTM](#tutorial_table_of_content_QI_PTM)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Generate another Chi object from a PTM object
@end_toggle

Output
```bash
chi: [[0.+0.j 0.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 2.+0.j 0.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 2.+0.j 0.+0.j]
 [0.+0.j 0.+0.j 0.+0.j 2.+0.j]]
```

## Obtain internal data

### Input and outut dim
Obtain the input dimension input_dim and output dimension output_dim of the quantum channel

[Here is API doc for Chi.get_input_dim](@ref pyqpanda3.quantum_info.quantum_info.Chi.get_input_dim)

[Here is API doc for Chi.get_output_dim](@ref pyqpanda3.quantum_info.quantum_info.Chi.get_output_dim)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Generate another Obtain the input dimension input_dim and output dimension output_dim of the quantum channel
@end_toggle

Output
```bash
input_dim: 2
output_dim: 2
```


## Evolution of quantum states

[Here is API doc for Chi.evolve](@ref pyqpanda3.quantum_info.quantum_info.Chi.evolve)

### DensityMatrix

Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object. The dimension of the density matrix is obtained by the member method dim() and should be equal to the input dimension of the Chi object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix).

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object
@end_toggle

Output
```bash
res: [[ 1.30727832+0.j         -0.38338527+0.17772383j]
 [-0.38338527-0.17772383j  1.69272168+0.j        ]]
```
### StateVector

Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object. The dimension of the StateVector object is obtained by the member method dim() and should be equal to the input dimension of the Chi object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix)

Please refer to [StateVector](#tutorial_table_of_content_QI_StateVector)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object
@end_toggle

output
```
res: [[1.+0.j 0.+0.j]
 [0.+0.j 2.+0.j]]
```

## Boolean function

### Equal
Determine whether the internal data of two Chi objects are equal

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Chi.__eq__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Chi.py Determine whether the internal data of two Chi objects are equal
@end_toggle

Output
```bash
True
```



