# Adaptive Elephant Herding Optimization & Enhanced AES for Dynamic Task Scheduling and Security over Cloud Computing

> Implementation of the AEHO-EAES algorithm for energy-efficient dynamic task scheduling and hypervisor attack detection in cloud environments.

---

## Table of Contents

- [Overview](#overview)
- [Research Paper](#research-paper)
- [System Architecture](#system-architecture)
- [Algorithms](#algorithms)
  - [AEHO — Dynamic Task Scheduling](#aeho--dynamic-task-scheduling)
  - [EAES — Security & Hypervisor Attack Detection](#eaes--security--hypervisor-attack-detection)
- [Dataset](#dataset)
- [Implementation](#implementation)
- [Results](#results)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [How to Run](#how-to-run)
- [References](#references)

---

## Overview

Cloud computing faces two major challenges: **energy-efficient task scheduling** and **security against hypervisor attacks**. This project implements and evaluates the **AEHO-EAES** framework, which combines:

- **Adaptive Elephant Herding Optimization (AEHO)** — for optimal Virtual Machine (VM) selection and dynamic task scheduling with minimum energy consumption.
- **Enhanced Advanced Encryption Standard (EAES)** — for cloud data security and real-time hypervisor/VM attack detection using a double-key encryption mechanism.

The proposed system outperforms existing methods (EDS and DVMC) across all key metrics: time complexity, cost complexity, throughput, energy consumption, and reliability.

---

## Research Paper

**Title:** Adaptive Elephant Herding Optimization and Enhanced Advanced Encryption Standard Algorithm for Dynamic Task Scheduling and Security over Cloud Computing

**Key Contributions:**
1. Construction of a cloud system model incorporating cloud users, tasks, VMs, and resources.
2. A virtualization layer enabling seamless VM migration across physical hosts.
3. The **AEHO** algorithm for energy-aware dynamic task scheduling via Clan Updating Operator (CUO) and Separating Operator (SO).
4. The **EAES** algorithm for detecting and mitigating hypervisor and VM attacks using round-based AES enhancements.

---

## System Architecture

```
Cloud Users
    │
    ▼
Request Services ──► Virtualization Layer (VM Migration)
    │
    ▼
┌─────────────────────────────────────────┐
│          AEHO Algorithm                 │
│  - Fitness evaluation                   │
│  - Clan Updating Operator (CUO)         │
│  - Separating Operator (SO)             │
│  - Optimal VM selection (low energy)    │
└─────────────────────────────────────────┘
    │
    ▼
Dynamic Task Scheduling → VM Placement
    │
    ▼
┌─────────────────────────────────────────┐
│          EAES Algorithm                 │
│  - Double-key AES encryption            │
│  - Round-based shift operations         │
│  - Hypervisor attack detection          │
│  - VM intrusion monitoring              │
└─────────────────────────────────────────┘
    │
    ▼
Response Services ──► Performance Evaluation
```

---

## Algorithms

### AEHO — Dynamic Task Scheduling

Elephant Herding Optimization (EHO) is a swarm intelligence algorithm inspired by the social behavior of elephant herds. The **Adaptive** variant (AEHO) improves convergence speed and exploitation capability through three key operators:

#### Clan Updating Operator (CUO)

Updates each elephant's position relative to its clan's matriarch and center:

```
h_i,j^(t+1) = h_i,j^t + CP1 × (m_i^t - h_i,j^t) + CP2 × (c_i^t - h_i,j^t) + CP3 × l
```

where `l = 2 × rand - 1 × (h_max - h_min)`, and CP1, CP2, CP3 are control parameters balancing convergence toward the matriarch, clan center, and a random walk.

The matriarch is updated as:
```
m^(t+1) = m^(t+1) + CP2 × (c^t - m^(t+1))
```

#### Separating Operator (SO)

Simulates male elephants leaving the herd, replacing the worst-performing individual with a new random solution:

```
h_i,worst^t = h_min + (h_max - h_min) × rand
```

#### Elitist Strategy

Preserves the best-performing elephant (VM) across generations, preventing degradation of the optimal solution.

**AEHO workflow:**
1. Initialize population of NP elephant-VMs with resource constraints.
2. Sort elephants by fitness value (energy consumption, task requirements).
3. Apply CUO to update clan positions.
4. Apply SO to replace worst members.
5. Evaluate updated population via dynamic task scheduling.
6. Output optimal VM selection with minimum energy consumption.

---

### EAES — Security & Hypervisor Attack Detection

The Enhanced AES builds on standard AES (128-bit block, variable key: 128/192/256-bit) with a **round-based dynamic shift operation** and **double-key encryption** for stronger cloud security.

#### Key Generation

Input data and encryption key are arranged as 4×4 byte matrices:

```
Key = [k0  k4  k8  k12]
      [k1  k5  k9  k13]
      [k2  k6  k10 k14]
      [k3  k7  k11 k15]
```

#### Enhanced Round-Based Shift Operation

Unlike standard AES which always shifts rows by fixed positions, EAES dynamically varies the shift based on the current round number:

- **Odd round (e.g., round 1):** Shift rows by 1 position.
- **Even round (e.g., round 2):** Shift rows by 2 positions.

This variation strengthens the diffusion property across rounds.

#### Encryption Steps per Round

1. **Sub-bytes** — Non-linear byte substitution using the reversible S-Box.
2. **Round-based Shift Rows** — Dynamic row shifting based on round parity.
3. **Mix Columns** — Column-wise GF(2⁸) matrix multiplication.
4. **Add Round Key** — Bitwise XOR with the round key derived from key schedule.

#### Hypervisor Attack Detection

The EAES layer monitors all data exchanges between VMs and the hypervisor in real time:

- Continuously tracks cloud request patterns from hypervisors.
- Detects anomalies or unauthorized exchanges between VMs.
- Identifies both external (hypervisor hijacking) and internal (rogue VM) attacks.
- Automatically blocks and mitigates detected threats.

---

## Dataset

The dataset used in this project is **synthetically generated** to simulate a realistic cloud data center environment.

### Dataset Parameters

| Parameter | Description |
|---|---|
| Cloud Users | Number of users sending task requests |
| Tasks (T) | Each task defined by CPU, memory, bandwidth, storage requirements |
| Virtual Machines (VM) | Characterized by processing power, memory, network, storage |
| Physical Hosts (H) | Defined by CPU capacity, memory volume, voltage-frequency pairs |
| Energy Model | Static + dynamic power; idle host uses ~70% of peak power |

### Task Attributes

Each task `T_k` is represented as a tuple:
```
T_k = (T_k^p, T_k^m, T_k^n, T_k^s)
```
- `T_k^p` — Processor power requirement
- `T_k^m` — Main memory requirement
- `T_k^n` — Network bandwidth requirement
- `T_k^s` — Storage requirement

### VM Attributes

Each VM `V_j` is represented as:
```
V_j = (V_j^m, V_j^p, V_j^n, V_j^s)
```

### Synthetic Generation

The dataset was generated programmatically to cover diverse workload scenarios including compute-heavy tasks, memory-intensive tasks, and mixed workloads, ensuring comprehensive evaluation of the scheduling and security algorithms.

---

## Implementation

The implementation was carried out using **CloudSim** for cloud data center simulation, with the AEHO and EAES algorithms implemented from scratch.

### Simulation Environment

- **Simulator:** CloudSim
- **Purpose:** Mimics a cloud data center with configurable hosts, VMs, and task workloads.
- **Evaluation:** QoS metrics, energy efficiency, and security performance.

### Implementation Steps

1. **System Model Construction** — Define hosts, VMs, tasks, and cloud users.
2. **Virtualization Setup** — Configure hypervisor and VM migration policies.
3. **AEHO Scheduling** — Run the adaptive optimization loop to assign tasks to optimal VMs.
4. **EAES Security Layer** — Apply encryption and activate hypervisor monitoring.
5. **Performance Evaluation** — Measure all metrics and compare against baselines.

### Baseline Comparisons

| Method | Description |
|---|---|
| **EDS** | Energy-efficient Dynamic Scheduling — classification-based scheduling for virtual cloud data centers |
| **DVMC** | Dynamic VM Consolidation — security-aware VM consolidation approach |
| **AEHO-EAES** | Proposed method (this work) |

---

## Results

The proposed AEHO-EAES method was evaluated against EDS and DVMC across five metrics:

| Method | Time Complexity (sec) | Cost Complexity ($/GB) | Throughput (Mbps) | Energy Consumption (J) | Reliability (%) |
|---|---|---|---|---|---|
| EDS | 56 | 0.130 | 0.79 | 36 | 81.7 |
| DVMC | 42 | 0.097 | 0.85 | 25 | 86.3 |
| **AEHO-EAES** | **28** | **0.058** | **0.93** | **18** | **92.5** |

### Key Findings

- **50% reduction in time complexity** compared to EDS.
- **55% reduction in cost complexity** compared to EDS.
- **17.7% higher throughput** compared to EDS.
- **50% reduction in energy consumption** compared to EDS.
- **10.8 percentage point improvement in reliability** compared to EDS.

---

## Project Structure

```
adaptive-elephant-herding-optimization-enhanced-aes-cloud/
│
├── data/
│   ├── synthetic_dataset.csv        # Generated synthetic cloud workload dataset
│   └── dataset_generator.py         # Script to regenerate the synthetic dataset
│
├── aeho/
│   ├── aeho.py                      # Core AEHO algorithm
│   ├── clan_updating_operator.py    # CUO implementation
│   ├── separating_operator.py       # SO implementation
│   └── fitness.py                   # Fitness evaluation (energy + task constraints)
│
├── eaes/
│   ├── eaes.py                      # Enhanced AES encryption
│   ├── key_generation.py            # Double-key generation
│   ├── shift_operations.py          # Round-based dynamic shift rows
│   └── hypervisor_detection.py      # Hypervisor attack monitoring
│
├── simulation/
│   ├── cloud_model.py               # System model (hosts, VMs, tasks, users)
│   ├── virtualization.py            # VM migration and hypervisor setup
│   └── scheduler.py                 # Dynamic task scheduler (AEHO integration)
│
├── evaluation/
│   ├── metrics.py                   # Time, cost, throughput, energy, reliability
│   └── compare_baselines.py         # Comparison with EDS and DVMC
│
├── results/
│   └── figures/                     # Performance comparison plots
│
├── requirements.txt
└── README.md
```

---

## Requirements

```
python >= 3.8
numpy
pandas
matplotlib
scikit-learn
pycryptodome        # AES implementation base
cloudsim (or py-cloudsim)
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

## How to Run

### 1. Generate Synthetic Dataset

```bash
python data/dataset_generator.py
```

### 2. Run AEHO Task Scheduling

```bash
python simulation/scheduler.py --tasks 100 --vms 20 --hosts 10
```

### 3. Run EAES Security Layer

```bash
python eaes/eaes.py --keysize 128 --input data/synthetic_dataset.csv
```

### 4. Full Simulation with Evaluation

```bash
python simulation/cloud_model.py --mode full
```

### 5. Compare Against Baselines

```bash
python evaluation/compare_baselines.py
```

Results and plots will be saved in the `results/` directory.

---

## References

1. Desai, M. R., & Patel, H. B. (2015). Efficient virtual machine migration in cloud computing. *ICCWAMTIP*. IEEE.
2. Hanini, M., El Kafhali, S., & Salah, K. (2019). Dynamic VM allocation and traffic control to manage QoS and energy. *IJCAT* 60(4).
3. Di Pietro, R., & Lombardi, F. (2018). Virtualization Technologies and Cloud Security. *Springer*.
4. Lee, Y. C., & Zomaya, A. Y. (2012). Energy efficient utilization of resources in cloud computing. *Journal of Supercomputing* 60(2).
5. Gan, C., et al. (2020). Dynamical propagation model of malware for cloud computing security. *IEEE Access* 8.
6. Li, X.-K., et al. (2015). Virtual machine placement strategy based on discrete firefly algorithm. *ICCWAMTIP*. IEEE.
7. Zhixue, W. U. (2017). Advances on virtualization technology of cloud computing. *Journal of Computer Applications* 37(4).
8. Elhosseini, M. A., et al. (2019). On the performance improvement of elephant herding optimization. *Knowledge-Based Systems* 166.
9. Marahatta, A., et al. (2019). Classification-based and energy-efficient dynamic task scheduling. *IEEE Transactions on Cloud Computing* 9(4).
10. Zekri, M., et al. (2017). DDoS attack detection using machine learning in cloud environments. *CloudTech*. IEEE.
11. Rawashdeh, A., et al. (2018). An anomaly-based approach for DDoS attack detection in cloud. *IJCAT* 57(4).
12. Annadanam, C. S., et al. (2020). Intermediate node selection for Scatter-Gather VM migration. *ESTIJ* 23(5).
13. Beloglazov, A., et al. (2012). Energy-aware resource allocation heuristics for cloud data centers. *Future Generation Computer Systems* 28(5).
14. Hakli, H. (2020). BinEHO: A new binary variant of elephant herding optimization. *Neural Computing and Applications* 32(22).
15. Strumberger, I., et al. (2017). Hybridized elephant herding optimization for constrained optimization. *HIS*. Springer.
16. Pendli, V., et al. (2016). Improvising performance of Advanced Encryption Standard algorithm. *MobiSecServ*. IEEE.
17. Szefer, J., et al. (2011). Eliminating the hypervisor attack surface. *ACM CCS*.
18. Dildar, M. S., et al. (2017). Effective way to defend hypervisor attacks in cloud computing. *ICACC*. IEEE.
19. Elshabka, M. A., et al. (2020). Security-aware dynamic VM consolidation. *Egyptian Informatics Journal*.

---

## License

This project is developed for academic research purposes.
