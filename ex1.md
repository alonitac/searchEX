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


## CSP

**Terminology:**

- $S = \{s_1, ..., s_n\}$  total set of stations
- $T = \{t_1, ... , t_k\}$  total set of trains, which equals total set of routes since every train is assigned to one route only and vice versa
- for train $i \, \epsilon \, [1, ..., k]$: $S^i \subseteq S$, ordered sequence of stations of train i, called "route of train i", $|S^i| = n_i \leq n \, \forall \, i$ (how many stops train i has (including first station)).
- $travel(A, B) =  travel(B, A)$  for $A, B \, \epsilon \, S$ the time in minutes that it takes for the train to go from station $A$ to station $B$ or vice versa.

**Variables:**

$D = \{d_1, ... , d_k\}$ departure time-tables of each of the k trains. Where every $d_i \,  i \, \epsilon \, [1, ..., k]$ is the sequence of departure times for train i. The time of departure of the first station is given, the others are the ones that we optimize with the CSP. 

**Domains:**

The maximal possible domain is the discrete interval $[0, q]$, where q is the end of the period of interest. For example if we want to find the schedule for a full day, the interval is $[1, 1440)$, where every number i represents the i-th minute of the day.  

But the actual domain of interest for each variable is an dynamic interval, which means that if we want to find $d_{i,n}$ (the departure time at the n-th station of train i), we already know that the domain of $d_{i,n}$ can be reduced to $[\, (d_{i,n-1} + travel(s_{i,n-1}, s_{i,n}) + 1), 1440 \, )$ ("departure time at previous station plus the time that it takes to travel from previous to current station plus at least one minute waiting time at current station"). This reduction of the interval makes the search faster.

**Representation:** 

To have a nice lookup-table to check the conditions we will store the departure times as a Matrix, where each row is a train/route ($T$), and the columns are stations ($S$). Note that the columns are ordered exactly like $S = \{s_1, ..., s_n\}$, they include all stations of the map. This means that for every train usually some stations will have no departure time, since the train does not pass by this station. Also the stations are not necessarily visited in ascending order. 

To make things a bit more convenient for later, we don't only store the departure time $d_j$ of station j , but also the arrival time at the station. The arrival $a_j$ at station j is merely calculated as $a_j = d_{j-i} + travel(s_{j-1}, s_{j})$. The times are represented as a tuple ($a_j, d_j$). See the following table as an example:

| (i, j) | $s_1$  | $s_2$  | $s_3$  | $s_4$   | $s_5$  |
| ------ | ------ | ------ | ------ | ------- | ------ |
| $t_1$  | (0, 1) | (2, 3) | -      | (6, 10) | -      |
| $t_2$  | (4, 5) | (2, 3) | (6, 8) | -       | (0, 1) |

Train $t_1$ visits stations ($s_1$, $s_2$, $s_4$) in this order, $t_2$ visits stations ($s_5$, $s_2$, $s_1$, $s_3$).

To draw the connection between this Matrix and the graph, we use the route-sequence $S^i$. We can call $S^i(j)$ to get the j-th station of the route and similarly $S^i(j+1)$ when we are interested in which station the train goes to next. 

**Constraints:**

- **Waiting time at station is at least 1 minute:** This is taken care of in the reduction of the domain to each variable. The smallest possible value for the new departure time $d_n$ is $(d_{n-1} + travel(d_{n-1}, d_n) + 1)$. 

- **At any given time only one train at track between stations**: This means that when we're choosing a new departure time, we first need to see, if the next part of the line is empty. In the table above $t_1$ does the following: 

  (1: leave $s_1$, 1-2: travel line $(s_1, s_2)$, 2: arrive at $s_2$, 2-3: wait at $s_2$, 3: leave $s_2$) and t_2: 

  (before 3: stand at $s_3$, 3: leave $s_2$, 3-4: travel line $(s_2, s_1)$, 4: arrive at $s_1$, 4-5: wait at $s_1$). 

  Let's say we wanted to assign the departure time of train $t_2$ from station $s_2$ to station $s_1$. The departure time is at least 3 (check domain of variable, dependence of domain on previous departure). Then check if at time 3 the line between $(s_2, s_1) = (s_1, s_2)$ is empty and will stay empty for the full traveling time. This means that neither a train from $(s_1 \rightarrow  s_2)$ nor $(s_2 \rightarrow  s_1)$ are allowed to leave between $[(3 - travel(s_1, s_2)), (3+ travel(s_1, s_2))$] = [2, 4], because this would lead to an overlap in the time that the train spend on the line between the stations. In this example $t_1$ already left the relevant part of the track at time 2, so the line is empty and $t_2$ can enter it at time 3. 

  If the line would not have been empty we need to adjust the waiting time of one of the correlating trains. It is most promising to choose the train that crosses the least other trains in the future so that his delay does not effect too many other trains (something like reverse degree heuristics)

- **Stations can only accommodate a certain amount of trains**: This works via forward checking. Before we choose a departure time, we need to check if we can enter the next station when we arrive there. In the table for example we know that train $t_1$ waits at $s_2$ from minute 2-3. When we now assign the departure time of $t_2$ when it wants to travel from $(s_5 \rightarrow  s_2)$, we know that $travel(s_5, s_2) = 1$, so it will arrive at $s_2$ one minute after the departure time. We choose departure time 1 and check if the station is empty enough when we arrive there. We see that train $t_1$ is also waiting from time 2-3 at the station, but luckily $s_2$ can accommodate up to two trains, so we can let $t_2$ leave $s_5$ at time 1.

  In case the goal station is already full, we again choose the train that crosses the least other trains in the future and adjust his prior departure time accordingly.

Of course both adjustments to previous departure times can always cause other problems that haven't been there before. This is why this algorithm may need a lot of repetitive forward checking. 

Since **the departure time at the first station** is given for every train, we start with the train that starts the earliest. First give him the most optimistic departure times (always one minute after arrival), then we go to the train that leaves the second and move on until we break a constraint and solve it with adjustments. 

If the departure times would not have been fix, it would make sense to start at a junction of many trains and then move outwards from there (pick the variables that have the highest degree and assign values that violate the least constraints (Degree Heuristic and Minimum-Conflict). But in our case the Degree Heuristic does not make sense here. Only because some trains cross at some point on the map, it does not mean that they cross each other at any close time. So they do not actually (time and space) interfere with each other. 

**Optimization:** We want to find the solution where the sum of all the waiting times at the stations are minimized. Which is $W = \sum_{i \epsilon [1, ... , k]} \sum_{j \epsilon [1, ..., n]} w_{i,j} $, where $w_{i, j} = d_{i,j} - a_{i,j}$ . This is mainly achieved through the fact that we try the values for the departure time in increasing order, starting with the smallest possible one. Also if it comes to a conflict (because of a non-empty line between two stations or because a station already accommodates the maximum amount of trains), we adjust the waiting time of the one train, that interferes with less trains in the future. 

