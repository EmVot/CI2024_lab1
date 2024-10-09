# CI2024_lab1
Laboratory 1 for Computational Intelligence Course

# Overview
## Presentation
In this laboratory the hill-climber approach for boulig a solution for the 'Cover Set' problem is presented.
Additionally to the original .ipynb file provided, I added an Utlity funcion called:
'covered_elements'
which returns the total coverage of the until-built solution.
## Code implementation
The solution presented firstly implement a greedy algorithm and a brute-force one to make comparisons with the solutions found by the different approaches of the hill-climbing algorithms implemented.
Since the brute-force algorithm is unfeasible with all the configuartions apart from the first one, i recomend to use the greedy-one in case you want to repeat the experiments.
The file contains different approaches to the problem using basically two approaches of the hill-climber structure:

* A bottom-up solution-building: starts from none sets taken and add a random one until it achieve a complete coverage
* A top-down solution approach: starts with the dumb solution of all the sets taken and eliminates a random one while leaving the solution valid; the process is repeated till we achieve a target 'threshold', which represents the ratio between the considered set-cover and the total number of sets, or the process cannot find another item to take away in a 'time_limit' iterations number.

The former two method implement a very basic implementation with no regards of the costs, which aims to study, for the problem in question, which advantages and disadvantages the two approaches present.
The latter takes in consideration the 'cost' variable, and make more af an 'aimed' decicision while building the solution (exploitation-driven method)

## Considerations
While little is to be said about the first two algorithms, I feel some conisderations need to be made regarding the last one;
First of all with this implementation we explare the 'balance' between explaration and exploitation:
* The code can handle multiple indexes candidates before doing its choice based on the cost; this somehow implements the idea of 'wideness' of exploration: the higher the cardinality of the array containing the cantidate indexes, the further the algorithm is able to see beyond its local landscape, potentially beign able to find at some point the optimal solution; it's clear anyway that in a context where the jump is made randomically, the more the algorithm is able to jump from one state to another, the more it is unstable, resulting eventually in a search time which equals the brue-force one in terms of worst-case-scenario, as it becomes practically a test on permutations
* The code which implemnts the a tweak approach, as long as the concept of 'Simulated Annealing', as it can take slightly worse solutions, as long the coverage obtained by the new solution is higher than the previous one

Experiments and cocnlusions are reported as follow:


# Experiments and conclusions

Note: I running time of Approcheches 2 & 3 take more than 1 hour to complete when working with the last three configurations

|Configuration | Elements | Sets   | Density | Greedy Optimal Solution | Approach 1   | Approach 2   | Approach 3    |
|--------------|----------|--------|---------|-------------------------|--------------|--------------|---------------|
| 1            | 100      | 10     | 0.2     | 254.9091                | 281.8948     | 254.9091     | 254.9091      |
| 2            | 1000     | 100    | 0.2     | 5702.5028               | 8527.5181    | 14023.5398   | 10326.0617    |
| 3            | 10000    | 1000   | 0.2     | 101574.7907             | 158757.5319  | 1710263.6344 | 183182.3254   |
| 4            | 100000   | 10000  | 0.1     | 1526557.7851            | 2411102.0418 | 50234342.6956| 2415922.1224  |
| 5            | 100000   | 10000  | 0.2     | 1719097.0827            | 2585915.2155 | ?            | ?             |
| 6            | 100000   | 10000  | 0.3     | ?                       | ?            | ?            | ?             |

The performances of the three algorithms will be evaluated primarly on results obtained by the rrd configuration, which represent a good tradeoff between problem complexity and computaion time

The Hill-Climbing approach to an NP-H problem generally produce very scare results both in terms of computational time and minimization of the cost.
The same problem approached with a sub-optimal greedy algorithm produce a much better result in 1/10th of the time.
Confronting the three RHMC algorithms we can see how their results strongly depend on the initital state of hte system:
+ Approach 1 is theoreticallt the worst one beacouse it randomly adds sets till the set-cover is saturated: this means it probably add more sets than it should, resulting in gerally higher costs
+ Approach 2 effectiveness depends on the fisrst set which is removed from its list: if this set covers a large portion of the universe and it is associated with a low cost, the solution become very inefficient (see configuration 4 vs configuaration 3)
+ Approach 3 is the one which performs its decisions in the most ocultaed way, but its effectiveness strongly depends on the initial state, as we can see it produce results that are better worse, or quite on par w.r.t. the other two approaches

To better study the behavoiur of the three RHMC algorithms we take in consideration the fitness curve w.r.t. configuration 3
