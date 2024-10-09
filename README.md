# CI2024_lab1
Laboratory 1 for Computational Intelligence Course

# Overview
## Presentation
In this laboratory, the hill-climber approach for building a solution for the 'Set Cover' problem is presented.  
In addition to the original `.ipynb` file provided, I added a utility function called `covered_elements`,  
which returns the total coverage of the current solution.

## Code Implementation
The solution first implements a greedy algorithm and a brute-force one to compare with the solutions found by the different approaches of the hill-climbing algorithms implemented.  
Since the brute-force algorithm is infeasible for all configurations except the first one, I recommend using the greedy algorithm if you want to repeat the experiments.

The file contains different approaches to the problem using two hill-climbing structures:

* **Bottom-up solution-building**: Starts with no sets taken and adds a random one until it achieves complete coverage.
* **Top-down solution approach**: Starts with the trivial solution where all sets are taken and removes a random one while ensuring the solution remains valid. The process is repeated until we achieve a target 'threshold' (the ratio between the considered set-cover and the total number of sets) or the process can't find another item to remove within a 'time_limit' of iterations.

These two methods implement a basic approach without considering costs. The goal is to study the advantages and disadvantages of the two strategies for this problem.  
A final approach takes into consideration the 'cost' variable and makes more informed decisions while building the solution (an exploitation-driven method).

## Considerations
While little can be said about the first two algorithms, some considerations need to be made regarding the last one:

First, this implementation explores the balance between **exploration** and **exploitation**:

* The code handles multiple candidate indices before making a choice based on cost. This implements the idea of 'wideness' in exploration: the higher the cardinality of the array containing the candidate indices, the further the algorithm can see beyond its local landscape, potentially finding the optimal solution. However, it's clear that in a context where jumps are made randomly, the more the algorithm jumps between states, the more unstable it becomes. Eventually, search time could match that of brute-force in the worst-case scenario, as it essentially tests permutations.
* The code implements a tweak approach, similar to **Simulated Annealing**, where it accepts slightly worse solutions as long as the coverage obtained by the new solution is higher than the previous one.

Experiments and conclusions are reported below:

# Experiments and Conclusions

## Experiment Results
Note: The running time of Approaches 2 & 3 takes more than 1:50 hours to complete for the last two configurations.

| Configuration | Elements | Sets   | Density | Greedy Optimal Solution | Approach 1   | Approach 2   | Approach 3    |
|---------------|----------|--------|---------|-------------------------|--------------|--------------|---------------|
| 1             | 100      | 10     | 0.2     | 254.9091                | 281.8948     | 254.9091     | 254.9091      |
| 2             | 1000     | 100    | 0.2     | 5702.5028               | 8527.5181    | 14023.5398   | 10326.0617    |
| 3             | 10000    | 1000   | 0.2     | 101574.7907             | 158757.5319  | 1710263.6344 | 183182.3254   |
| 4             | 100000   | 10000  | 0.1     | 1526557.7851            | 2411102.0418 | 50234342.6956| 2415922.1224  |
| 5             | 100000   | 10000  | 0.2     | 1719097.0827            | 2585915.2155 | 4484641.2155 | 2585180.2155  |
| 6             | 100000   | 10000  | 0.3     | 1756195.7223            | 2522800.3354 | /            | 413672179.8582  |

## Conclusions
The performance of the three algorithms was primarily evaluated based on results obtained from the third configuration, which represents a good tradeoff between problem complexity and computation time.

The hill-climbing approach to an NP-hard problem generally produces poor results in both computational time and cost minimization. The same problem approached with a sub-optimal greedy algorithm produced much better results in 1/10th of the time.

Moreover, as the problem grows in size, the performance of hill-climbing increasingly depends on selecting a good starting point. A random selection almost always leads to suboptimal results, which are often poor.

### Comparing the Three Hill-Climbing Algorithms:

- **Approach 1** is theoretically the worst because it randomly adds sets until the set-cover is saturated. This means it likely adds more sets than necessary, resulting in higher costs. However, it has the lowest computational load, making it the preferred approach for high-dimensionality configurations.
  
- **Approach 2**’s effectiveness depends on the first set removed from the solution. If this set covers a large portion of the universe at a low cost, the solution becomes very inefficient (e.g., configuration 4 vs. configuration 3). Additionally, the program becomes infeasible for high-dimensionality problems because identifying which sets to exclude is hard with random selection.

- **Approach 3** makes its decisions in the most methodical way, but its effectiveness strongly depends on the initial state. As we see, its results can be better, worse, or on par with the other approaches. Like the other two approaches, it becomes infeasible for high-dimensionality problems due to its Big-O complexity.

To better study the behavior of the three hill-climbing algorithms, we consider the fitness curve for configuration 3. Looking at the results shown in the notebook:

- Approach 1 reaches saturation in the final steps.
- Approach 2 does not saturate with the given constraints, suggesting that:
  - A more efficient constraint on the stopping condition exists.
  - We could implement self-evolution strategies for the learning rate (though this is not the focus of this lab).
- Approach 3 reaches a good cost value in the early steps, but struggles to find a valid solution for high-dimensionality problems. This may be because once a good cost is reached, it becomes difficult to find another configuration that improves the solution. This problem could be addressed by introducing evolution strategies.

### Final Remarks:
The first approach yielded the best results across the board, outperforming even the approach theoretically expected to produce the best results (Approach 3). This was despite the lack of cost consideration, annealing strategies, and multiple mutation strategies in Approach 1. This may be due to the nature of the problem and the fact that introducing such strategies without more sophisticated parameter management may only worsen results. This leads to preferring a simpler approach over a more complex one when dealing with random mutations for an NP-hard problem.
