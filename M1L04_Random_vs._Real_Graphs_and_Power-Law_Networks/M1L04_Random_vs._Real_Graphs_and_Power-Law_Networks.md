# Module 1 Lesson 04 - Random vs. Real Graphs and Power-Law Networks

## Learning Objectives

Students will be able to:
- See examples of real networks with highly skewed degree distributions
- Understand the math of power-law distributions and the concept of “scale-free” networks
- Learn about models that can generate networks with power-law degree distribution
- Explain the practical significance of power-law degree distributions through case studies

## Degree Distribution of Real Networks

![M1L04_Fig01](imgs/M1L04_Fig01.png)
![M1L04_Fig02](imgs/M1L04_Fig02.png)
![M1L04_Fig03](imgs/M1L04_Fig03.png)
![M1L04_Fig04](imgs/M1L04_Fig04.png)

## Power-law Degree Distribution

A “power-law network” has a degree distribution that is defined by the following equation:
p_k = c * k^(-α)

In other words, the probability that the degree of a node is equal to a positive integer k is proportional to k^(-α) where α >0.

The proportionality coefficient c is calculated so that the sum of all degree probabilities is equal to one for a given α and a given minimum degree k_min (the minimum degree may not always be 1). 
![M1L04_01](imgs/M1L04_01.png)

The calculation of c can be simplified if we approximate the discrete degree distribution with a continuous distribution:
![M1L04_02](imgs/M1L04_02.png)

Note that the exponent of the CCDF function is α - 1, instead of α. So, if the probability that a node has degree k decays with a power-law exponent of 3, the probability that we see nodes with degree greater than k decays with an exponent of 2.

For directed networks, we can have that the in-degree or the out-degree or both follow a power-law distribution (with potentially different exponents).

**Food for Thought**
- **Prompt 1:** Repeat the derivations given here in more detail. ​
- **Prompt 2:** Can you think of a network with n nodes in which all nodes have about the same in-degree but the out-degrees are highly skewed?​

