KL Divergence  {#tutorial_table_of_content_QI_KL_divergence}
=============================================================================

[TOC]

@prev_tutorial{tutorial_table_of_content_QI_HellingerFidelity}
@next_tutorial{tutorial_table_of_content_QI_Unitary}

-------------------------------------------------------------------------------------------------------------------------------

# Introduction

The KL divergence in the quantum domain is an important concept used to measure the difference or information loss between two quantum probability distributions. The following is a detailed introduction to KL divergence in the quantum field:

## Definition and Background
KL divergence, also known as information gain or relative entropy, is widely used in information theory and statistics to evaluate the information loss incurred when a theoretical or fitted distribution Q approximates a true distribution P. In the quantum domain, the KL divergence can also be used to measure the difference between two quantum states or quantum probability distributions.

## Formula and Calculation
For quantum states ρ and σ, their KL divergence is usually defined as
\f[
D_KL(ρ||σ) = tr(ρ log(ρ/σ))
\f]

Among them, tr represents the trace of a matrix, which is the sum of the elements on the diagonal of the matrix. This formula is the standard form of the quantum KL divergence, which is used to calculate the difference between two quantum states.

## Nature and characteristics
### Asymmetry KL 
divergence is asymmetric, that is
 \f$D_KL(ρ||σ) ≠ D_KL(σ||ρ)。\f$ 
This means that when using KL divergence to measure the difference between two quantum states, one needs to pay attention to the order.
### Non-negative
KL divergence is always greater than or equal to 0, that is
 \f$D_KL(ρ||σ) ≥ 0\f$ 
.If and only if \f$ρ=σ\f$ ,
 \f$D_KL(ρ||σ) = 0\f$
  。
This shows that when two quantum states are exactly the same, the KL divergence between them is 0.
Information loss: KL divergence can be seen as the amount of extra information needed when using distribution σ to encode samples from distribution ρ. It measures the loss of information when using one distribution to approximate another.

# In QPanda3

[Here is API doc for KL_divergence](@ref pyqpanda3.quantum_info.quantum_info.KL_divergence)

## KL divergence for calculating the probability distribution of discrete random variables

@add_toggle_python
    @snippet samples\python\QuantumInformation\Analysis\KL_divergence.py KL divergence for calculating the probability distribution of discrete random variables
@end_toggle

Output
```bash
p_prob: [0.07732759 0.0389159  0.07715404 0.11005092 0.02503134 0.02849995
 0.01620025 0.02423337 0.03629235 0.17035652 0.17097729 0.22496048]
q_prob: [0.19705237 0.04430997 0.28896571 0.09751531 0.03782515 0.00203659
 0.15336831 0.06625373 0.03402398 0.05084955 0.02178681 0.00601251]
kld: 1.2135042033718717
```

## Calculate the KL divergence of the probability of continuous random variables

@add_toggle_python
    @snippet samples\python\QuantumInformation\Analysis\KL_divergence.py Calculate the KL divergence of the probability of continuous random variables
@end_toggle

Output
```bash
p_pdf(3.14) 0.12692546072510916
kld: 9.422541885590975
```