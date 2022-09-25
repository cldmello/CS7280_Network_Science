# Module 2 Lesson 04 - Random vs. Real Graphs and Power-Law Networks

## Learning Objectives

Students will be able to:
- See examples of real networks with highly skewed degree distributions
- Understand the math of power-law distributions and the concept of “scale-free” networks
- Learn about models that can generate networks with power-law degree distribution
- Explain the practical significance of power-law degree distributions through case studies

## Degree Distribution of Real Networks

![M2L04_Fig01](imgs/M2L04_Fig01.png)
![M2L04_Fig02](imgs/M2L04_Fig02.png)
![M2L04_Fig03](imgs/M2L04_Fig03.png)
![M2L04_Fig04](imgs/M2L04_Fig04.png)

## Power-law Degree Distribution

A “power-law network” has a degree distribution that is defined by the following equation:
p_k = c * k^(-α)

In other words, the probability that the degree of a node is equal to a positive integer k is proportional to k^(-α) where α >0.

The proportionality coefficient c is calculated so that the sum of all degree probabilities is equal to one for a given α and a given minimum degree k_min (the minimum degree may not always be 1). 
![M2L04_01](imgs/M2L04_01.png)

The calculation of c can be simplified if we approximate the discrete degree distribution with a continuous distribution:
![M2L04_02](imgs/M2L04_02.png)

Note that the exponent of the CCDF function is α - 1, instead of α. So, if the probability that a node has degree k decays with a power-law exponent of 3, the probability that we see nodes with degree greater than k decays with an exponent of 2.

For directed networks, we can have that the in-degree or the out-degree or both follow a power-law distribution (with potentially different exponents).

**Food for Thought**
- **Prompt 1:** Repeat the derivations given here in more detail. 
- **Prompt 2:** Can you think of a network with n nodes in which all nodes have about the same in-degree but the out-degrees are highly skewed?​

## The Role of the Exponent of a Power-law Distribution 
What is the mean and the variance of a power-law degree distribution?

More generally we can ask: what is the m’th statistical moment of a power-law degree distribution?

It is defined as:
![M2L04_03](imgs/M2L04_03.png)

where c is the proportionality coefficient we derived in the previous page. 

If we rely again on the continuous k approximation, the previous summation becomes an integral that we can easily calculate:
![M2L04_04](imgs/M2L04_04.png)

Note that this integral diverges to infinity if m - α + 1 >= 0 and so, the m’th moment of a power-law degree distribution is well defined (finite) if m < α - 1. 

Consequently, the mean (first moment) exists if α > 2 and the variance (second moment minus the square of the mean) exists if α > 3.  

Of course the variance cannot be “infinite” if the network has a finite number of nodes (i.e., k never ”extends to infinity” in real networks). For many real-world networks however, the exponent α is estimated to be between 2 and 3, which means that even though the distribution has a well-defined average degree, the variability of the degree across different nodes is extremely large.

![M2L04_Fig05](imgs/M2L04_Fig05.jpg)
*Standard Deviation is Large in Real Networks, Figure 4.8 from networksciencebook.com by Albert-László Barabási*

To illustrate this last point, let’s look at the relation between the average degree and the standard deviation of the degreek_istribution for several real-networks (for more details about these networks please review Table 4.1 of your textbook). 
![M2L04_05](imgs/M2L04_05.png)

Note that many real-world networks have much higher σ than that --  in some cases σ is even an order of magnitude larger than k_. In the case of the WWW in-degree distribution, for example, the average in-degree is only around 4 while the standard deviation of the in-degree is almost 40!

**Food for Thought**
- Prompt: repeat the derivation for the m’th moment outlined above in more detail. 


## How to Check if a Network Has Power-law Degree Distribution

![M2L04_Fig06](imgs/M2L04_Fig06.jpeg)
*Rescaling the Degree Distribution, Figure 4.23 from networksciencebook.com by Albert-László Barabási*

In practice, the degree distribution may not follow an ideal power-law distribution throughout the entire range of degrees. In other words, the empirical degree distribution may not be a perfect straight-line when plotted in log-log scale. Instead, it may look similar to the left plot at this page. There are two important points about this distribution:

- a) For lower values of k, we observe a “low-degree saturation” that **decreases the probability of seeing low-degree nodes** compared to an ideal power-law. If this saturation effect takes place mostly for nodes with degree less than k_sat, we can capture  this effect by considering a modified power-law expression: (k + k_sat)^(-α) Do you see why adding the term k_sat causes a decrease in the probability of low-degree nodes (but it has a minor effect on high-degree nodes)? 
- b) For very high values of k there is again a deviation from the ideal power-law form. The reason is that in practice there are always some “structural” effects that **limit the maximum degree** that a node can have. For example, in a router-level computer network, the maximum degree is limited by the maximum number of interfaces that a router can have due to hardware or cost constraints. To capture such a high-degree cutoff point, we often need to constrain the upper-tail of the degree distribution up to a value k_cut. This value is determined by practical connectivity constraints for the network we analyze. 

In summary, when we want to check if a network follows a power-law degree distribution, we  need to consider whether that distribution drops almost linearly with k in a range between k_sat and k_cut. 

Another approach, that addresses the low-degree saturation effect, is to “rescale” the distribution so that we examine the behavior of the degree probability with (k +k_sat) (instead of k) – still considering the range up to k_cut. This rescaling approach is shown at the plot at the right. 

**Food for Thought**
A more rigorous statistical approach to examine if a network has a power-law degree distribution is described at the following site: [Power-law Distributions in Empirical Data](https://aaronclauset.github.io/powerlaws/). You will experiment with this (or similar) method in an assignment. For now, you may just want to read the description of that method from the given URL. 


## How to Plot Power-law Degree Distribution

![M2L04_Fig07](imgs/M2L04_Fig07.jpeg)
*Plotting a Degree Distributions, Image 4.22 from networksciencebook.com by Albert-László Barabási*

There are several ways to plot a power-law distribution – but not all of them are good and some of them can even be misleading. 

This plot visualizes the same distribution, ![M2L04_06](imgs/M2L04_06.png)i n four different ways. The term k_0 = 10 causes the “low-degree saturation” that you see at the left tail of the distribution, decreasing the probability of lower-degrees.

**The four plots are:**
- linear-linear scale. This is clearly a bad way to plot a power-law distribution. 
- log-log scale but with linear binning (the bin width of the histogram increases linearly with k). This is also not an appropriate approach because the bins of the histogram for high values of k are quite narrow and they often include 0 or 1 measurements, creating the “plateau” that you see at the tail of the distribution. 
- log-log scale but with logarithmic binning (the bin width of the histogram increases exponentially with k). Note how the tail of the distribution drops almost linearly with k when the degree is higher than about 50-100. A potential issue with this approach is that we still need to figure out how fast to increase the bin width with k.
- the Complementary Cumulative Distribution Function (C-CDF), which shows the probability P_k that the degree is higher or equal than k (all previous plots show the probability p_k that the degree is equal to k). This is the best approach because we do not need to determine an appropriate sequence of bin-widths. Please note however that the slope of the C-CDF is not the same as the slope of the degree distribution. In this example, α = 2.5 and so the exponent of the C-CDF is 1.5.

## Scale-free Nature of Power-law Networks

![M2L04_Fig08](imgs/M2L04_Fig08.jpeg)
*Lack of an Internal Scale, figure 4.7 from networksciencebook.com by Albert-László Barabási*

One of the first things we learn in statistics is that the Normal distribution describes quite accurately many random variables (due to the Central Limit Theorem), and that according to that distribution 99.8% of the data are expected to fall within 3 standard deviations from the average.

Qualitatively, this is true for all distributions with exponentially fast decreasing tails, which includes the Poisson distribution and many others. In networks that have such degree distributions, the average degree represents the **“typical scale”** of the network, in terms of the number of connections per node (see green distribution at the visualization). 

On the other hand, a power-law degree distribution with exponent 2 < α < 3 has finite mean but infinite variance (any higher moments, such as skewness are also infinite). The infinite variance of this statistical distribution means that we cannot expect the data to fall close to the mean. On the contrary, the mean (the average node degree in our case) can be a rather uninformative statistic in the sense that a large fraction of values can be lower than the mean, and that many values can be much higher than the mean (see purple distribution  at the visualization). 
 
For this reason, people often refer to power-law networks as **“scale-free”**, in the sense that the node degree of such networks does not have a **“typical scale”**. In the rest of this course, we prefer to use the term **“power-law networks”** because it is more precise. 

## The Maximum Degree in a Power-law Network

Let us now derive an expression for the maximum degree we can expect to see in a power-law network with  nodes, and to compare that with the corresponding maximum degree we can expect to get from a network in which the degree distribution decays exponentially fast with the degree . Recall that the Poisson distribution of  networks decays even faster than exponential distribution.
![M2L04_Fig09](imgs/M2L04_Fig09.png)
This means that the maximum degree increases very slowly (logarithmically) with the network size n, when the degree distribution decays exponentially fast with k. 


Let us now repeat these derivations but for a power-law network with the same minimum degree k_min and an exponent α.
![M2L04_Fig10](imgs/M2L04_Fig10.png)
This means that the **maximum degree in a power-law network increases as a power-law of the network size n. If the maximum degree increases with the square-root of n.**

**In the more extreme case that α = 2 the maximum degree increases linearly with n!**

To put these numbers in perspective, consider a network with one million nodes, and an average degree of k_ = 3. If the network follows the exponential degree distribution, the maximum expected degree is only about 10 – not much larger than the average degree.  

If the network follows the power-law degree distribution with exponent α = 2.5 (recall that this is a typical value for many real-world networks), we get that the maximum degree is about 10,000!

![M2L04_Fig11](imgs/M2L04_Fig11.jpeg)
*Random vs. Scale-free Networks, Figure 4.6 from networksciencebook.com by Albert-László Barabási*

The example above clearly illustrates a major difference between exponential and power-law networks: the latter have nodes with a much greater number of connections than the average node – we typically refer to those nodes as **Hubs**. 

The visualization at this page illustrates the difference between exponential and power-law networks focusing on the presence of hubs. The network of major interstate highways in the US follows a Poisson distribution, without any nodes (cities) that have a much larger degree than the average. On the other hand, the network of direct flights between the major US cities follows a power-law degree distribution and there are obvious hubs, such as the airports in Atlanta, Chicago or New York city. 

**Food for Thought**
Derive the integrals shown in this page yourself.


## Random Failures and Targeted Attacks in Power-law Networks
Another interesting characteristic of power-law networks is that they behave very differently than random ER-graphs (or, more generally, networks with Poisson degree distribution) in the presence of node failures. 

Let us first distinguish between random failures (where a fraction f of randomly selected nodes are removed from the network), and targeted attacks (where a fraction f of the nodes with the highest degree are removed from the network). In a communication network, for instance, random failures can be caused by router malfunctions, while targeted attacks may be caused by a terrorist that knows the topology of the network and disrupts the highest-connectivity routers first. 

![M2L04_Fig12](imgs/M2L04_Fig12.jpeg)
*Scale-free Network Under Attack, Figure 8.11 from networksciencebook.com by Albert-László Barabási*

![M2L04_Fig13](imgs/M2L04_Fig13.jpeg)
*Attacks and Failures in Random Networks. Figure 8.13 from networksciencebook.com by Albert-László Barabási*

The plots in this page compare the effect of both random failures and attacks on both power-law networks (first) and ER-graphs (second). In both cases, the networks have the same size (10,000 nodes and 15,000 edges). The exponent of the power-law distribution in the network at the left is 2.5 (meaning that the variance of the degree distribution is infinite) and the average degree is 3. 

The y-axis of both plots shows the fraction of nodes that belong to the largest connected component, for a given fraction f of removed nodes (more precisely, the y-axis shows the ratio ![M2L04_07](imgs/M2L04_07.png), where the numerator is the probability that a node belongs in the largest connected component given a fraction f of removed nodes, while the denominator is the same probability but when f = 0). 

In terms of random failures, the power-law degree network is much more robust than the random network: the largest connected component includes almost all nodes, even as f approaches 100% of the nodes. On the other hand, the random network’s largest connected component disintegrates after f exceeds a critical threshold (around 0.7 in this example). This difference is mostly due to the presence of hubs in power-law networks: hubs have so many connections that they manage to keep the non-deleted nodes in the same connected component, even when f is close to 1. 

The situation is very different from targeted attacks: power-law networks are very sensitive to those because an attacker would first delete the hub nodes, causing a disintegration of the largest connected component after f exceeds a critical threshold (around 0.15 in this example). That critical threshold is higher for Poisson networks because they do not have hub nodes. 

In summary, power-law networks are more robust to random failures than Poisson networks – but they are also more sensitive to targeted attacks than Poisson networks. 

## Degree-preserving Randomization

![M2L04_Fig14](imgs/M2L04_Fig14.jpeg)
*Degree Preserving Randomization, Figure 4.17 from networksciencebook.com by Albert-László Barabási*

Suppose that you analyze a certain network G and you find it has an interesting property P.  For example, P may be one of the robustness properties we discussed in the previous page about the size of the largest connected component under random or targeted node removals. How can you check statistically whether P is caused by the degree distribution of the network G (as opposed to other network characteristics)?

One approach to do is to **”randomize”** the network G but without modifying the degree of any node. If we have a way to do so, we can create a large number of **“degree-preserving”** random networks – and then examine whether these networks also exhibit the property P. To make the analysis convincing we can also create another ensemble of randomized networks that do NOT have the same degree distribution with G but that maintain the same number of nodes and edges. Let us call these networks **”fully randomized”**. 

If the property P of G is present in the degree-preserving randomized networks – but P is not present in the fully randomized networks, we can be confident that the property P is a consequence of the degree distribution of G and not of any other property of G.

To perform **”full randomization”**, we can simply pick each edge (S1, T1) of G and change it to (S1, T2), where T2 is a randomly selected node. Note that this does not change the number of nodes or edges (and so the network density remains the same). The degree distribution however can change significantly. 

To perform **“degree-preserving randomization”**, we can pick two random edges(S1, T1) and (S2, T2) and rewire them to (S1,T2) and (S2, T1). Note that this approach preserves the degree of every node. This approach is repeated until we have rewired each edge at least once. 

The visualization at the lower part of the panel shows an example of a power-law network and two randomized networks. The degree-preserving network maintains the presence of hubs as well as the connected nature of the original network. The fully randomized network on the other hand does not have hubs (and it could even include disconnected nodes – even though that does not happen in this example). 

**Food for Thought**
There are more randomization approaches that can preserve more network properties than the degree distribution. How would you randomize a directed network so that the in-degree and the out-degree distributions remain the same? 

## The Average Degree of the Nearest Neighbor at a Power-law Network

![M2L04_Fig15](imgs/M2L04_Fig15.jpeg)
*Structural Disassortativity, Figure 7.7 from networksciencebook.com by Albert-László Barabási*

Recall the notion of **“average neighbor degree”** from Lesson-3 – which is the same with the expected degree of a node that is connected to a randomly sampled edge stub. 

Under the assumption of a **“neutral network”** (i.e., no correlation between the degrees of two connected nodes), we derived that the average neighbor degree is: 

![M2L04_08](imgs/M2L04_08.png)

This shows one more interesting property of power-law networks: as we previously discussed, these networks can have very large degree variability in practice (i.e., σ is much higher than k_). Consequently, the average neighbor degree can be much higher than the network’s average degree – intensifying the impact of the friendship paradox. 

The visualization at this page shows a power-law network with 300 nodes, 450 edges, and exponent 2.2, generated by the configuration model. We highlight the two highest-degree hubs. Note that many nodes are connected to those hubs, increasing the average neighbor degree of those nodes. 

## Configuration Model

Suppose that a real-world communication network with one thousand nodes has a power-law degree distribution with exponent α = 2.5 – you may want to investigate how this network will perform when it grows to ten thousand nodes, assuming that its degree distribution exponent remains the same. 

How can we create a synthetic network that has a given degree distribution? This is a central question in network modeling. 

A general way to create synthetic networks with a specified degree distribution p_k is the “configuration model”. The inputs to this model is a) the desired number of nodes n, and b) the degree ki of each node i . The collection of all degrees specifies the degree distribution of the synthetic network. 

![M2L04_Fig16](imgs/M2L04_Fig16.jpeg)
*The Configuration Model, Figure 4.15 from networksciencebook.com by Albert-László Barabási*

The configuration model starts by creating the n nodes: node i has ki available “edge stubs”. Then, we keep selecting randomly two available stubs and connect them together with an edge, until there are no available stubs. The process is guaranteed to cover all stubs as long as the sum of all node degrees is even.

The configuration model process is random and so it creates different networks each time, allowing us to produce an ensemble of networks with the given degree distribution. 

Additionally, note that the constructed edges may form self-loops (connecting a node to itself) or multi-edges (connecting the same pair of nodes multiple times). In some applications multi-edges and self-loops are not allowed – but the good news is that they are unlikely to happen when n is very large and the network is sparse. Removing them artificially is another option but it can cause deviations from the desired degree distribution. 

The visualization of this page shows three different networks with n=4 that can result from the configuration model, given the same degree distribution. Note that there are more than three different networks that could include self loops and multi-edges - can you find the rest?

**Food for Thought**
What is the probability that the configuration model will connect two nodes of degree ki and kj? 

## Preferential Attachment Model

The configuration model can generate networks with arbitrary degree distributions – including power-laws with any exponent. However, the configuration model does not suggest any “generating mechanism” that explains how a network can gradually acquire a power-law degree distribution. 

![M2L04_Fig17](imgs/M2L04_Fig17.jpeg)
*Evolution of the Barabási-Albert Model, Figure 5.3 from networksciencebook.com by Albert-László Barabási*

One such generating model is known as **“preferential attachment”** (or PA model or “Barabási-Albert” model). In this model, the network grows by one node at each time step – so the network has t nodes after t time steps. Every time a new node is added to the network, it connects to m existing nodes (m is the same for all new nodes – suppose that we allow self-loops and multi-edges for now). The neighbors of the new node are chosen randomly but with a non-uniform probability, as follows.

Suppose that the new node arrives at a point in time t, and let ki(t) be the degree of node i at that time. The probability that the new node will connect to node i is:

![M2L04_09](imgs/M2L04_09.png)

In other words, in the preferential attachment model, the network grows over time and new nodes are more likely to connect to nodes with higher degrees (see above plot). This is a ”rich get richer” effect because nodes with higher degree attract more connections from new nodes, making their degree even higher relative to other nodes. 

Later in the course, we will return to this model and study its behavior mathematically.

For now, we only mention without proof that this model produces power-law networks with an exponent α=3. The value of the parameter m (number of edges of new node) does not affect the exponent of the distribution.

The degree distribution at the plot below refers to a network that was created with the preferential attachment model, after generating n=100,000 nodes and with m=3 (the green dots show a log-binned estimate of the distribution while the purple dots show a linearly-binned histogram).

![M2L04_Fig18](imgs/M2L04_Fig18.jpeg)
*The Degree Distribution, Figure 5.4 from networksciencebook.com by Albert-László Barabási*

The main value of the preferential attachment model is that it suggests that power-law networks can be generated through the combined effect of two mechanisms: growth and preferential connecting to nodes with a higher degree. Either of these two mechanisms on its own would not be sufficient to produce power-law networks.
 

**Food for Thought**
How would you modify the preferential attachment model so that you get an exponent α between 2 and 3?

## Link Selection Model
![M2L04_Fig19](imgs/M2L04_Fig19.jpeg)
*Link Selection Model, Figure 5.13 from networksciencebook.com by Albert-László Barabási*

Another very simple generating model that also creates power-law networks with exponent 3 is the “link selection” model.

Suppose that each time we introduce a new node, we select a random link and the new node connects to one of the two end-points of that link (randomly chosen). In other words, the new node connects to a randomly selected edge-stub (see visualization).

In this model, the probability that the new node connects to a node of degree k is proportional to k (because the node of degree-k has k stubs). But this is exactly the same condition with the preferential attachment model: a linear relation between the degree k of an existing node and the probability that the new node connects to that existing node of degree-k. 

So, the link selection model is just a variant of preferential attachment and it also produces power-law degree distribution with exponent α=3. 

## What Does The Power-law Property Mean in Practice?

![M2L04_Fig20](imgs/M2L04_Fig20.jpg)
[Image source](https://www.nature.com/articles/35082140/figures/2)

The focus of this lecture so far has been on the statistical properties of networks with power-law degree distribution. What does this property mean in practice, however? And how does it affect network phenomena that all of us care about, such as the spread of epidemics?

To answer the first question, let us consider the case of networks of sexual partners. There are several diseases that spread through sexual intercourse, including HIV-AIDS, syphilis or gonorrhea. The degree distribution in such networks relates to the number of sexual partners of each individual (node in the graph). The plots on this page are based on a 1996 survey of sexual behavior conducted in Sweden. The number of respondents was 2,810 and the age range was from 18 to 74 years old (roughly balanced between men and women).

The plot at the left is the C–CDF for the number of partners of each individual during the last 12 months, shown separately for men and women. Note that the distributions drop roughly linearly in the log-log scale plot, suggesting the presence of a power-law distribution (at least in the range from 2 to 20).

The plot at the right is the corresponding C-CDF but this time for the entire lifetime of each individual. As expected, the range of the distribution now extends to a wider range (up to 100 partners for women and 1000 for men). Note the low-degree saturation effect we discussed earlier in this lesson, especially for less than 10 partners. The exponent of the C-CDF distributions is α_tot = 2.1 ± 0.3 for women (in the range k_tot > 20), and α_tot = 1.6 ± 0.3 for men (in the range 20 k_tot < 400). Estimates for females and males agree within statistical uncertainty. Note that these exponents refer to the C-CDF – so the corresponding exponents for the degree distributions would be, on average, 3.1 for women and 2.6 for men.

These exponent values suggest that, at least for men, the corresponding network of sexual contacts would have a power-law distribution with very high variability (theoretically, “infinite variance”). The distribution also shows the presence of hubs: individuals with hundreds of partners during their lifetime. The wide variability in this distribution justifies targeted intervention approaches that aim to identify the “hub individuals” and provide them with additional information, resources (such as condoms or treatment), and when available, vaccination.

## Case Studies: Superspreaders

**Superspreaders in SARS epidemic**
![M2L04_Fig21](imgs/M2L04_Fig21.jpeg)
The SARS (Severa Acute Respiratory Syndrome) was an epidemic back in 2002-3. It infected 8000 people in 23 countries and it caused about 800 deaths.

The plot shown here shows how the infections progressed from a single individual (labeled as patient-1) to many others. Such plots result from a process known as “contact tracing” – finding out the chain of successive infections in a population.

It is important to note the presence of a few hub nodes, referred to as **“superspreaders”** in the context of epidemics. The superspreaders are labeled with an integer identifier in this plot. The superspreader 1, for example, infected directly about 20 individuals.

The presence of superspreaders emphasizes the role of degree heterogeneity in network phenomena such as epidemics. If the infection network was more **“Poisson-like”**, it would not have superspreaders and the total number of infected individuals would be considerably smaller.

Source: [Super-spreaders in infectious diseases Richard A.Stein](https://www.sciencedirect.com/science/article/pii/S1201971211000245), International Journal of Infectious Diseases, August 2011.

**Superspreaders Versus The Average Reproductive Number R0**
![M2L04_Fig22](imgs/M2L04_Fig22.png)

Epidemiologists often use the basic “reproductive number”, R0, which describes the average number of secondary infections that arise from one infected individual in an otherwise totally susceptible population.

One way to estimate R0 is to multiply the average number of contacts of an infected individual by the probability that a susceptive individual will become infected by a single infected individual (“shedding potential”). So, the R0 metric does not depend only on the given pathogen – it also depends on the number of contacts that each individual has. If R0>1 then an outbreak is likely to become an epidemic, while if R0<1 then an outbreak will not spread beyond a few initially infected individuals

It is important to realize however that R0 is only an average – it does not capture the heterogeneity in the number of contacts of different individuals (and it also does not capture the heterogeneity in the shedding potential of the pathogen at different individuals). As we know by now, contact networks can be extremely heterogeneous in terms of the degree distribution, and they can be modeled with a power-law distribution of (theoretically) infinite variance. Such networks include hubs – and as we saw above, hubs can act as superspreaders during epidemic outbreaks.

The table in this page confirms this point for several epidemics. The third column shows R0 while the fourth column shows ”Superspreading events” (SSE). These are events during an outbreak in which a single infected individual causes a large number of direct or indirect infections. For example, in the case of the 2003 SARS epidemic in Hong Kong, even though R0  was only 3, there was an SSE in which an infected individual caused a total of 187 infections (patient-1 above).

SSEs have been observed in practically every epidemic – and they have major consequences both in terms of the speed through which an epidemic spreads and in terms of appropriate interventions. For example, in the case of respiratory infections (such as COVID-19) “social distancing” is an effective intervention only as long as it is adopted widely enough to also include superspreaders.

Source: Cellular Superspreaders: [An Epidemiological Perspective on HIV Infection inside the Body Kristina Talbert-Slagle et al., 2014](https://doi.org/10.1371/journal.ppat.1004092
)