# Lab 7

## Results

| Grid Size Multiplier | Kinetic Energy (Step 1950) | CPU Execution Time (ms) | GPU Execution Time (ms) | GPU Speedup (CPU / GPU) |
| :--- | :--- | :--- | :--- | :--- |
| **Default (1x)** | 7745.78 | 930.43 | 528.06 | **1.76x** |
| **4x** | 140558 | 9417.09 | 587.04 | **16.04x** |
| **8x** | 598349 | 29017.40 | 926.90 | **31.31x** |
| **16x** | 2.45694e+06 | 72191.90 | 2082.21 | **34.67x** |

## Conclusion
The results perfectly illustrate the theoretical differences between CPU latency-oriented architecture and GPU throughput-oriented architecture:

1.  **Validation:** Across all grid sizes, the calculated total kinetic energy perfectly matched between the CPU and GPU runs, confirming that the OpenMP target data regions and kernels were implemented correctly without altering the underlying physics or introducing race conditions.
2.  **Small Workload Overhead (1x):** At the default grid size, the GPU is only 1.76 times faster than the 4-core CPU. At this scale, the overhead of launching GPU kernels and moving data across the PCIe bus takes up a significant fraction of the runtime, preventing the GPU from achieving its full potential.
3.  **Massive Scaling Advantage (4x - 16x):** As the grid size increases, the computational workload becomes large enough to fully saturate the GPU's thousands of streaming multiprocessors. By the 16x scale, the CPU execution time skyrockets to over 72 seconds, while the GPU completes the exact same workload in roughly 2 seconds—achieving a massive **~34.7x speedup**.

This scaling study demonstrates that GPU offloading via OpenMP provides tremendous performance benefits for highly parallelizable CFD workloads, provided the problem size is large enough to overcome the initial hardware communication overhead.