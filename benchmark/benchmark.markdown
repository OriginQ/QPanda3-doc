Benchmark {#benchmark}
======================

[TOC]

The following benchmark rom IBM's Qiskit [benchpress](https://github.com/Qiskit/benchpress). We forked the repository and added test items for pyqpanda3 to complete the relevant tests. Our test repository can be found at https://github.com/OriginQ/benchpress. We have introduced more detailed experimental content in the literature  [QPanda3: A High-Performance Software-Hardware Collaborative Framework for Large-Scale Quantum-Classical Computing Integration](https://doi.org/10.48550/arXiv.2504.02455). This webpage  cited important experimental results from the article.

> "The Benchpress open-source benchmarking suite comprises over 1,000 different tests. These are standardized benchmarking tests designed by other members of the quantum community. For example, Benchpress compares SDKs’ abilities to generate QASMBench circuits, Feynman circuits, and Hamiltonian circuits. It also includes tests designed to test a language's ability to transpiler circuits for specific hardware, including the heavy hex architecture of IBM quantum processors and other generic qubit layouts."
>
> — **IBM Qiskit**

### Quantum Circuit Construct

The performance of several SDKs in circuit construction is illustrated in Figure **Quantum Circuit Construct** . It can be observed that QPanda3, qiskit, and cirq passed all seven test cases. For the four test cases, namely test_multi_control_circuit, test_bigint_qasm2_import, test_param_circSU2_100_build, and test_clifford_build, QPanda3 demonstrated the shortest execution time. In the case of the three test cases test_QV100_qasm2_import, test_param_circSU2_100_build, and test_QV100_build QPanda3 took longer than qiskit but significantly less time compared to cirq, braket, and bqskit. For the test case test_DTC100_set_build, QPanda3's execution time was shorter than that of qiskit but longer than cirq's, ranking as the second shortest. This figure highlights the advantages of QPanda3 in circuit construction.

![Quantum Circuit Construct](images/benchmark/benchmark_construct_circuits_time.png)

### Manipulate

The [Benchmarking the performance of quantum computing software](https://doi.org/10.48550/arXiv.2409.08844) literature highlights that Manipulate is a crucial metric for evaluating SDKs. As shown in Figure **Manipulate** , only QPanda3 and qiskit passed all four test cases. In the test_QV100_basis_change test case, QPanda3 demonstrated the shortest execution time. For the three test cases: test_random_clifford_decompose, test_multi_control_decompose, and test_DTC100_twirling, QPanda3's execution time slightly exceeded that of qiskit, with a minimal difference. This figure suggests that QPanda3 holds an advantage in terms of the Manipulate metric.

![Manipulate](images/benchmark/benchmark_manipulate_time.png)

### Quantum Circuit Transpilation

In our experiments, only QPanda3 and qiskit passed the majority of the test cases related to compilation. We have plotted the corresponding experimental results into multiple scatter plots, as shown in Figure **Quantum Circuit Transpilation** , which illustrate the differences between QPanda3 and qiskit in terms of 2Q Gate Count, 2Q Gate Depth, and compilation time for various topological structures. In each plot, the blue dashed line represents the point where the values for QPanda3 and qiskit are equal for the corresponding metric. Points above the blue dashed line indicate that the corresponding value for QPanda3 is less than that for qiskit. As observed from Figure **Quantum Circuit Transpilation** , for both 2Q Gate Count and 2Q Gate Depth, the majority of the scatter points are densely distributed along or very close to the blue dashed line, suggesting that the performance of QPanda3 and qiskit is essentially the same. However, regarding compilation time, all scatter points in the four subplots of Figure **Quantum Circuit Transpilation** are located above the blue dashed line, indicating that QPanda3 exhibits higher compilation efficiency than qiskit. This characteristic is not limited by topological structure and is effective for a large number of different circuits. Additionally, it can be observed that only a very few test cases result in compilation times exceeding 10 seconds when using QPanda3. These observations collectively demonstrate the high compilation efficiency of QPanda3.

![Quantum Circuit Transpilation](images/benchmark/benchmark_transpile.png)
