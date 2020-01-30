# Assignment 1, Part 2

PML&DL course
D.Romanov, BS4-DS1

# How to run?

Requirements: `pandas`, `numpy`, `matplotlib`, `geopy`, `shapely`, `celluloid`
Put it into Google Colab and push "Run all" button.

# Convergence

For `numpy` random seed = 2, `random` random seed = 2:
* Anneal rate = 0.999 converges within 44001 iterations with the result 33577 km
* Anneal rate = 0.995 converges within 8781 iterations with the result 36612 km  
* Anneal rate = 0.99 converges within 4381 iterations with the result 37572 km  
* Anneal rate = 0.95 converges within 856 iterations with the result 42425 km  

Little difference between 0.995 and 0.99 may indicate, that the model ends up at local maxima even with 0.99, other 4000 iterations are spent for fine-tuning and finding the local maxima inside a local maxima.
On the other hand, drastic change in the number of iterations between 0.99/0.995 and 0.999 may be too big to be paying off for a decrease in the resulting optimal path length.

# Performance issues

1. Due to some unknown reason my implementation refuses to go for actually optimal paths and stucks around 33k kilometers. I tried different ways of swapping cities, played with hyperparameters, changed the energy function and changed nothing. The only possible error I could think of is problems with shallow copying the path list inside `random_swap` function, but I only change order of the same objects in a new list, with no changes to objects themself, so even shallow copy should be working.
2. Because of animation rendering my solution takes a lot of time to be computed. It may take around 20 minutes for 2000 iterations to be rendered. I tried to copy common information like cities and country outline, but `matplotlib` has some troubles with copying axis, and refused to properly work at all.
3. Even after I switched of the `matplotlib` interactive mode with `plt.ioff()`, it still wastes time to render uncomprehensible and useless artifacts:
    ![](https://i.imgur.com/6AsGzCo.png)

# Discussion

From one point of view, correctly implemented and working algorithm may produce a solution to such discrete and "non-differentiatable" problem with arbitrary precision due to relatively big amount of hyperparameters.
From the other point of view, it may be more practical to use minimum spanning tree algorithms for that - they provide The Optimal Solution with O(E\*logE) time complexity (Kruskal's algorithm). For sure, they will struggle on bigger graphs, but Simulated Annealing will struggle as well.
On the other hand, the algorithm showed very nice results given how little prior information it "knew" about the problem. 
As a future work, I would probably research other ways of expressing energy levels and more "sentient" ways to pick vertices for swapping.