---
layout: post
title: "Network Routing Decisions with Network Control Plane Simulator"
subtitle: "Developed for SIT202 - A short guide on how Bellman-Ford and Dijkstra's algorithms work for network path finding."
background: '/img/banner.png'
tags: tutorials
---

Understanding routing decisions in computer networking is crucial. The control plane, which directs data in networks, relies on routing algorithms, such as Dijkstra's shortest path and the Bellman-Ford algorithm. In this blog post, we'll explore a Network Control Plane Simulator that helps users gain insights into network routing.

![python](/img/tutorials/network-routing-decisions/img1.png)

You can view the program code here:

![Readme Card](https://github-readme-stats.vercel.app/api/pin/?username=breezy-codes&repo=Control-Plane-Simulator&show_owner=true&)

### **Code Structure**

#### **Common Components**

**Network Topology (routers dictionary):** This dictionary describes our virtual network layout, where routers connect to neighbouring routers with numerical weights.
#### **Dijkstra's Algorithm**
**Dijkstra's Algorithm Function (dijkstra function):** Dijkstra's algorithm is based on the concept of finding the shortest path in a weighted graph. It uses a priority queue to explore the network efficiently. The algorithm works by calculating the shortest distance from a starting router to all other routers in the network.
**Path Determination (get_path function):** This function determines the optimal path from a source to a destination router using Dijkstra's results. It backtracks using predecessors to find the shortest path.

![python](/img/tutorials/network-routing-decisions/img2.png)

#### **Bellman-Ford Algorithm**
**Bellman-Ford Algorithm Function (bellman_ford function):** The Bellman-Ford algorithm is another method for finding the shortest path in a weighted graph. Unlike Dijkstra's algorithm, it can handle graphs with negative edge weights and identify negative weight cycles. The algorithm iteratively relaxes edges to determine the shortest paths.
**Path Determination using Bellman-Ford (get_path_bf function):** This function traces back the optimal path from source to destination using Bellman-Ford's results.

![python](/img/tutorials/network-routing-decisions/img3.png)

#### **Main Execution**
Sample test cases at the end of each script demonstrate the algorithms in action, including finding paths between specified router pairs.
### **Mathematics Behind the Algorithms**
**Dijkstra's Algorithm:** Dijkstra's algorithm uses concepts of graph theory and dynamic programming. It maintains a set of visited nodes and computes the shortest distance to each node by iteratively selecting the node with the smallest tentative distance. The algorithm relies on the principle that a shorter path to a node is found by relaxing edges in a particular order.
**Bellman-Ford Algorithm:** The Bellman-Ford algorithm also works with weighted graphs, using the concept of relaxation to iteratively update estimates of the shortest distance to each node. It is designed to handle graphs with negative edge weights and detect negative weight cycles by repeating the relaxation step.
### **Getting Started**
To run the simulator:
1. Ensure you have Python installed on your system.
2. Clone this repository or download the scripts.
3. Navigate to the directory and run the appropriate script:
    - For Dijkstra's algorithm:    
    `python dijkstra_simulator.py`
4. For Bellman-Ford's algorithm:
    `python bellmanford_simulator.py`   
Executing the scripts will display optimal paths for sample test cases and their associated costs.
### **Improvements and Extensions**
1. **Dynamic Topology:** Implement a user interface or command-line prompt for users to input their own topology dynamically.
2. **Visualisation:** Design a graphical interface to visually depict routers and connections, enhancing understanding.
3. **Multiple Algorithm Comparison:** Allow users to select from several algorithms and compare results.
4. **Network Failures Simulation:** Simulate network failures, such as routers or connections failing, to test routing algorithm resilience.
5. **Performance Metrics:** Measure routing decision execution times to examine the impact of factors like network size and complexity.
6. **Support for Additional Costs:** Extend support for metrics like bandwidth, congestion, or reliability determining optimal paths.
### **Conclusion**
The Network Control Plane Simulator is a valuable tool for understanding routing decisions in computer networks. As our digital landscape evolves, tools like this are invaluable for students, professionals, and enthusiasts. Proposed improvements would enhance this simulator for advanced network simulations and algorithm comparisons. Understanding network routing is crucial in our data-driven world.