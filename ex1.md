## Question 2

Consider a locker which can be opened by a key of m digits, the range of each digit is 1,...,b. We assume that m and b are very large numbers. Let's say that **any given sequence of digits** will open the locker, such a dummy game. 

A state space represents this problem is a **full tree** (the leafs are goals). Clearly, the time complexity of IDDFS is $$\Omega(b^{m-1})$$ while in DFS is $$\Theta(bm)$$.



## Question 3

### a

True. UCS for a search space in which the cost of all actions is constant, would function as BFS. 

Note: it actually depends on the internal implementation of the priority queue. But for our purpose let's say that a priority queue for constant cost, function as a queue. 

### b

No. Consider a search space with evaluation function f(n). No one guarantee that order f(n) imply correspond with the order of DFS (unless we artificially build such f(n)). 

### c

Yes. When  $$\forall n, h(n)=0$$, A* is UCS by definition. 

## Informed Search

### 1

### 2

We will proof by induction on the n - number of nodes. 

For n = 2, where $$n_2$$ is the goal state: from consistency, it holds that $$h(n_1)\le cost(n_1,n_2) + h(n_2)$$. Since $$cost(n_1,n_2)=h^*(n_2)$$ and $$h(n_2)=0$$, we get that $$\forall n, h(n)\le h^*(n)$$, thus $$h(n)$$ is admissible by definition.  

Consider an induction assumption that for every graph with n nodes, for every consistent h $$\Rightarrow$$ h is admissible. Given a graph $$(V,E)$$ with n+1 nodes and consistent heuristic function h, we pick one node, S, and remove it and all it's edges. By the induction assumption, we know that h is admissible upon the remaining graph. 

## Optimization

### a

$$min_{V_1,V_2}\Big[|\{(i,j)\in E: i\in V_1, j\in V_2\}|+abs(|V_1|-|V_2|)\Big]$$ s.t $$V_1,V_2$$ disjoint sets, and their union is $$V$$.

### b

