Hellinger Distance  {#tutorial_table_of_content_QI_HellingerDistance}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_PTM}
@next_tutorial{tutorial_table_of_content_QI_HellingerFidelity}

-------------------------------------------------------------------------------------------------------------------------------
# Introduction

In the quantum field, Hellinger Distance is mainly used to measure the similarity between two probability distributions. This concept originated from probability statistics and information theory and is widely used in machine learning. The following is a detailed introduction to the Hailinger distance in the quantum field:

## Definitions and Formulas
Hellinger distance is a special distance measure used to evaluate the similarity between two discrete probability distributions. Given two discrete probability distributions\f$ P = {p1, p2, …, pn}\f$ and \f$Q = {q1, q2, …, qn}\f$, their Hellinger distance can be calculated using the following formula

\f[
D_{\text {Hellinger }}(P, Q)=\sqrt{\frac{1}{2} \sum_{i=1}^{n}\left(\sqrt{p_{i}}-\sqrt{q_{i}}\right)^{2}}
\f]

This formula calculates the distance after square root transformation of two probability distributions, and ensures that the result remains within the interval 0,1 through square root operation. Among them, 0 indicates that the two distributions are completely identical, and 1 indicates that the two distributions are completely different.

## Nature and Characteristics
### Range restriction
The value of Hellinger distance is between 0 and 1, which makes it have special properties in the probability space, making it easy to compare and interpret.
### Stability
Even if the element values in the probability distribution vary greatly, the Hellinger distance can maintain a certain degree of stability and accuracy.
### Symmetry
The Hellinger distance is a symmetric distance measure, that is, \f$D_{Hellinger}(P,Q) = D_{Hellinger}(Q,P)\f$ .
### Application and Significance
In the quantum field, the application of Hellinger distance is mainly focused on the following aspects:

#### Quantum information processing
In the fields of quantum communication, quantum computing, and other quantum information processing, Hellinger distance can be used to evaluate the similarity between different quantum states, thereby guiding the encoding, transmission, and decoding of quantum information.
#### Classification and identification of quantum states
In the task of classification and identification of quantum states, Hellinger distance can be used as a part of the classifier to compare the similarity between the quantum states to be classified and the known quantum states, thus achieving accurate classification and identification.
#### Quantum machine learning
In the field of quantum machine learning, Hellinger distance can be used as a distance metric tool to construct quantum machine learning models based on probability distributions, such as quantum support vector machines and quantum clustering.

# In QPanda3

[Here is API doc for hellinger_distance](@ref pyqpanda3.quantum_info.quantum_info.hellinger_distance)

## Using integer as the random variable value

@add_toggle_python
    @snippet samples\python\QuantumInformation\Analysis\hellinger_distance.py Using integer as the random variable value
@end_toggle

Output
```bash
test_hellinger_distance_1()
sum(p_prob_val): 0.9999999999999999
sum(q_prob_val): 0.9999999999999998
p_prob: {-34: 0.07712188633486798, -43: 0.0021738588008518066, -40: 0.06545344411436894, 52: 0.027455777672370986, -63: 0.017321484561736326, -18: 0.0061523729552665565, -68: 0.028955428941571864, -45: 0.005067773803139212, -53: 0.019347483061192116, -19: 0.038422474076999065, 11: 0.013807373687689771, 80: 0.012652870142195793, -38: 0.0059116186641265025, 74: 0.007793372442072249, -74: 0.00087742930744125, -23: 0.028886736653718742, 6: 0.022981557641975476, 12: 0.012157852967685802, -46: 0.050276053184079836, -70: 0.033016719337137596, -17: 0.06567815571113278, -44: 0.024115235881328936, 33: 0.00237716615149921, -10: 0.00783214565197405, 10: 0.009112240694593808, -4: 0.009145320973744145, 65: 0.03519111895150703, 64: 0.0029129102416369287, -6: 0.0022656764245848637, -80: 0.0007735326730851091, -27: 0.04077993504560989, 30: 0.050661191547952795, 31: 0.04024798452226477, 58: 0.028390297693020355, 63: 0.023446227630413336, -81: 0.012809640115083416, -76: 0.00415933935876831, -56: 0.06537671148628461, 61: 0.0029169671069667496, -54: 0.08823235220634285, 59: 0.007742281581718114}
q_prob: {-34: 0.03457068849138363, -43: 0.004772839775182649, -40: 0.05876174923023205, 52: 0.004514882467470146, -63: 0.0023376071699338892, -18: 0.031166679017488225, -68: 0.010315123790310512, -45: 0.05216095388830002, -53: 0.012815596062048464, -19: 0.019876476216471567, 11: 0.016646038345262078, 80: 0.005707215422743231, -38: 0.03447117527893066, 74: 0.037502737027731846, -74: 0.0018889327122107945, -23: 0.0009203140064451261, 6: 0.009228622921980043, 12: 0.013669293668669537, -46: 0.0017679876752143108, -70: 0.017638212047885776, -17: 0.007484458247633026, -44: 0.029601534194591692, 33: 0.024829564558624628, -10: 0.012842045650639709, 10: 0.0006956768320232853, -4: 0.007677693551464498, 65: 0.051461955064759514, 64: 0.006251227334274152, -6: 0.012987213419061098, -80: 0.05062656204554687, -27: 0.014650982148159889, 30: 0.022886363985379892, 31: 0.012025384934912836, 58: 0.026762884176056614, 63: 0.024403218630415755, -81: 0.005035368039735155, -76: 0.025210681937574014, -56: 0.0009270713478883147, 61: 0.02000229669587882, -54: 0.23884368054853133, 59: 0.034061011440954234}
hd: 0.43507275019757485
```

## Using large integer as the random variable value

@add_toggle_python
    @snippet samples\python\QuantumInformation\Analysis\hellinger_distance.py Using large integer as the random variable value
@end_toggle

Output
```bash
test_hellinger_distance_2()
p_prob: {1000000000001: 0.03777468613192854, 1000000000002: 0.31747765049945736, 1000000000003: 0.45123869480136053, 10000000000004: 0.19350896856725355}
q_prob: {1000000000001: 0.024525210045130204, 1000000000002: 0.06444511900799894, 1000000000003: 0.18338277734850766, 10000000000004: 0.7276468935983633}
hd: 0.4045010669051521
```

## Using string as random variable value

@add_toggle_python
    @snippet samples\python\QuantumInformation\Analysis\hellinger_distance.py Using string as the random variable value
@end_toggle

Output
```bash
test_hellinger_distance_3()
p_prob: {'ab': 0.7808567524866675, 'bc': 0.21914324751333236}
q_prob: {'ab': 0.6427825171953377, 'bc': 0.3572174828046623}
hd: 0.10838483574009858
```