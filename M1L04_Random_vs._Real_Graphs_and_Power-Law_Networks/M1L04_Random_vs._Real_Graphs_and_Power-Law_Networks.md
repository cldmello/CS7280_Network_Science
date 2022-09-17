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
- **Prompt 1:** Repeat the derivations given here in more detail. 
- **Prompt 2:** Can you think of a network with n nodes in which all nodes have about the same in-degree but the out-degrees are highly skewed?​

## The Role of the Exponent of a Power-law Distribution 
What is the mean and the variance of a power-law degree distribution?

More generally we can ask: what is the m’th statistical moment of a power-law degree distribution?

It is defined as:
![M1L04_03](imgs/M1L04_03.png)

where c is the proportionality coefficient we derived in the previous page. 

If we rely again on the continuous k approximation, the previous summation becomes an integral that we can easily calculate:
![M1L04_04](imgs/M1L04_04.png)

Note that this integral diverges to infinity if m - α + 1 >= 0 and so, the m’th moment of a power-law degree distribution is well defined (finite) if m < α - 1. 

Consequently, the mean (first moment) exists if α > 2 and the variance (second moment minus the square of the mean) exists if α > 3.  

Of course the variance cannot be “infinite” if the network has a finite number of nodes (i.e., k never ”extends to infinity” in real networks). For many real-world networks however, the exponent α is estimated to be between 2 and 3, which means that even though the distribution has a well-defined average degree, the variability of the degree across different nodes is extremely large.

![M1L04_Fig05](imgs/M1L04_Fig05.jpg)
Standard Deviation is Large in Real Networks, Figure 4.8 from networksciencebook.com by Albert-László Barabási

To illustrate this last point, let’s look at the relation between the average degree and the standard deviation of the degreek_istribution for several real-networks (for more details about these networks please review Table 4.1 of your textbook). 
![M1L04_05](imgs/M1L04_05.png)

Note that many real-world networks have much higher σ than that --  in some cases σ is even an order of magnitude larger than k_. In the case of the WWW in-degree distribution, for example, the average in-degree is only around 4 while the standard deviation of the in-degree is almost 40!​

**Food for Thought**
- Prompt: repeat the derivation for the m’th moment outlined above in more detail. 

