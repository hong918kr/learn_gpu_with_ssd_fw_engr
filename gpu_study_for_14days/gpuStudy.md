# 14-Day GPU Study Plan for HPC Engineers

## Introduction: From Systems Programming to High-Performance Computing

This 14-day intensive study plan is designed for experienced C/C++ engineers who want to dive deep into the world of GPU programming and High-Performance Computing (HPC). If you have a strong background in systems-level programming, hardware-software interaction, and memory hierarchies, this guide will leverage that expertise to accelerate your learning.

The goal is not just to learn CUDA syntax but to build a fundamental understanding of GPU architecture and the principles of parallel computing. This plan is structured to develop a principled approach to performance optimization—an essential skill for any GPU Infrastructure or HPC Software Engineer.

### The Roadmap

This two-week journey is logically structured. **Week 1** focuses on mastering the architecture and programming model of a single GPU. **Week 2** expands this knowledge to an infrastructure level, covering multi-GPU and distributed systems, which are crucial for tackling real-world, large-scale problems.

---

## Week 1: Mastering the Single GPU

**Objective:** To build a deep, hardware-level understanding of the GPU and master the core principles of CUDA programming and optimization.

### Day 1-2: The GPU Computing Paradigm & Architecture

* **Study (The "Why"):**
    * **CPU (Latency-oriented) vs. GPU (Throughput-oriented) Architectures:** Analyze why thousands of simple cores can outperform a few complex ones for parallel tasks.
    * **NVIDIA GPU Architecture Deep Dive:** Research the roles of the Streaming Multiprocessor (SM), CUDA Cores, Warps (groups of 32 threads), and the Warp Scheduler. Understand the SIMT (Single Instruction, Multiple Thread) execution model.
* **Practice (The "How"):**
    * Set up your environment by installing the latest **NVIDIA CUDA Toolkit** or preparing a **Google Colab** notebook.
    * Clone the official `cuda-samples` repository, then compile and run the `vectorAdd` example to verify your setup and learn the basics of the `nvcc` compiler.

### Day 3: The CUDA Programming & Memory Model

* **Study:**
    * **CUDA Programming Model:** Understand the hierarchy of **Grid -> Block -> Thread**. Learn how to use thread and block indices (`threadIdx`, `blockIdx`, etc.) to map threads to data.
    * **GPU Memory Model:** Study the characteristics (scope, speed, size) of the different memory spaces: Global, Shared, Constant, and Registers.
* **Practice:**
    * Write a simple 2D matrix addition kernel from scratch.
    * Experiment with different grid and block dimensions to solidify your understanding of thread indexing.

### Day 4-5: The Core of CUDA Optimization: Memory Access Patterns

* **Study:**
    * **Global Memory Coalescing:** Learn why it is critical for performance that parallel threads access contiguous locations in global memory.
    * **Shared Memory (Tiling):** Understand the "tiling" technique, where data is staged from slow global memory to fast on-chip shared memory to maximize reuse and reduce latency. Learn the role of `__syncthreads()` for synchronization.
* **Practice:**
    * Implement the canonical CUDA optimization exercise: **tiled matrix multiplication**.
    * Code a naive version first (using only global memory) and then a tiled version. Measure and compare the performance difference.

### Day 6-7: Profiling-Driven Optimization & Week 1 Review

* **Study:**
    * Learn the basics of NVIDIA's professional profiling tools: **Nsight Systems** (for application-level timeline analysis) and **Nsight Compute** (for in-depth kernel analysis).
* **Practice:**
    * Profile your tiled matrix multiplication kernel with **Nsight Compute**.
    * Analyze the "Memory Workload Analysis" section to verify that your global memory accesses are coalesced and check for shared memory bank conflicts.
    * Use the profiler's feedback to iterate and improve your kernel's performance.
    * **Project:** Begin a simple image processing project (e.g., grayscale conversion or a box blur) to apply all the concepts learned this week.

---

## Week 2: Scaling Up to HPC Infrastructure

**Objective:** To extend your knowledge from a single GPU to the broader context of large-scale systems, data centers, and real-world HPC workloads.

### Day 8: Asynchronous Operations and Concurrency

* **Study:**
    * **CUDA Streams:** Learn how to use streams to overlap data transfers (Host-to-Device, Device-to-Host) with kernel execution, hiding memory latency and maximizing GPU utilization.
* **Practice:**
    * Apply CUDA streams to your image processing project. Divide the image into chunks and create a pipeline where `[Copy Chunk N]` -> `[Process Chunk N-1]` -> `[Copy Back Chunk N-2]` run concurrently.
    * Use **Nsight Systems** to visualize the execution timeline and confirm that your operations are successfully overlapping.

### Day 9: Advanced Parallel Algorithms & The Library Ecosystem

* **Study:**
    * **Atomic Operations:** Understand how to perform thread-safe updates to the same memory location (e.g., for building histograms).
    * **Parallel Reduction:** Learn the common and powerful reduction pattern, a key building block in many HPC algorithms.
    * **The Ecosystem:** Acknowledge that you don't have to write everything from scratch. Learn about the purpose of key NVIDIA libraries like **cuBLAS** (linear algebra), **cuDNN** (deep learning), and **Thrust** (a C++ template library for parallel algorithms).
* **Practice:**
    * Replace your custom matrix multiplication code with a call to the **cuBLAS** library. Compare the performance and development effort.

### Day 10-11: Large-Scale System Architecture: Multi-GPU & Multi-Node

* **Study (Scale-Up):**
    * **Intra-Node Communication:** Learn how **NVLink** and **NVSwitch** provide high-bandwidth, direct communication between GPUs within a single server, enabling Peer-to-Peer (P2P) memory access.
    * **HPC Servers:** Research the architecture of systems like the NVIDIA DGX.
* **Study (Scale-Out):**
    * **Inter-Node Communication:** Understand the role of high-speed interconnects like **InfiniBand** and **RDMA (Remote Direct Memory Access)**.
    * **Programming Models:** Learn the basic concepts of the **Message Passing Interface (MPI)**, the standard for multi-node communication.

### Day 12: GPUs in the Data Center: Virtualization & Containers

* **Study:**
    * **GPU Virtualization:** Learn about technologies like **SR-IOV** and **MIG (Multi-Instance GPU)** that allow a single physical GPU to be partitioned and shared among multiple users or processes.
    * **Reproducible Environments:** Understand how **Docker** and the **NVIDIA Container Toolkit** are used to create portable, isolated, and GPU-enabled environments for development and deployment.
* **Practice:**
    * Install Docker and the NVIDIA Container Toolkit.
    * Create a Dockerfile for your image processing project and run it inside a GPU-enabled container.

### Day 13-14: Portfolio Project & Final Review

* **Focus:**
    * **GPUDirect Storage:** Deep dive into this technology, which allows for a direct data path between storage (like NVMe SSDs) and GPU memory, bypassing the CPU. Understand its architecture and performance benefits.
* **Finalize Portfolio Project:**
    * Commit your completed image processing or matrix multiplication project to a personal GitHub repository.
    * Write a detailed **`README.md`** for the project that includes:
        1.  A clear problem description.
        2.  An explanation of the CUDA optimization techniques you applied.
        3.  Screenshots from Nsight Compute/Systems showing performance improvements (Before vs. After).
        4.  A summary of what you learned.

By completing this plan, you will have gained a robust understanding of GPU computing, from low-level kernel optimization to high-level system architecture, positioning you as a strong candidate for roles in the GPU and HPC space. Good luck!