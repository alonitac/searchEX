# Uninformed Search

## Question 1

**(a)** 

<u>1 Cannibal 1 Missioner in a boat</u>  

$$(c, m, 1 | (m\le2) \and(c\le2))\rightarrow(c+1, m+1,0)$$

$$(c, m, 0 | (m\ge1) \and(c\ge1))\rightarrow(c-1, m-1,1)$$

<u>1 Missioner in a boat</u>

$$(c, m, 0| (m=1 \and c=1) \or(m=3\and c=2))\rightarrow(c, m-1, 1)$$

$$(c, m, 1 | (m=2) \or(m=0 \and c=1)\rightarrow(c, m+1, 0)$$

<u>1 Cannibal in a boat</u>

$$(c, m, 0| (c\ge 1) \and(m=0 \or m=3))\rightarrow(c-1, m, 1)$$

$$(c, m, 1 | (c\le2) \and(m=3 \or m=0)\rightarrow(c+1, m, 0)$$

<u>2 Cannibals in a boat</u>

$$(c, m, 0| (c\ge 2) \and(m=0 \or m=3))\rightarrow(c-2, m, 1)$$

$$(c, m, 0| (c\le 1) \and(m=0 \or m=3))\rightarrow(c+2, m, 0)$$

<u>2 Missioners in a boat</u>

$$(c, m, 0| (m\ge 2) \and(m=2 \or m-2\ge c))\rightarrow(c, m-2, 1)$$

$$(c, m, 0| (m\le 1) \and(m=1 \or m+2=c))\rightarrow(c, m+2, 0)$$



We have implemented short version of BFS, here are the solution (printed path from root to goal):

Reach goal!

 (3, 3, 0) -> (2, 2, 1) -> (2, 3, 0) -> (0, 3, 1) -> (1, 3, 0) -> (1, 1, 1) -> (2, 2, 0) -> (2, 0, 1) -> (3, 0, 0) -> (0, 1, 1) -> (1, 1, 0) -> (0, 0, 1)

**(b)**

No. By definition of constraints, transition from state to state is allowed only if the destination state is valid. Since we start with a valid state, all transitions are valid. 



## Question 2

Consider a locker that can be opened by a key of m digits, the range of each digit is 1,...,b. We assume that m and b are very large numbers. Let's say that **any given sequence of digits** will open the locker (such a dummy game). 

A state space represents this problem is a **full tree** (the leafs are goals). Clearly, the time complexity of IDDFS is $$\Omega(b^{m-1})$$ while in DFS is $$\Theta(bm)$$.



## Question 3

### a

True. UCS for a search space in which the cost of all actions is constant, would functioning as BFS. 

Note: it actually depends on the internal implementation of the priority queue. But for our purpose let's say that a priority queue for constant cost, function as a queue. 

### b

Consider the following tree:



![](/home/alon/Documents/studies/Computer science/intro to AI/ex1files/Untitled Diagram.png)

Best-first will return D while DFS will return C.

### c

Yes. When  $$\forall n, h(n)=0$$, A* is UCS by definition. 

## Informed Search

### 1

Consider the following graph:

![](/home/alon/Documents/studies/Computer science/intro to AI/ex1files/Untitled Diagram2.png)

It's easy to validate that for all n, $$h_1(n)\le h_2(n)\le h^*(n)$$, and that the two heuristics are consistent. Note that $$h_2 \equiv h^*$$. 

Nevertheless, $$A^*$$ with $$h_2$$ does not expand $$S_1$$ while with $$h_1$$ it does. 

### 2

Given a node n, we assume that there is a valid set of actions such that it is possible to reach a goal state from n, otherwise $$h^*(n)$$ is undefined.

Given a goal state g and a set of state $${n_1, n_2, .., n_k}$$ such that the path $$n-n_1-n_2-...-h_k-g$$ represents a valid set of actions from n to g. From consistency:

$$h(n)\le cost(n, n_1) + h(n_1)\le cost(n, n_1) + cost(n_1, n_2) + h(n_2)\le...$$

$$\le cost(n, n_1)+...+cost(n_k, g) +h(g)= cost(n, g) + h(g)=const(n,g)+0=h^*(n)$$

Thus $$h(n)\le h^*(n)$$ for all n for all paths from n to some goal state g. Thus $$h(n)$$ admissible by definition. 

##  Optimization

### a

$$min_{V_1,V_2}\Big[|\{(i,j)\in E: i\in V_1, j\in V_2\}|+abs(|V_1|-|V_2|)\Big]$$ s.t $$V_1,V_2$$ disjoint sets, and their union is $$V$$.

### b

We define states space by  {$$(V_1, V_2)$$ | $$V_1,V_2$$ disjoint sets, and their union is $$V$$}. The successors of  a state are all $$(V^{'}_1, V^{'}_2)$$ such that $$(V_1, V_2)$$ and $$(V^{'}_1, V^{'}_2)$$ are differ by moving one vertex from one group to another. Since we were required to perform gradient **ascent**, we would **maximize the minus** of the above optimization expression.  

### c

Since the minimization expression in (a) is not convex (it's easy to reduce the above problem to np problem learned in Algo), gradient ascent would not guarantee to find an optimal solution while simulated annealing   may find a better solution thanks to it's random feature.

 ### d



