# Girl-Hackathon
#Adaptive NOC: Optimizing Chip Communication for Performance and Efficiency
#1. Pseudocode to Measure Average Latency and Bandwidth:
function parse_monitor_output(monitor_output):
    latency_sum = 0
    bandwidth_sum = 0
    throttling_count = 0
    total_cycles = 0
    
    for entry in monitor_output:
        latency_sum += entry.latency
        bandwidth_sum += entry.bandwidth
        total_cycles += 1
        if entry.throttling:
            throttling_count += 1
    
    average_latency = latency_sum / total_cycles
    average_bandwidth = bandwidth_sum / total_cycles
    throttling_percentage = (throttling_count / total_cycles) * 100
    
    return average_latency, average_bandwidth, throttling_percentage

monitor_output = simulator.run()
avg_latency, avg_bandwidth, throttling_percentage = parse_monitor_output(monitor_output)


--------------------------------------------------------------------------------------------------------
2. Design Document for Reinforcement Learning (RL) Approach:
2. Design Document for Reinforcement Learning (RL) Approach:
RL Framework:
States: States represent NOC configurations and workload information. They include parameters such as buffer sizes, arbitration weights, current workload patterns, etc.
Actions: Actions correspond to adjustments to NOC parameters, such as modifying buffer sizes, changing arbitration weights, etc.
Rewards: The reward function is designed to balance latency, bandwidth, buffer occupancy, and throttling constraints. It encourages the RL agent to minimize latency, maximize bandwidth, maintain buffer occupancy at 90%, and keep throttling frequency below 5%.
RL Algorithm:
The problem statement involves continuous state and action spaces, making it suitable for model-free RL algorithms. Algorithms like Deep Q-Network (DQN) or Actor-Critic methods would be appropriate choices. These algorithms can handle complex, high-dimensional state spaces and continuous action spaces efficiently.

Training Procedure:
Initialization: Initialize the RL agent and the NOC environment with random parameters.
Training: Interact with the environment by selecting actions based on the current state and the RL policy. Update the agent's policy using RL algorithms such as DQN or Actor-Critic based on the observed rewards.
Evaluation: Evaluate the trained RL agent's performance on a separate validation set of workload scenarios. Ensure that the agent meets the specified optimization criteria in terms of latency, bandwidth, buffer occupancy, and throttling frequency.

---------------------------------------------------------------------------------------------------------

Evaluation:
Evaluate the trained RL agent's performance against the specified optimization criteria. Ensure that the NOC design derived from the RL agent's policy meets the requirements, including latency <= min_latency, bandwidth at 95% of max_bandwidth, buffer occupancy at 90%, and throttling frequency below 5%.

By following this RL framework and design document, developers can effectively use reinforcement learning to derive an optimal NOC design meeting the specified requirements.
