
# Experiment 1 – Parallel Computing

## Objective

To implement and study matrix multiplication using Sequential, OpenMP, and MPI approaches and compare their execution performance.

## Introduction

Matrix multiplication is a computationally intensive operation that can be implemented using different parallel computing models. In this experiment, the same matrix multiplication problem is implemented using a sequential CPU approach, shared-memory parallelism using OpenMP, and distributed-memory parallelism using MPI.

The following approaches are implemented:

- Sequential Matrix Multiplication
- OpenMP Matrix Multiplication
- MPI Matrix Multiplication

## Matrix Details

- Matrix Size: 4000 × 4000
- Input Matrices: A and B
- Output Matrix: C
- Verification Value: `C[0][0] = 4000.00`

---

## 1. Sequential Matrix Multiplication

The Sequential implementation performs the complete matrix multiplication using three nested loops. The calculation is executed by a single CPU thread without any parallel processing.

For each element of the output matrix, the corresponding row of matrix A and column of matrix B are used to calculate the result.

This implementation provides the baseline execution time for comparing the parallel approaches.

### Result

- Matrix Size: 4000 × 4000
- Execution Time: 339.308583 seconds
- Verification: `C[0][0] = 4000.00`

---

## 2. OpenMP Matrix Multiplication

OpenMP is used to parallelize the matrix multiplication on the CPU using multiple threads. The iterations of the computation are divided among the available threads, allowing different parts of the output matrix to be calculated simultaneously.

In this experiment, 8 OpenMP threads were used. Since the threads work in a shared-memory environment, they can access the required matrix data without using message passing.

### Result

- Matrix Size: 4000 × 4000
- Number of Threads: 8
- Execution Time: 41.021555 seconds
- Verification: `C[0][0] = 4000.00`

---

## 3. MPI Matrix Multiplication

MPI is used to implement matrix multiplication using distributed-memory parallelism. The computation is divided among multiple MPI processes running across different virtual machines.

The experiment uses one master process and three worker processes:

- Master
- Worker 1
- Worker 2
- Worker 3

The matrix data is distributed among the processes, and each process performs the multiplication for its assigned portion. The partial results are then collected to form the final output matrix.

The main MPI communication operations used are:

- `MPI_Scatter` – distributes the required matrix data among processes.
- `MPI_Bcast` – broadcasts matrix data to all processes.
- `MPI_Gather` – collects the computed results from the processes.

### Result

- Matrix Size: 4000 × 4000
- Number of MPI Processes: 4
- Execution Time: 226.167575 seconds
- Verification: `C[0][0] = 4000.00`

---

## 4. Comparison of Results

| Method | Processing Model | Threads / Processes | Execution Time (seconds) | Verification |
|--------|------------------|---------------------|--------------------------|--------------|
| Sequential | Single CPU thread | 1 thread | 339.308583 | 4000.00 |
| OpenMP | Shared-memory CPU | 8 threads | 41.021555 | 4000.00 |
| MPI | Distributed-memory | 4 processes | 226.167575 | 4000.00 |

---

## 5. Speedup


### OpenMP Speedup

```text
Speedup = Sequential Time / OpenMP Time

        = 339.308583 / 41.021555

        ≈ 8.27×
