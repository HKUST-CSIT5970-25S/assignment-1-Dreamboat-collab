[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Jiang, Juntao
### Student Id: 21076129
### Email: jjiangbl@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > The measurement tool I used for assessing CPU and memory performance is **Phoronix Test Suite**. I selected this tool because it is a widely-used open-source benchmark suite that can measure various aspects of system performance, including CPU and memory performance. For the configuration, I ran the **7-Zip Compression** test (identified as `pts/compress-7zip`) to evaluate CPU performance during data compression tasks. I set the number of threads to match the number of virtual CPUs available (4 threads) to fully utilize the multi-core capabilities of the processor. I also set the test duration to 10 minutes to allow sufficient time for the test to reach a stable performance reading. The measurement results from this test represent the **execution time** required for compressing data, providing an indicator of how efficiently the CPU handles compression tasks. Additionally, I ran memory tests using **RAMspeed** to evaluate memory bandwidth and latency, where the results represent the **throughput** and **speed** of the memory operations during the test. These results help in understanding how well the system's memory subsystem performs under load.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro` | 3768 MIPS | 10749.88 MB/s |
    | `t2.medium`  | 8766 MIPS | 18617.56 MB/s |
    | `c5d.large` | 7394 MIPS | 14751.28 MB/s |

    > Memory performance: As memory capacity increases, memory performance improves. t2.medium has the highest memory performance (18617.56 MB/s), and its memory resources are 4GB. Although both t2.micro and c5d.large have 4GB of memory, c5d.large has better memory performance than t2.micro (14751.28 MB/s vs. 10749.88 MB/s).
    >
    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.

## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` | 4200           | 0.25     |
    | `m5.large` - `m5.large    | 4950           | 0.148    |
    | `c5n.large` - `c5n.large` | 1840           | 0.733    |
    | `t3.medium` - `c5n.large` | 3820           | 0.2      |
    | `m5.large` - `c5n.large`  | 3850           | 0.189    |
    | `m5.large` - `t3.medium`  | 3720           | 0.245    |

    > In the same region, TCP bandwidth varies greatly between instance types. The bandwidth provided by m5.large instances (4950 Mbps) is higher than that provided by t3.medium (4200 Mbps), but the bandwidth provided by c5n.large (1840 Mbps) is relatively low.
    > In terms of RTT, m5.large and t3.medium have lower latency, while c5n.large shows higher latency.
    >
    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      | 22.5           | 57.014   |
    | N. Virginia - N. Virginia | 1840           | 0.733    |
    | Oregon - Oregon           | 4930           | 0.157    |

    > N. Virginia - Oregon has very low TCP bandwidth (22.5 Mbps) and high latency (57.014 ms), indicating poor performance for cross-region communication.
    > Within the same region, N. Virginia - N. Virginia and Oregon - Oregon have higher TCP bandwidths of 1840 Mbps and 4930 Mbps, respectively, and latencies of 0.733 ms and 0.157 ms, respectively, indicating better network performance within the same region.
    >
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.
