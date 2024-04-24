# Girl-Hackathon
#Adaptive NOC: Optimizing Chip Communication for Performance and Efficiency
**# 1. Pseudocode for measuring average latency and bandwidth:**
# Function to parse monitor output and measure average latency and bandwidth
def measure_latency_bandwidth(monitor_output):
    total_latency = 0
    total_bytes_transferred = 0
    previous_txn_timestamp = None

    for entry in monitor_output:
        timestamp, txn_type, data = entry

        # Calculate latency for read transactions
        if txn_type.startswith("Rd"):
            if previous_txn_timestamp is not None:
                latency = timestamp - previous_txn_timestamp
                total_latency += latency
            previous_txn_timestamp = timestamp

        # Calculate bandwidth for write transactions
        if txn_type.startswith("Wr"):
            total_bytes_transferred += 32  # Assuming each write transaction transfers 32 bytes

    # Calculate average latency and bandwidth
    num_read_transactions = sum(1 for entry in monitor_output if entry[1].startswith("Rd"))
    num_write_transactions = sum(1 for entry in monitor_output if entry[1].startswith("Wr"))
    average_latency = total_latency / num_read_transactions if num_read_transactions > 0 else 0
    average_bandwidth = total_bytes_transferred / num_write_transactions if num_write_transactions > 0 else 0

    return average_latency, average_bandwidth

# Example usage:
monitor_output = [
    (0, "Rd Addr1", None),
    (2, "Wr Addr2", 'hxxxxxxxx'),
    (4, "Wr Addr3", 'hyyyyyyyy'),
    (10, "Data Addr1", 'hzzzzzzzz')
    # Additional monitor output entries here
]

avg_latency, avg_bandwidth = measure_latency_bandwidth(monitor_output)
print("Average Latency:", avg_latency)
print("Average Bandwidth:", avg_bandwidth)


--------------------------------------------------------------------------------------------------------
**#2. Design Document for Reinforcement Learning (RL) Approach:**

**RL Framework:**

**States/Behaviors:**
1. **NOC Configuration State**: This includes parameters such as buffer sizes, arbitration weights, and throttle settings. These parameters influence the flow of traffic within the NOC and affect latency, bandwidth, and buffer occupancy.
2. **Workload Information State**: Information about the types and intensities of workloads running on the CPU and IO peripherals. This includes metrics such as read and write patterns, data access frequencies, and transaction sizes.
3. **System State**: Current system power level, buffer occupancy levels, and arbitration rates. These factors provide contextual information about the overall system status and performance.

**Actions:**
1. **Adjust Buffer Sizes**: Increase or decrease buffer sizes to accommodate varying workload demands and optimize buffer occupancy.
2. **Modify Arbitration Weights**: Adjust weights to prioritize either CPU or IO traffic based on current system conditions and workload requirements.
3. **Throttle Control**: Initiate throttling if system power exceeds a predefined threshold, balancing performance with power constraints.

**Rewards:**
1. **Latency**: Minimize latency by rewarding configurations that result in lower average latency. This encourages the NOC to prioritize transactions efficiently and reduce data transfer delays.
2. **Bandwidth**: Maximize bandwidth by rewarding configurations that achieve higher average bandwidth. This incentivizes the NOC to optimize data transfer rates and throughput.
3. **Buffer Occupancy**: Maintain buffer occupancy close to 90% by rewarding configurations that effectively manage buffer usage. This ensures efficient resource utilization and minimizes buffer overflow or underflow.
4. **Throttling Frequency**: Penalize configurations that lead to frequent throttling, aiming to keep throttling frequency below 5%. This discourages excessive throttling and encourages the NOC to maintain stable performance without frequent interruptions.

**RL Algorithm Recommended(Idea):**
Given the complexity of the problem statement and the continuous and discrete components of the state space, an algorithm that can handle high-dimensional state spaces and discrete action spaces would be suitable. Therefore, an Actor-Critic method, such as Advantage Actor-Critic (A2C) or Proximal Policy Optimization (PPO), may be well-suited for this problem statement. These algorithms offer a good balance between sample efficiency, stability, and scalability, making them suitable for optimizing the NOC configuration in a dynamic environment.
--------------------------------------------------------------------------------------------------------

**Evaluation:**
Evaluate the trained RL agent's performance against the specified optimization criteria. Ensure that the NOC design derived from the RL agent's policy meets the requirements, including latency <= min_latency, bandwidth at 95% of max_bandwidth, buffer occupancy at 90%, and throttling frequency below 5%.

By following this RL framework and design document, developers can effectively use reinforcement learning to derive an optimal NOC design meeting the specified requirements. This process involves testing the trained RL agent's policy in simulation environments with various workload scenarios and system conditions. Performance metrics such as latency, bandwidth, buffer occupancy, and throttling frequency are measured and compared against the optimization criteria.

If the trained RL agent consistently produces NOC configurations that meet or exceed the specified requirements across different scenarios, it indicates the effectiveness of the RL approach in designing an optimal NOC configuration. Any deviations or shortcomings from the desired performance metrics can be used to refine the RL framework, adjust hyperparameters, or augment the training data to improve the agent's performance further.
