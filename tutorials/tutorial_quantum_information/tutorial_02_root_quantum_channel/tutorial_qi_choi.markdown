Choi  {#tutorial_table_of_content_QI_Choi}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_Kraus}
@next_tutorial{tutorial_table_of_content_QI_SuperOp}

-------------------------------------------------------------------------------------------------------------------------------

# Introduction

Choi-matrix representation of a Quantum Channel.

The Choi-matrix representation of a quantum channel \f$\mathcal{E}\f$ is a matrix
\f[
\Lambda=\sum_{i, j}|i\rangle\langle j| \otimes \mathcal{E}(|i\rangle\langle j|)
\f]

Evolution of a DensityMatrix \f$\rho\f$ with respect to the Choi-matrix is given by
\f[
\mathcal{E}(\rho)=\operatorname{Tr}_{1}\left[\Lambda\left(\rho^{T} \otimes \mathbb{I}\right)\right]
\f]

Please refer: C.J. Wood, J.D. Biamonte, D.G. Cory, Tensor networks and graphical calculus for open quantum systems, Quant. Inf. Comp. 15, 0579-0811 (2015).[arXiv:1111.6950 [quant-ph]](https://arxiv.org/abs/1111.6950)

# In QPanda3 Quantum Information

## Constructing a Choi object

 [Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Choi.__init__)

### From Choi
Generate another Choi object from a Choi object

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Generate another Choi object from a Choi object
@end_toggle

```bash
choi2: [[ 1.+0.j  0.+0.j  0.+0.j -1.+0.j]
 [ 0.+0.j  2.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  2.+0.j  0.+0.j]
 [-1.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```


### From Chi
Generate another Choi object from a Chi object

Please refer to [Chi](#tutorial_table_of_content_QI_Chi)


@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Generate another Choi object from a Chi object
@end_toggle

Output
```bash
choi: [[ 1.+0.j  0.+0.j  0.+0.j -1.+0.j]
 [ 0.+0.j  2.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  2.+0.j  0.+0.j]
 [-1.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```

### From SuperOp
Generate another Choi object from a SuperOp object
Please refer to [SuperOp](#tutorial_table_of_content_QI_SuperOp)


@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Generate another Choi object from a SuperOp object
@end_toggle

Output
```bash
choi: [[ 1.+0.j  0.+0.j  0.+0.j -1.+0.j]
 [ 0.+0.j  2.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  2.+0.j  0.+0.j]
 [-1.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```

### From Kraus
Generate another Choi object from a Kraus object

Please refer to [Kraus](#tutorial_table_of_content_QI_Kraus)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Generate another Kraus object from a SuperOp object
@end_toggle

Output
```bash
choi: [[ 1.+0.j  0.+0.j  0.+0.j -1.+0.j]
 [ 0.+0.j  2.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  2.+0.j  0.+0.j]
 [-1.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```

### From PTM
Generate another Choi object from a PTM object

Please refer to [PTM](#tutorial_table_of_content_QI_PTM)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Generate another Choi object from a PTM object
@end_toggle

Output
```bash
choi: [[ 1.+0.j  0.+0.j  0.+0.j -1.+0.j]
 [ 0.+0.j  2.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j  2.+0.j  0.+0.j]
 [-1.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```

## Obtain internal data

### Input and output dim
Obtain the input dimension input_dim and output dimension output_dim of the quantum channel

The shape of the Choi matrix is（input_dim * output_dim,input_dim * output_dim）

[Here is API doc for Choi.get_input_dim](@ref pyqpanda3.quantum_info.quantum_info.Choi.get_input_dim)

[Here is API doc for Choi.get_output_dim](@ref pyqpanda3.quantum_info.quantum_info.Choi.get_output_dim)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Obtain the input dimension input_dim and output dimension output_dim of the quantum channel
@end_toggle

Output
```bash
input_dim: 2
output_dim: 2
```


## Evolution of quantum states

[Here is API doc for Choi.evolve](@ref pyqpanda3.quantum_info.quantum_info.Choi.evolve)

### DensityMatrix
Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object

The dimension of the density matrix is obtained by the member method dim() and should be equal to the input dimension of the Choi object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix).

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object
@end_toggle

Output
```bash
res: [[ 1.30727832+0.j         -0.38338527+0.17772383j]
 [-0.38338527-0.17772383j  1.69272168+0.j        ]]
```

### StateVector
Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object

The dimension of the StateVector object is obtained by the member method dim() and should be equal to the input dimension of the Choi object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix)

Please refer to [StateVector](#tutorial_table_of_content_QI_StateVector)


@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object
@end_toggle
  
Output
```bash
res: [[1.+0.j 0.+0.j]
 [0.+0.j 2.+0.j]]
```

## Boolean function

### Equal

Determine whether the internal data of two Choi objects are equal

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.Choi.__eq__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\Choi.py Determine whether the internal data of two Choi objects are equal
@end_toggle
  
Output
```bash
True
```