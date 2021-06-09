## Ant colony optimization for continuous domains

Krzysztof Socha, Marco Dorigo

*IRIDIA, Universite´ Libre de Bruxelles, CP 194/6, Ave. Franklin D. Roosevelt 50, 1050 Brussels, Belgium*

Received 1 August 2005; accepted 1 June 2006

Available online 3 November 2006

**Abstract:** In this paper we present an extension of ant colony optimization (ACO) to continuous domains. We show how ACO, which was initially developed to be a metaheuristic for combinatorial optimization, can be adapted to continuous optimi- zation without any major conceptual change to its structure. We present the general idea, implementation, and results obtained. We compare the results with those reported in the literature for other continuous optimization methods: other ant-related approaches and other metaheuristics initially developed for combinatorial optimization and later adapted to handle the continuous case. We discuss how our extended ACO compares to those algorithms, and we present some analysis of its eﬃciency and robustness.

*Keywords:* Ant colony optimization; Continuous optimization; Metaheuristics

## 连续域的蚁群优化算法

### 摘要

本⽂提出了蚁群优化算法在连续域上的⼀种扩展。我们展示了蚁群算法是如何适应连续优化的，⽽不需要对其结构进⾏任何重⼤的概念改变，蚁群算法最初是作为组合优化的元启发式算法开发的。我们介绍了总体思路、具体实现和获得的结果。我们将结果与⽂献中报道的其他连续优化⽅法进⾏⽐较：其他与蚂蚁相关的⽅法和其他元启发式⽅法最初是为组合优化开发的，后来被⽤来处理连续情况。我们讨论了我们的扩展蚁群算法与这些算法的⽐较，并对其效率和鲁棒性进⾏了分析。

关键词:蚁群优化，连续优化，元启发式

### 引言

受蚂蚁觅⻝⾏为启发的优化算法最初被提出⽤于解决组合优化问题。典型的组合优化问题包括调度、⻋辆路线、时间表排等。这些问题中的很多问题，尤其是那些实际相关的问题，都是NP难的。换句话说，被认为不可能找到有效的(即多项式时间)算法来最优地解决它们。通常这些问题⽤启发式⽅法(即不精确的⽅法)来解决，允许在合理的时间内找到近似解(即较优的但不可证明是最优的解)。
组合优化，顾名思义，即处理寻找可⽤问题组成的最佳组合或排列。因此，需要将问题划分为有限的组成集合，组合优化算法试图找到它们的最佳组合或排列。许多现实世界的优化问题可以⽤简单的⽅式表示为组合优化问题。然⽽，有⼀类重要的问题并⾮如此：需要为连续变量选择值的优化问题。只有允许值的连续范围被转换成有限集合时，才可以⽤组合优化算法来解决这些问题。⼤多情况下这并不便捷，尤其是如果初始范围很⼴的情况下，并且所需的划分精度⾮常⾼。在这些情况下，那些天⽣处理连续变量的算法通常性能更好。本⽂提出了⼀种有效的⽅法，使得蚁群优化算法应⽤于连续优化问题。

⾃从蚁群算法作为⼀种组合优化⽅法出现以来，⼈们⼀直试图⽤它来解决连续问题。然⽽，将蚁群元启发式算法应⽤于连续领域并不简单，所提出的⽅法通常从蚁群算法中获得灵感，但并不完全遵循它。

与早期的⽅法相反，本⽂提出了⼀种将蚁群算法扩展到连续域的⽅法，⽽不需要对其结构进⾏任何重⼤的概念改变。为了提⾼论⽂的简洁性，我们⽤表示推⼴到连续域的蚁群算法。我们旨在展示应⽤于连续域的的核⼼思想，以及其在标准基准测试问题上良好的实现。

