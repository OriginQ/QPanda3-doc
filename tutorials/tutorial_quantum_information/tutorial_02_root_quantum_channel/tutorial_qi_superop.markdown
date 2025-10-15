SuperOp  {#tutorial_table_of_content_QI_SuperOp}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_Choi}
@next_tutorial{tutorial_table_of_content_QI_Chi}

-------------------------------------------------------------------------------------------------------------------------------

# Introduction

Superoperator representation of a quantum channel.

The Superoperator representation of a quantum channel 
\f$\mathcal{E}\f$ is a matrix 
\f$\mathcal{S}\f$ such that the evolution of a DensityMatrix 
\f$\rho\f$ is given by
\f[
|\mathcal{E}(\rho)\rangle\rangle=S|\rho\rangle\rangle
\f]

Please refer: C.J. Wood, J.D. Biamonte, D.G. Cory, Tensor networks and graphical calculus for open quantum systems, Quant. Inf. Comp. 15, 0579-0811 (2015).[arXiv:1111.6950 [quant-ph]](https://arxiv.org/abs/1111.6950)

# In QPanda3 Quantum Information

## Constructing a SuperOp object

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.SuperOp.__init__)

### From Choi

Generate another SuperOp object from a Choi object

Please refer to [Choi](#tutorial_table_of_content_QI_Choi)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Generate another SuperOp object from a Choi object
@end_toggle

Output
```bash
sop: [[ 1.+0.j  0.+0.j  0.+0.j  2.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 2.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```


### From Chi

Generate another SuperOp object from a Chi object

Please refer to [Chi](#tutorial_table_of_content_QI_Chi)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Generate another SuperOp object from a Chi object
@end_toggle

Output
```bash
sop: [[ 1.+0.j  0.+0.j  0.+0.j  2.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 2.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```

### From SuperOp

Generate another SuperOp object from a SuperOp object

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Generate another SuperOp object from a SuperOp object
@end_toggle

Output
```bash
sop2: [[ 1.+0.j  0.+0.j  0.+0.j  2.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 2.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```

### From Kraus

Generate another SuperOp object from a Kraus object

Please refer to [Kraus](#tutorial_table_of_content_QI_Kraus)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Generate another SuperOp object from a Kraus object
@end_toggle

Output
```bash
sop: [[ 1.+0.j  0.+0.j  0.+0.j  2.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 2.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```

### From PTM

Generate another SuperOp object from a PTM object

Please refer to [PTM](#tutorial_table_of_content_QI_PTM)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Generate another SuperOp object from a PTM object
@end_toggle

Output
```bash
sop: [[ 1.+0.j  0.+0.j  0.+0.j  2.+0.j]
 [ 0.+0.j -1.+0.j  0.+0.j  0.+0.j]
 [ 0.+0.j  0.+0.j -1.+0.j  0.+0.j]
 [ 2.+0.j  0.+0.j  0.+0.j  1.+0.j]]
```

## Obtain internal data

### Input and output dim

Obtain the input dimension input_dim and output dimension output_dim of the quantum channel

The shape of the SuperOp matrix is（output_dim * output_dim,input_dim * input_dim）

[Here is API doc for SuperOp.get_input_dim](@ref pyqpanda3.quantum_info.quantum_info.SuperOp.get_input_dim)

[Here is API doc for SuperOp.get_output_dim](@ref pyqpanda3.quantum_info.quantum_info.SuperOp.get_output_dim)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Obtain the input dimension input_dim and output dimension output_dim of the quantum channel
@end_toggle

Output
```bash
input_dim: 2
output_dim: 2
```

## Evolution of quantum states

[Here is API doc for SuperOp.evolve](@ref pyqpanda3.quantum_info.quantum_info.SuperOp.evolve)

### DensityMatrix

Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object

The dimension of the density matrix is obtained by the member method dim() and should be equal to the input dimension of the SuperOp object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix).

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Evolution of a DensityMatrix object, and the evolution result is returned as a DensityMatrix object
@end_toggle

Output
```bash
res: [[ 1.30727832+0.j         -0.38338527+0.17772383j]
 [-0.38338527-0.17772383j  1.69272168+0.j        ]]
```
### StateVector


Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object

The dimension of the StateVector object is obtained by the member method dim() and should be equal to the input dimension of the Choi object

Please refer to [DensityMatrix](#tutorial_table_of_content_QI_DensityMatrix).

Please refer to [StateVector](#tutorial_table_of_content_QI_StateVector).

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Evolution of the StateVector object is performed, and the result is returned as a DensityMatrix object
@end_toggle

Output
```bash
res: [[1.+0.j 0.+0.j]
 [0.+0.j 2.+0.j]]
```

## Boolean function

### Equal

Determine whether the internal data of two SuperOp objects are equal

[Here is API doc](@ref pyqpanda3.quantum_info.quantum_info.SuperOp.__eq__)

@add_toggle_python
    @snippet samples\python\QuantumInformation\Channel\SuperOp.py Determine whether the internal data of two SuperOp objects are equal
@end_toggle

Output
```bash
True
```