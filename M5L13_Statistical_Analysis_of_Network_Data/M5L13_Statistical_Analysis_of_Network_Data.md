# Module 5 Lesson 13 - Introduction to Network Sampling

## Learning Objectives
Students will:

- Understand some major questions and methods in "network inference"
- Learn key network sampling strategies
- Review link prediction statistical methods
- Infer association networks based on correlation metrics
- Understand the principles of network tomography through theory and case studies
 

## Required and Recommended Reading
**Required Reading**
- [Sampling and estimation in network graphs](https://link.springer.com/chapter/10.1007/978-0-387-88146-1_5), by Eric D.Kolaczyk, 2009
- [Network inference with confidence from multivariate time series](https://journals.aps.org/pre/abstract/10.1103/PhysRevE.79.061916), by Mark Kramer et al, 2009
- [Network topology inference](https://link.springer.com/chapter/10.1007/978-0-387-88146-1_7), by Eric D.Kolaczyk, 2009

**Recommended Reading**
- [What is the real size of a sampled network? The case of the Internet](https://journals.aps.org/pre/abstract/10.1103/PhysRevE.75.056111) by Fabian Viger et al., 2007


## Introduction to Network Sampling
It is often the case that we do not know the complete network. Instead, we only have sampled network data, and we need to rely on that sample to infer properties of the entire network. The area of statistical sampling theory provides some general approaches and results that are useful in this goal.

Let us denote the complete network as G = (V,E) – we will be referring to it as the **population graph**.

Additionally, we have a **sampled graph** G* = (V*,E*), which consists of a subset of nodes V* and edges E* from the population graph.

To illustrate the challenges involved in network sampling, consider the following problem.

Suppose we want to estimate  the average degree of the population graph, defined as
![M5L13_01](imgs/M5L13_01.png)

where k_i is the degree of node-i and N is the number of nodes in the population graph.

The obvious approach is to estimate this with the average degree of the sampled graph G* as follows:
![M5L13_02](imgs/M5L13_02.png)

where n is the size of the sample.

Now, consider two different network sampling strategies, or “designs”. In both of them we start with a random sample V* of n nodes:
![M5L13_03](imgs/M5L13_03.png)

Note that Design-1 requires that we know all neighbors of each sampled node, even if those neighbors are not sampled – while Design-2 only observes the adjacencies between sampled nodes. In practice, this means that Design-2 would be simpler or less costly to conduct. Imagine, for example, that we construct a social network based on who is calling whom. Design-1 would require that we know all phone calls of the sampled individuals. Design-2 would require that we only know whether two sampled individuals have called each other.

Which of the two Designs will result in a better estimate of E[k] in your opinion? Please think about this for a minute before you look at the answer below.

If you answered Design-1, you are right. With Design-2, we underestimate the degree of each node by a factor of roughly n/N because, on the average, we only “see” that fraction of nodes of the population graph.

That does not mean however that Design-2 is useless. As we will see later in this Lesson, we could use Design-2 and get a reasonable estimate of the average degree as long as we add an appropriate “correction factor”. For example, we could get a better estimate with Design-2 if we use the following correction:

![M5L13_04](imgs/M5L13_04.png)

This can be a useful approach if Design-2 is simpler or cheaper to apply than Design-1.

Here is another example of how statistical sampling theory can be useful: imagine that you want to estimate the number N of nodes in the population network G. For practical reasons however, it is impossible to identify every single node in G -- you can only collect samples of nodes. How would you estimate N? Please think about this for a minute before you read the following.

![M5L13_Fig01](imgs/M5L13_Fig01.jpeg)
Image source: http://www.old-ib.bioninja.com.au/options/option-g-ecology-and-conser/g5-population-ecology.html

We could use a statistical technique called **capture-recapture estimation**. The simplest version is that we first select a random sample S_1 of n_1 nodes, without replacement. The important point is that we somehow “mark” these nodes. For instance, if they have a unique identifier, we record that identifier.

![M5L13_05](imgs/M5L13_05.png)

In sampling theory, two important questions are:

- a) Is the estimator unbiased, meaning that its expected value is equal to the population-level metric we try to estimate. In the previous example, is it that E[^N] = N, across all possible independent samples S_1 and S_2?
- b) What is the variance of the estimator? Estimators with large variance are less valuable in practice.

![M5L13_06](imgs/M5L13_06.png)

**Food For Thought**
For the previous estimator ^N, show that if n_1 and n_2 are fixed, the variable n_3 follows the hypergeometric distribution. Use this result to derive the previous expression for the variance of ^N.

## Network Sampling Strategies

Induced subgraph sampling
![M5L13_Fig02](imgs/M5L13_Fig02.png)

Incident subgraph sampling
![M5L13_Fig03](imgs/M5L13_Fig03.png)

Snowball sampling
![M5L13_Fig04](imgs/M5L13_Fig04.png)

Traceroute sampling
![M5L13_Fig05](imgs/M5L13_Fig05.png)

## Inclusion Probabilities with Each Sampling Strategy

An important question for each sampling strategy is: what is the probability that a given sampling strategy includes a node or an edge in the sample? As we will see later, these probabilities are essential in deriving important network estimators. 

![M5L13_Fig06](imgs/M5L13_Fig06.png)
*Image source: Kolaczyk, Eric D. Statistical Analysis of Network Data: Methods and Models (2009) Springer Science+Business Media LLC.*

For **induced subgraph sampling** (see above), the node and edge inclusion probabilities are, respectively:

π_i = n/N, π_i,j = n(n-1)/N(N-1)

where n is the number of sampled nodes -- because each node of the population graph is sampled uniformly at random without replacement, and each edge of the population graph is sampled if the two corresponding nodes are sampled. Note that the node inclusion probability is the same for all nodes (and the same is true for the edge inclusion probability). 

![M5L13_Fig07](imgs/M5L13_Fig07.png)

For **incident subgraph sampling** (see above), recall that we sample n edges randomly and without replacement. The inclusion probability for edges is simply: π_i,j = n/|E|, where |E| is the number of edges in the population graph. The inclusion probability for node i is one minus the probability that none of the  edges of node i is sampled: 

![M5L13_07](imgs/M5L13_07.png)

Of course if n > |E| - k_i then π_i = 1. Note that, with incident subgraph sampling, nodes of higher degree have a higher inclusion probability. 

![M5L13_Fig08](imgs/M5L13_Fig08.png)

With **snowball sampling** (see above -- the yellow nodes are the seeds and the process has two stages: first orange nodes and then brown nodes) , the inclusion probabilities are harder to derive, especially when the snowball process includes multiple stages. The statistical literature provides some approximate expressions, however. 

![M5L13_Fig09](imgs/M5L13_Fig09.png)

With **link tracing**(including traceroute sampling -- see above), the inclusion probabilities are also harder to derive. Suppose however that the "traceroutes" (sampled paths from source nodes to target nodes) follow only **shortest-paths**, a fraction ρ_s of nodes is marked as sources while another fraction ρ_t of nodes is marked as targets. and b_i is the betweenness centrality of node i  while b_i,j is the betweenness centrality of edge (i,j). Then, the node and edge inclusion probabilities are approximately:

![M5L13_08](imgs/M5L13_08.png)

**Food For Thought**
- Search the literature to find at least an approximate expression for the inclusion probability of nodes and edges in two-step snowball sampling.


## Horvitz-Thompson Estimation of "Node Totals"

Let us now use the previous inclusion probabilities to estimate various "node totals", i.e., **metrics that are defined based on the summation of a metric across all nodes.**

![M5L13_09](imgs/M5L13_09.png)

For instance, if y_i is the degree of each node, then we can use τ to calculate the average node degree (just divide τ by the number of nodes). 

Or, if y_i represents whether a node of a social network is a bot or not (a binary variable), then τ is the total number of bots in the network. 

Or, if y_i represents some notion of node capacity, then τ is the total capacity of the network. 

Suppose that we have a sample S of n network nodes, and we know the value y_i of each sampled node. 

An important result in statistics is the **Horvitz-Thompson** estimator. It states that we can estimate  as follows:

![M5L13_10](imgs/M5L13_10.png)

where π_i is the inclusion probability of node-i, as long as π_i > 0 for all nodes in the sample S.

In other words, to estimate the "total value" we should not just add the values of the sampled nodes and multiply the sum by N/n (that would be okay only if π_i = n/N for all i). Instead, we need to normalize the value of each sampled node by the probability that that node is sampled. 


A key property of the Horvitz-Thompson estimator is that it is unbiased, as shown below. Let us define as Z_i an indicator random variable that is equal to one if node i is sampled in S -- and 0 otherwise. Then,

![M5L13_11](imgs/M5L13_11.png)

An estimate of the variance of this estimator can be calculated from the sample S as follows:

![M5L13_12](imgs/M5L13_12.png)

where π_i,j is the probability that both nodes i and j are sampled in S (when i=j, we define that π_i,j = π_i). 

**Food For Thought**
- Suppose that i and j are sampled independently. How would you simplify the previous expression for the variance of the Horvitz-Thompson estimator?

## Estimating the Number of Edges and the Average Degree

We can also apply the Horvitz-Thompson estimator to "totals" that are defined over all possible node pairs. Here, each node pair (i,j) belong to V x V is associated with a value y_i,j, and we want to estimate the total value:

![M5L13_13](imgs/M5L13_13.png)

For instance, if y_i,j is one when the two nodes are connected and zero otherwise, the total τ is twice the number of edges in the population graph. 

Another example: it could be that y_i,j is one if the shortest path between the two nodes traverses a given node-k (and zero otherwise). Then, the value τ is related to the betweenness centrality of node-k. 

Suppose that we have a sample S of n node pairs,  with the corresponding values y_i,j of the sampled node pairs. The Horvitz-Thompson estimator tells us that an unbiased estimator of τ is:

![M5L13_14](imgs/M5L13_14.png)

Let us first apply this framework in **estimating the number of edges in the population graph**. Suppose that we perform induced subgraph sampling, starting with n nodes chosen without replacement. Then, the probability of sampling a node pair (i,j) of the population graph is 

![M5L13_15](imgs/M5L13_15.png)

So, our estimate for the number of edges in the population graph is:

![M5L13_16](imgs/M5L13_16.png)

which means that we just need to multiply the number of sampled edges |E*| by a correction factor (the inverse of the fraction of sampled node pairs).   

![M5L13_Fig10](imgs/M5L13_Fig10.png)
*Image source: Kolaczyk, Eric D. Statistical Analysis of Network Data: Methods and Models (2009) Springer Science+Business Media LLC.*

To put these results in a more empirical setting, the plot shows the results of estimating the network density (and thus the number of edges) in the Yeast protein-protein interaction network, when using induced subgraph sampling. The actual number of edges is 31,201 (and the number of nodes is 5,151). The plots at the left show the empirical distribution of the sampled number of edges |E*| for three node sampling fractions: p=0.10, 0.20 and 0.30 of the total number of nodes. The plots at the right show the empirical distribution of the standard error (estimate of standard deviation of |E*|). All distributions are based on 10,000 trials. 

Let us now use this result to also **estimate the average degree** E[k] **in the population network**. Recall that the average degree is related to the number of nodes N and the number of edges |E| as follows:

E[k] = 2|E|/N

Using the previous estimate for the number of edges at the sampled graph, we get the following estimate for the average degree of the network:

![M5L13_17](imgs/M5L13_17.png)

![M5L13_18](imgs/M5L13_18.png)

**Food For Thought**
- Recall that the Transitivity of a graph requires us to calculate the number of connected node triplets and triangles. How would you apply the previous framework to estimate these quantities from a sample of the graph, using induced subgraph sampling?

## Estimating the Number of Nodes with Traceroute-like Methods


Let us now see how we can use a traceroute-like sampling strategy to estimate the number of nodes in a large network. Suppose that we have a set of sources S = {s_1, s_2, ..., s_n_s} and a set of targets T = {t_1, t_2, ..., t_n_r}. Each traceroute starts from a source node and it traverses a network path to a target node, also "observing" (sampling) the intermediate nodes in the path. Note that an intermediate node may be observed in more than one traceroute path. The number of  observed nodes is denoted by N*, while the number of nodes in the population network is denoted by N. How can we use N* to estimate N?

Here is the key idea in the following method: suppose that we drop a given target node t_j from the study. Would that target be observed in the traceroute paths to the remaining targets? We can easily measure the fraction of targets that would be observed in this manner. Intuitively, the lower this fraction is, the larger the population network we are trying to sample. Is there a way we can use this fraction to "inflate" N* so that it gives us a good estimate of N? As we will see next, the answer is yes through a simple mathematical argument. 

![M5L13_19](imgs/M5L13_19.png)

![M5L13_20](imgs/M5L13_20.png)

Let us now see what happens when this method is applied to estimate the size of an older snapshot of the Internet that included N=624,324 nodes and 1,191,525 edges. The number of sources was n_s = 10. The "target density", defined as ρ_T = n_T / N, is shown as the x-axis variable in the following graph. The y-axis shows the fraction of the estimated network size over the actual network size (ideally it should be equal to 1). The solid dots show the estimates from the method we described above (including intervals of +- one standard deviaton around the mean). The open dots show the same fraction if we had simply estimated the size of the network based on N*. 


![M5L13_Fig11](imgs/M5L13_Fig11.png)
*Image source: Kolaczyk, Eric D. Statistical Analysis of Network Data: Methods and Models (2009) Springer Science+Business Media LLC.*

Note that the previous method is fairly accurate even if the target density is as low as 0.005 (i.e., about 3,100 targets). 

On the contrary, if we had estimated the size of the network simply based on the number of observed nodes we would grossly underestimate how large the network is (unless if our targets pretty much cover all network nodes). 

**Food For Thought**
- If you could place the n_S sources anywhere you want, where would you place them to improve this estimation process?

## Topology Inference Problems

Let us now move from network sampling to a different problem: **topology inference**. How can we estimate the topology of a network from incomplete information about its nodes and edges?

The problem of topology inference has several variations, described below. The following visualization helps to illustrate each variation. The top-left figure shows the actual (complete) network, which consists of five nodes and five edges (shown in solid dark blue) -- the dotted dark blue lines represent node pairs that are NOT connected with an edge. The topology inference problems we will cover in the following pages are:

![M5L13_Fig12](imgs/M5L13_Fig12.png)
*Image source: Kolaczyk, Eric D. Statistical Analysis of Network Data: Methods and Models (2009) Springer Science+Business Media LLC.*

- **a) Link prediction:** As shown in the top-right figure, in some cases we know that certain node pairs are connected with an edge (solid dark blue) or that they are NOT connected with an edge (dotted dark blue) -- but we do not know what happens with the remaining node pairs. Are they connected or not? In the top-right figure those node pairs are shown with solid light blue and dotted light blue, respectively. We have already mentioned the link prediction problem in Lesson-12, in the context of the Hierarchical Random Graph (HRG) model. Here, we will see how to solve this problem even if we do not model the network using HRG. 

- **b) Association networks:** As shown in the bottom-left figure, in some cases we know the set of nodes but we do not have any information about the edges. Instead, we have some data about various characteristics of the nodes, such as their temporal activity or their attributes. Imagine, for instance, that we know all the characteristics, hobbies, interests, etc, of the students in a class, and we try to infer who is friend with whom. We can use the node attributes to identify pairs of nodes that are highly similar according to a given metric. Node pairs that are highly similar are then assumed to be connected. 

- **c) Network tomography:** As shown in the bottom-right figure, in some cases we know some nodes (shown with red) but we do not know about the existence of some other nodes (shown in pink) -- and we may not know the edges either. This is clearly the hardest topology inference problem in networks but sufficient progress has been made in solving it as long as we can make some "on-demand path measurements" from the nodes that we know of. Additionally, the problem of network tomography is significantly simpler if we can make the assumption that the underlying network has a tree topology (this is not the case in the given example).

## Link Prediction

Let us introduce the Link Prediction problem with an example. The following network refers to a set of 36 lawyers (partners and associates) working for a law firm in New England. Two lawyers are connected with an edge if they indicated (through a survey) that they have worked together in a case. We know several attributes for each lawyer: seniority in the firm (indicated by the number next to each node), gender (nodes 27, 29, 34 are females), office location (indicated by the shape of the node -- there are three locations in the dataset), and type of practice (red for Litigation and cyan for Corporate Law). 

![M5L13_Fig13](imgs/M5L13_Fig13.png)
*Source: “Statistical analysis of network data” by E.D.Kolaczyk. http://math.bu.edu/ness12/ness2012-shortcourse-kolaczyk.pdf*

Suppose that we can observe a portion of this graph -- but not the whole thing. How would you infer whether two nodes that appear disconnected are actually connected or not? Intuitively, you can rely on two sources of data:

- a) The first is the node attributes -- for instance, it may be more likely for two lawyers to work together if they share the same office location and they both practice corporate law.

- b) The second is the topological information from the known edges. For instance, if we know that A and B are two nodes that do not share any common neighbors, it may be unlikely that A and B are connected. 

Let us start by stating an important assumption. In the following, we assume that the missing edges are **randomly missing** -- so, whether an edge is observed or not does not depend on its own attributes. Without this assumption, the problem is significantly harder (imagine solving the link prediction problem in the context of a social network in which certain kinds of relationships are often hidden). 

Let us first define some topological metrics that we can use as "predictor variables" or "features" in our statistical model. Consider a node i, and let N_i_obs be the set of its observed neighbors. As we have seen, many networks in practice are highly clustered. This means that if two nodes have highly overlapping observed neighbors, they are probably connected as well.

![M5L13_21](imgs/M5L13_21.png)

Here, node k is a common neighbor of both i and j. The idea is that if k is highly connected to other nodes, it does not add much evidence for a connection between i and j. If k is only connected to i and j though, it makes that connection more likely. This metric is sometimes referred to as "Adamic-Adar similarity". 

There are several more topological similarity metrics in the literature -- but the previous two give you the basic idea. 

Together with topological similarity scores s(i,j), we can also use the node attributes to construct additional predictor variables for every node pair. Returning to the lawyer collaboration example, we could define, for instance, the following five variables:

![M5L13_22](imgs/M5L13_22.png)

Many other such predictor variables can be defined, based on the node attributes and topological similarity metrics. 

Now that we have defined these predictor variables for each pair of nodes, we can design a *binary classifier* using *Logistic Regression*.

![M5L13_23](imgs/M5L13_23.png)

where Y_i,j = 1 means that the edge between nodes i and j actually exists (observed or missing) -- while Y_i,j = 0 means that the edge does not exist. The vector z includes all the predictor variables for that node pair (we defined six such variables above). Finally, the vector β is the vector of the regression coefficients, and it is assumed to be the same for all node pairs. 

A logistic regression model can be trained based on the observed data Y_obs, calculating the optimal vector of regression coefficients β. If you want to learn how this optimization is done computationally, please refer to any machine learning or statistical inference textbook. 

After training the model with the observed node pairs (connected or not), we can use the logistic regression model to predict whether any missing node pair is actually connected using the following equation:

![M5L13_24](imgs/M5L13_24.png)

This equation follows directly from the logistic regression model. For instance, if the previous probability is larger than 0.5 for a node pair (i,j), we can infer that the two nodes are connected.

The link prediction problem is very general and any other binary classification algorithm could be used instead of logistic regression. For example, we could use a support vector machine (SVM) or a neural network. 

**Food For Thought**
- In the link prediction framework we presented here, the logistic regression coefficients are the same for every node pair. What does this mean/assume about the structure of the network? How would you compare this approach with link prediction using the HRG modeling approach we studied in Lesson-12? 

## Association Networks

In some cases we know the nodes of the network -- but none of the edges! Instead, we have some observations for the state of each node, and we know that the state of a node depends on the state of the nodes it is connected with.

For instance, in the context of climate science, the nodes may represent different geographical regions. For each node we may have measurements of temperature, precipitation, atmospheric pressure, etc over time. Further, we know that the climate system is interconnected, creating spatial correlations between different regions in terms of these variables (e.g., the sea surface temperate at the Indian ocean is strongly correlated with the sea surface temperature at an area of the Pacific that is west of central America). How would you construct an "association network" that shows the pairs of regions that are highly correlated in terms of each climate variable?

Recall that we had examined this problem in Lesson-8, when we discussed the δ-MAPS method. Our focus back then however was on the community detection method to identify regions with homogeneous climate variables (i.e., the nodes of the network) -- and not on the association network that interconnects those regions.

To introduce the "association network" problem more formally, suppose that we have N nodes, and for each node i we have a random vector X_i of independent observations. We want to compute an undirected network between the N nodes in which two nodes i and j are connected if X_i and X_j are sufficiently "associated". 

To solve this problem, we need to first answer the following three questions:

- a) How to measure the association between node pairs? What is an appropriate statistical metric?

- b) Given an association metric, how can we determine whether the association between two nodes is statistically significant? 

- c) Given an answer to the previous two questions, how can we detect the set of statistically significant edges while we also control the rate of false positives? (i.e., spurious edges that do not really exist)

**Let us start with the first question: which association metric to use?s**

The simplest association metric between X_i and X_j is Pearson's correlation coefficient:

![M5L13_25](imgs/M5L13_25.png)

where μ_i and σ_i are the mean and standard deviation of X_i, respectively. As you probably know, this coefficient is more appropriate for detecting linear dependencies because its absolute magnitude is equal to 1 if the two vectors are related through a (positive or negative) proportionality. If X_i and X_j are independent, then the correlation coefficient is zero (but the converse may not be true). 

Instead of Pearson's correlation coefficient, we could also use *Spearman's rank correlation* (it is more robust to **outliers**), *mutual information* (it can detect non-linear correlations) or several other statistical association metrics. Additionally, we could use partial correlations to deal with the case that both X_i and X_j are affected by a third variable X_k -- the partial correlation of X_i and X_j removes the effect of X_k from both X_k and X_j.

To keep things simple, in the following we assume that the association between X_i and X_j is measured using Pearson's correlation coefficient. 

**Second question: when is the association between two nodes statistically significant?**

The "null hypothesis" that we want to evaluate is whether the correlation between X_i and X_j is zero (meaning that the two nodes should not be connected) or not:

H_0 : ρ_i,j = 0 versus H_1: ρ_i,j != 0

To answer this question rigorously, we need to know the statistical distribution of the metric ρ_i,j under the null hypothesis H_0. 

First let us apply the **Fisher transformation** on ρ_i,j so that, instead of being limited in the range [-1,+1], it varies monotonically in (-inf, inf):

z_i,j = 1/2 log((1 + ρ_i,j)/(1 - ρ_i,j))

Here is a relevant result from Statistics: if the two random vectors (X_i, X_j) are uncorrelated (i.e., under the previous null hypothesis H_0) and if they follow a bivariate Gaussian distribution, the distribution of the Fisher-transformed correlation metric z_i,j follows the Gaussian distribution with zero mean and variance 1/(m - 3), where m is the length of the X_i vector. 

Now that we know the distribution of under the null hypothesis, we can easily calculate a p-value for the correlation between X_i and X_j. Recall that the p-value represents the probability that we reject the null hypothesis H_0 even though it is true. So, we should state that X_i and X_j are significantly correlated only if the corresponding p-value is very small -- typically less than 1% or so. 

To illustrate what we have discussed so far, the following figure shows a scatter plot of m=445 observations for the expression level of two genes at the bacterium Escherichia coli (E. coli). The two genes are tyrR and aroG. The expression levels were measured with microarray experiments (log-relative units). The correlation metric between the two expression level vectors is ρ = 0.43. Is this value statistically significant however?  

![M5L13_Fig14](imgs/M5L13_Fig14.png)
*Image source: Kolaczyk, Eric D. Statistical Analysis of Network Data: Methods and Models (2009) Springer Science+Business Media LLC.*

![M5L13_26](imgs/M5L13_26.png)

**Third question: how to control the rate of false positive edges?**

You may be thinking that we are done -- we now have a way to identify statistically significant correlations between node pairs. So we can connect nodes i and j with an edge if the corresponding p-value of the null hypothesis for ρ_i,j is less than a given threshold (say 1%).  What is the problem with this approach?

Suppose that we have N = 1000 nodes, and thus m = N(N-1)/2 = 499,500 potential edges in our network. Further, suppose that a correlation ρ_i,j is considered significant if the p-value is less than 1%. Consider the extreme case that none of these pairwise correlations are actually significant. This means that if we apply the previous test 499,500 times, in 1% of those tests we will incorrectly reject the null hypothesis that ρ_i,j = 0. In other words, we may end up with 4,995 spurious edges that are false positives!

This is a well-known problem in Statistics, referred to as the Multiple Testing problem. A common way to address it is to apply the False Discovery Rate (FDR) method of Benjamini and Hochberg, which aims to control the rate α of false positives. For instance, if α = 10^-6, we are willing to accept only up to one-per-million false positive edges. 

Specifically, suppose that we sort the p-values of the m = N(N-1)/2 hypothesis tests from lowest to highest, yielding the sequence p(1) <= p(2) <= ... p(m).  The Benjamini-Hochberg method finds the highest value of k belong to {1 ... m} such that

p(k) <= k/m \* α

if such a p-value exists -- otherwise k = 0.

![M5L13_Fig15](imgs/M5L13_Fig15.png)
*Image source: "The power of the Benjamini-Hochberg procedure", by W. van Loon, http://www.math.leidenuniv.nl/scripties/MastervanLoon.pdf*

The null hypothesis for the tests with the k lowest p-values is rejected, meaning that we only "discover" those k edges. All other potential edges are ignored as not statistically significant. Benjamini and Hochberg proved that if the m tests are independent, then the rate of false positives in these k detections is less than α.

In our context, where the m tests are applied between all possible node pairs, the test independence assumption is typically not true however. There are more sophisticated FDR-control methods in the statistical literature that one could use if it is important to satisfy the α constraint. 

**Food For Thought**
- a) Explain why the m tests are probably not independent in the context of association network inference.
- b) What would you do if the Benjamini-Hochberg method gives k=0 for the value of α that you want. You are not allowed to increase α. 

## Topology Inference Using Network Tomography

Let us now focus on topology inference using *network tomography*. In medical imaging, "tomography" refers to methods that observe the internal structure of a system (e.g., brain tissue) using only measurements from the exterior or "periphery" of the system. In the context of networks, the internal structure refers to the topology of the network (both nodes and edges), while the "periphery" is few nodes that are observable and that we can utilize to make measurements.

For instance, in the following tree network the observable nodes are the root (blue) and the leaves (yellow) -- all internal nodes (green) and the edges that interconnect all nodes are not known.

![M5L13_Fig16](imgs/M5L13_Fig16.png)
*Image source: Kolaczyk, Eric D. Statistical Analysis of Network Data: Methods and Models (2009) Springer Science+Business Media LLC.*

To simplify, we will describe network tomography in the context of computer networks, where the nodes represent computers or routers and the edges represent transmission links. Please keep in mind however that network tomography methods are quite general and they are also applicable in other contexts, such as the *inference of phylogenetic trees in biology*. 

Let us first review a basic fact about computer networks. When a packet of size L bits is transmitted by a router (or computer) on a link of capacity C bits-per-second, the transmission takes L/C seconds. So, if we send two packets of size L at that link, the second packet will have to wait at a router buffer for the transmission of the first packet -- and that waiting time is L/C.

We can use this fact to design a smart measurement technique called "sandwich probing". Consider the previous tree network. Suppose that the root node sends three packets P1,P2,P3 at the same time. Packets P1 and P3 are small (the minimum possible size) and they are destined to one of the observable nodes, say R_i, while the intermediate packet P2 is large (the largest possible size) and it is destined to another observable node R_j. The packets are timestamped upon transmission, and the receiving node R_i measures the end-to-end transfer delay that the two small packets experienced in the network. 

If the destinations R_i and R_j are reachable from the root through completely different paths (e.g., such as the leaves 1 and 5 in the previous tree), packet P3 will never be delayed due to packet P2 because the latter follows a different path. So, the transfer delays of the two small packets P1 and P3 will be very similar. The absolute difference of those two transfer delays will be close to 0. 

On the contrary, if R_i and R_j are reachable through highly overlapping paths (such as the leaves 1 and 2 in the previous tree), packet P3 will be delayed by the transmission delay of packet P2 at every intermediate router in the overlapping segment of the two paths. The more the intermediate routers in the common portion of the paths to R_i and R_j, the larger the extra transfer delay of packet P3 relative to the transfer delay of packet P1. 

Let us denote by d_i,j the difference between the transfer delays of packets P1 and P3 when they are sent to destination R_i, while the large packet P2 is sent to destination R_j. This metric carries some information about the overlap of the network paths from the root node to R_i and R_j. In practice, we would not send just one "packet sandwich" for each pair of destinations -- we would repeat this 1000s of times and measure the average value of d_i,j. 

![M5L13_Fig17](imgs/M5L13_Fig17.png)
*Image source: Kolaczyk, Eric D. Statistical Analysis of Network Data: Methods and Models (2009) Springer Science+Business Media LLC.*

The previous figure visualizes these average delay differences for an experiment in which about 10,000 "packet sandwiches" were sent from a computer at Rice University in Texas to ten different computers: two of them also at Rice, others at other US universities and two (IST and IT) in Portugal. The darker color represents lower values (closer to 0), while the brighter color represents higher values. Note, for instance, that when we send the small packets to one of the Rice destinations and the large packet to a destination outside of Rice, the delay difference is quite low (relative to the case that both destinations are outside of Rice).

Now that we understand that we can use d_i,j as a metric of "path similarity" for every pair of destinations, we can apply a *hierarchical clustering algorithm* to infer a binary tree, rooted at the source of the packet sandwiches, while all destinations reside at the leaves of the tree. Recall that we used hierarchical clustering algorithms in Lesson-7 for community detection. Here, our goal is to identify the binary tree that "best explains" the delay differences d_i,j, across all possible pairs i and j.

The algorithm proceeds iteratively, creating a new internal tree node in each iteration. At the first iteration, it identifies the two leaves i and j that have the largest delay difference (i.e., the two destinations that appear to have the highest network path overlap), and it groups them together by creating an internal tree node a(i,j) that becomes the parent of nodes i and j. Then, the delay difference between the new node a(i,j) and any other leaf k is calculated as the average of the delay differences d_i,k and d_j,k (i.e., we use "average linking"). The two leaves i and j are marked as "covered", so that they are not selected again in subsequent iterations. 

The algorithm proceeds until all nodes, both the original leaves and the created intermediate nodes, are "covered". 

In the following figure we show what happens when we apply this method in the previous delay difference data. The top visualization shows the actual network paths from the source node at Rice to the ten destinations. The bottom visualization shows the result of the hierarchical clustering algorithm we described earlier. 

![M5L13_Fig18](imgs/M5L13_Fig18.png)
*Image source: Kolaczyk, Eric D. Statistical Analysis of Network Data: Methods and Models (2009) Springer Science+Business Media LLC.*

Note that a first difference between the "ground truth" network and the inferred network is that the latter is a binary tree, while the former includes branching nodes (routers) with more than two children. There is also a router (IND) that is completely missing from the inferred network, probably because it is so far that it does not cause a measurable increase in the delay of packet P3. 

 
**Food For Thought**
- a) The metric we have introduced here is based on delay variations using "packet sandwiches". Can you think of other ways to probe a network in order to measure the topological overlap of different paths?
- b) The network inference method we used here is based on hierarchical clustering. Can you think of other network inference methods we could use instead? (Hint: remember what we did in Lesson-12 with the dendrogram of the HRG model)

## Other Network Estimation and Tomography Problems
![M5L13_Fig19](imgs/M5L13_Fig19.png)
![M5L13_Fig20](imgs/M5L13_Fig20.png)
![M5L13_Fig21](imgs/M5L13_Fig21.png)
![M5L13_Fig22](imgs/M5L13_Fig22.png)
![M5L13_Fig23](imgs/M5L13_Fig23.png)

## Lesson Summary

This lesson focused on the use of statistical methods in the analysis of network data. This is a broad area, with many different topics. We mostly focused on two of them:

- a) **network sampling:** design of different sampling strategies and inference of network properties from those samples,

- b) **topology inference:** detecting the presence of missing links, creating association networks, and using tomography methods to discover the topology of the network. 

Here is a list of other topics in the statistical analysis of network data:

- Statistical modeling and prediction of dynamic processes on networks (i.e., applying statistical methods such as Markov Random Fields or Kernel-Based Regression on dynamic processes on networks such as epidemics)
- Analysis and design of directed and undirected graphical models
- Efficient algorithms for the computation of network motifs from sampled network data
- Efficient algorithms for the computation of centrality metrics and communities.

In parallel, some of these problems are also pursued by the Machine Learning community, as we will see in the next lesson. 





























