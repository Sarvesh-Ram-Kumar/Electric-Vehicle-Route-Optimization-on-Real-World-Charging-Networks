# EV Charging-Aware Route Optimization Using Reinforcement Learning
Implementation accompanying the research manuscript submitted to *Scientific Reports*.

This repository contains the implementation used in the research paper on **electric vehicle (EV) route planning using reinforcement learning with energy-aware constraints**. The project evaluates classical graph-based algorithms and reinforcement learning approaches for long-distance EV route planning with charging station considerations.

The experiments compare baseline routing methods with several reinforcement learning approaches, including a **Primal–Dual Q-learning framework with optimization improvements**.

---

# Repository Structure

The repository contains the following main code files used for the route between Orlando and Atlanta. Random seeds can be set in the notebooks to ensure reproducibility of the reinforcement learning experiments.

### 1. EV_Q_Learning_ORS.ipynb
Implements **standard Q-learning** for EV route planning.

Features:
- Tabular Q-learning agent
- Reward based on travel distance and route progress
- Graph built from clustered EV charging stations
- Evaluation using OpenRouteService (ORS) road distances

This serves as the **baseline reinforcement learning model**.

---

### 2. EV_Double_Q_Learning_ORS.ipynb
Implements **Double Q-learning** to reduce overestimation bias in Q-value updates.

Features:
- Two Q-tables for stable value estimation
- Reduced bias compared to standard Q-learning
- Same environment and reward structure as the baseline model

This method is evaluated to determine whether Double Q-learning improves routing stability.

---

### 3. EV_Modified_A_ORS.ipynb
Implements a **modified A* algorithm** adapted for EV route planning.

Features:
- Charging-station-aware heuristic
- Distance and charger availability considerations
- Evaluation using real road distances through ORS

This represents a **classical heuristic routing baseline**.

---

### 4. EV_Modified_Dijkstra_ORS.ipynb
Implements a **modified Dijkstra algorithm** for EV route planning.

Features:
- Graph-based shortest path computation
- Incorporates charger availability in edge weighting
- Used as a deterministic baseline for comparison

---

### 5. EV_RL_Primal_Dual_Q_Lagrangian.ipynb
Implements the **Primal–Dual Q-learning model** using a Lagrangian formulation.

Features:
- Dual Q-learning structure
- Separate reward and cost Q-tables
- Lagrangian multiplier controlling energy feasibility
- Adaptive balancing between distance and charging constraints

This file contains the **core proposed RL formulation** used in the study.

---

### 6. EV_RL_Optimization_Primal_Dual_Q.ipynb
Implements the **optimized version of the Primal–Dual Q-learning model**.

Features:
- More stable convergence behaviour
- Final evaluation and visualization of routing results

This file produces the **final results reported in the paper**.

---

## Additional Experiment Modules

### Success Rate Code

The `Success rate code` folder contains scripts used to evaluate the **success rate of different routing algorithms**.

These scripts run multiple routing trials and compute the percentage of runs in which the agent successfully reaches the destination while maintaining feasible charging constraints.

The experiments compare:

- Q-learning  
- Double Q-learning  
- Primal–Dual Q-learning  
- Optimized Primal–Dual Q-learning  

The results from these experiments were used to produce the **success rate comparison results reported in the paper**.

---

### Ablation Study Code

The `Ablation Study Code` folder contains scripts used to analyze the contribution of different components of the proposed model.

The ablation experiments evaluate how performance changes when specific elements of the framework are modified or removed, such as:

- removing the dual update mechanism  
- removing constraint-aware cost signals  
- using reward-only learning  
- comparing different RL formulations  

These experiments help demonstrate that the **dual update mechanism plays a key role in improving routing feasibility and learning stability**.

---

### DBSCAN Analysis Code

The `DBSCAN Analysis Code` folder contains scripts used to analyze the clustering process used to construct the routing graph.

In the proposed framework, charging stations are clustered using the **DBSCAN algorithm** to reduce graph complexity and group nearby stations into representative nodes.

The analysis scripts evaluate:

- clustering behavior  
- spatial distribution of clusters  
- the effect of clustering parameters on routing performance  

This analysis helps justify the clustering configuration used during graph construction.

---

## Graph Data File

### `graph_data.gpickle`

The repository includes a precomputed graph file.



# Key Methodology

The routing environment is modeled as a **graph of EV charging station clusters**, where:

- Nodes represent clustered charging stations.
- Edges represent feasible travel connections between nearby stations.
- Edge weights incorporate distance and charging availability.

The reinforcement learning agent learns routing policies that balance:

- travel distance,
- charging availability,
- energy feasibility constraints.

The proposed approach uses a **Primal–Dual reinforcement learning framework** to incorporate constraint-aware learning through adaptive dual variables.

---

# Dataset

The project uses EV charging station data obtained from public datasets.

See:

`data_instructions.txt`

for instructions on how to download and prepare the dataset.

---

## Method Overview

The system models the EV routing problem as a graph-based Markov Decision Process (MDP).

Key components include:

1. **Graph Construction**
   - EV charging stations are clustered using DBSCAN.
   - A weighted graph is created using geodesic distances.

2. **Environment**
   - States: Charging station nodes
   - Actions: Travel to neighboring nodes
   - Rewards: Distance minimization and charging feasibility
   - Costs: Energy consumption constraints

3. **Learning Framework**
   - Standard Q-learning baseline
   - Dual-value learning for reward and cost optimization
   - Adaptive dual parameter (μ) update

4. **Evaluation**
   - Route success rate
   - Path efficiency relative to geodesic distance
   - Comparison across RL methods

---

## Running the Code

### Step 1: Install dependencies

pip install -r requirements.txt

### Step 2: Run the notebooks

The notebook performs the following steps:

1. Load and preprocess EV charging station data  
2. Construct the routing graph  
3. Train reinforcement learning agents  
4. Generate routing policies  
5. Evaluate route efficiency and charging feasibility  
6. Visualize routes and training metrics  

---

## Reference On-Road Distance

To evaluate routing efficiency, a reference real-world road distance is used.

The approximate driving distance between the selected origin and destination (Orlando → Atlanta) is approximately:

**710 km**

This value is obtained from publicly available online mapping services and represents a typical real-world driving route.

It is **not generated by the reinforcement learning models**.

Instead, this reference distance is used to evaluate the efficiency of the routes produced by the routing algorithms.

Efficiency is computed as:

Efficiency = Reference On-Road Distance / Model Route Distance

This allows comparison of how closely the learned routing strategies approximate realistic travel routes.

All evaluated algorithms use the same reference value for consistency.

---

## Optional Road Network Validation

Some experiments optionally compute real-world driving distances using the OpenRouteService API.

If this feature is used, insert a valid API key where indicated in the notebook.

---

## Notes

- The implementation is intended for research and reproducibility purposes.
- Experiments were conducted using a graph constructed from real-world EV charging station data.

---

## Code Availability

The full implementation used in this study is publicly available in this repository.

All code required to reproduce the experiments, including reinforcement learning models, classical routing baselines, clustering analysis, and evaluation scripts, is provided.

The repository includes:

- Reinforcement learning implementations (Q-learning, Double Q-learning, Primal–Dual Q-learning)
- Classical routing baselines (Modified A* and Modified Dijkstra)
- Optimization experiments for the proposed Primal–Dual RL framework
- Success rate evaluation scripts
- Ablation study experiments
- DBSCAN clustering analysis code
- Precomputed graph data used for experiments

This repository is intended to support **reproducibility and transparency of the results reported in the associated research manuscript**.

---

## License

This code is released for academic and research purposes. Users are free to use and adapt the implementation with appropriate citation of the associated research work.