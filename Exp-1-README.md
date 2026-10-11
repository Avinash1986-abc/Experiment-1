# Experiment 1 – Parallel Computing

## Objective

To implement and study matrix multiplication using Sequential, OpenMP, MPI, and CUDA approaches and compare their execution performance.

## Introduction

Matrix multiplication is a computationally intensive operation that can be implemented using different parallel computing models. This experiment compares four approaches: sequential CPU processing, shared-memory parallelism using OpenMP, distributed-memory parallelism using MPI, and GPU parallelism using CUDA.

The experiment evaluates the execution time of each approach using matrices of size 4000 × 4000.

## Matrix Details

- **Matrix Size:** 4000 × 4000
- **Input Matrices:** A and B
- **Output Matrix:** C
- **Verification Value:** `C[0][0] = 4000.00`

---

## 1. Sequential Matrix Multiplication

The sequential implementation performs matrix multiplication using three nested loops. All calculations are executed by a single CPU thread.

This implementation provides the baseline execution time for comparing the parallel approaches.

### Result

- **Matrix Size:** 4000 × 4000
- **Execution Time:** 339.308583 seconds
- **Verification:** `C[0][0] = 4000.00`

---

## 2. OpenMP Matrix Multiplication

OpenMP is used to parallelize matrix multiplication on the CPU using multiple threads. The iterations are divided among threads so that different elements of the output matrix can be calculated simultaneously.

In this experiment, 8 OpenMP threads were used. All threads share the same memory space.

### Result

- **Matrix Size:** 4000 × 4000
- **Number of Threads:** 8
- **Execution Time:** 41.021555 seconds
- **Verification:** `C[0][0] = 4000.00`

---

## 3. MPI Matrix Multiplication

MPI (Message Passing Interface) is used to implement matrix multiplication using distributed-memory parallelism. The computation is divided among four MPI processes: one master process and three worker processes.

Each process calculates its assigned portion of the output matrix. The results are collected to form the final matrix.

The main MPI communication operations used are:

- `MPI_Scatter` – Distributes matrix data among processes.
- `MPI_Bcast` – Broadcasts data to all processes.
- `MPI_Gather` – Collects computed results from the processes.

### Result

- **Matrix Size:** 4000 × 4000
- **Number of MPI Processes:** 4
- **Execution Time:** 226.167575 seconds
- **Verification:** `C[0][0] = 4000.00`

---

## 4. CUDA Matrix Multiplication

CUDA is used to perform matrix multiplication on the GPU. The computation is divided among GPU threads, allowing multiple matrix elements to be calculated in parallel.

The CUDA program was compiled using the NVIDIA CUDA compiler (`nvcc`) and executed for matrices of size 4000 × 4000.

The grid contains multiple blocks, and each block contains threads arranged in a two-dimensional configuration.

### Result

- **Matrix Size:** 4000 × 4000
- **Grid Size:** 250 × 250 blocks
- **Block Size:** 16 × 16 threads
- **Threads per Block:** 256
- **Kernel Execution Time:** 0.064499 seconds
- **Total CUDA Phase Time:** 0.077087 seconds
- **Verification:** `C[0][0] = 4000.00`

### CUDA Execution Details

- `nvcc` compiles the CUDA source code.
- The grid organizes the GPU blocks.
- Each block contains 256 threads.
- The CUDA kernel performs matrix multiplication on the GPU.
- The verification value confirms the expected output for the tested matrix element.

---

## 5. Comparison of Results

| Method | Processing Model | Threads / Processes | Execution Time (seconds) | Verification |
|---|---|---|---:|---|
| Sequential | Single-threaded CPU | 1 thread | 339.308583 | 4000.00 |
| OpenMP | Shared-memory CPU | 8 threads | 41.021555 | 4000.00 |
| MPI | Distributed-memory | 4 processes | 226.167575 | 4000.00 |
| CUDA | GPU parallel processing | 256 threads per block | 0.064499 (kernel) | 4000.00 |

**Note:** The CUDA result in the table represents kernel execution time. The total CUDA phase time is 0.077087 seconds. A fair performance comparison requires consistent timing boundaries across implementations.

---

## 6. Speedup Calculations

Speedup is calculated using the formula:

`Speedup = Sequential Execution Time / Parallel Execution Time`

### 6.1 OpenMP Speedup

```text
Speedup = 339.308583 / 41.021555
        ≈ 8.27×
```

### 6.2 MPI Speedup

```text
Speedup = 339.308583 / 226.167575
        ≈ 1.50×
```

### 6.3 CUDA Speedup

Using the kernel execution time:

```text
Speedup = 339.308583 / 0.064499
        ≈ 5,260.66×
```

Using the total CUDA phase time:

```text
Speedup = 339.308583 / 0.077087
        ≈ 4,401.91×
```

**Note:** These speedups are calculated from the recorded execution times. The CUDA speedup should not be treated as a fully comparable end-to-end speedup unless the timing boundaries and measurement conditions are equivalent.

---

## 7. Performance Analysis

- **Sequential:** Executes all computations using one CPU thread and has the longest execution time.
- **OpenMP:** Improves performance by distributing computations among multiple CPU threads.
- **MPI:** Distributes work among multiple processes, but communication and data distribution can introduce overhead.
- **CUDA:** Executes matrix multiplication using GPU threads and records a kernel execution time of 0.064499 seconds.

The results demonstrate the potential benefits of parallel computing for computationally intensive matrix multiplication.

---

## 8. Conclusion

In this experiment, matrix multiplication was implemented using Sequential, OpenMP, MPI, and CUDA approaches for matrices of size 4000 × 4000.

OpenMP reduced the execution time by using multiple CPU threads, while MPI distributed the computation across four processes. CUDA achieved a significantly shorter recorded kernel execution time by using GPU parallelism.

All implementations reported the expected verification value, `C[0][0] = 4000.00`. The experiment demonstrates the differences between sequential processing, shared-memory parallelism, distributed-memory parallelism, and GPU-based parallel computing.
