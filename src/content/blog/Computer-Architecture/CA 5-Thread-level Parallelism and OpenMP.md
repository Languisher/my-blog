---
author: Nan Lin
pubDatetime: 2025-01-05T10:00:00Z
modDatetime: 2025-01-05T10:00:00.000Z
title: CS X-Thread-level Parallelism and OpenMP
slug: computer-systems-X
featured: false
draft: false
tags:
  - Computer-Systems
description: Lab report of lab 10 of the course ICE4414P-Computer-Architecture.
---

> This article serves as the lab report of lab 10 of the course ICE4414P-Computer-Architecture.

Parallelism is the optimal solution for cases where we could operate the same instruction for multiple data (idea of _SIMD_).
## Parallel Pipeline: Task Division and Workers
- Tasks are divided into separate parts _that_ should be independent from one another.
- These tasks are handled by different "workers," called _threads_ in CS.

## Threads: Workers Operating in Parallel

_Threads are non-deterministic_, which means:
- When multiple threads are spawned simultaneously by a specific instruction, the sequence of their birth is not guaranteed. (Handled by the OS)
- Once the thread has completed its task, it "dies" and is killed. (If there are no explicit synchronization)

### OpenMP: Hello World

Example: Here is a C program based on OpenMP that could spawn several threads.
```c
// openmp_example.c

#include <stdio.h>
#include <omp.h>

int main() {
    // set num of threads
    omp_set_num_threads(8);

    // shared variable is public to all the threads
    int shared_variable = 2025;
    #pragma omp parallel
    {
        // variables defined in the brackets are private to each thread
        int thread_ID = omp_get_thread_num();
        printf(" hello world %d and shared variable is %d\n", thread_ID, shared_variable);
    }
}
```

Then execute
```bash
gcc -std=gnu99 -fopenmp -c -g openmp_example.c -o openmp_example.o
./openmp_example
```

The output is
```txt
 hello world 0 and shared variable is 2025
 hello world 4 and shared variable is 2025
 hello world 2 and shared variable is 2025
 hello world 3 and shared variable is 2025
 ...
```

The code manually sets the number of threads to be spawned.
- Each thread has the shared variables defined outside the braces and private variables defined in the braces.
- `#pragma omp parallel` defines the _directive lines_ for OpenMP.
- The part inside the braces should be executed in parallel _by different threads._

The sequence of spawning the threads is non-deterministic.


## Dividing the Task: How?

The threads could operate simultaneously and accelerate the overall efficiency because _the operated objects are independent_.

The problem now is how we can divide the task (which usually contains various components) into subtasks that could be handled by the "workers".
### Exercise 1: Vector Addition, OMP Parallel

Suppose that we want to calculate
$$
\vec{X} \text{ op } \vec{Y} = \vec{Z} \iff \forall i, \; x[i] \text{ op } y[i] = z[i]
$$

Consider the circumstance that the number of threads is smaller than the size of the vector. Thus, each thread needs to handle _some_ of the elements in the vector. The question is, which elements?

There are two methods, element-wise slices and contiguous chunks, which are shown below:
![](attachment/Lab%2010/Canvas%201.png)

And the corresponding OpenMP Code is:
```c
// ex1.c
#include "ex1.h"

void v_add_naive(double* x, double* y, double* z) {
    #pragma omp parallel
    {
        for(int i=0; i<ARRAY_SIZE; i++)
            z[i] = x[i] + y[i];
    }
}

// Adjacent Method
void v_add_optimized_adjacent(double* x, double* y, double* z) {
    #pragma omp parallel
    {
        int thread_id = omp_get_thread_num();        // Get current thread ID
        int num_threads = omp_get_num_threads();    // Total number of threads

        for (int i = thread_id; i < ARRAY_SIZE; i += num_threads) {
            z[i] = x[i] + y[i];                     // Perform addition for assigned indices
        }
    }
}

// Chunks Method
void v_add_optimized_chunks(double* x, double* y, double* z) {
    #pragma omp parallel
    {
        int thread_id = omp_get_thread_num();        // Get current thread ID
        int num_threads = omp_get_num_threads();    // Total number of threads
        int chunk_size = (ARRAY_SIZE + num_threads - 1) / num_threads; // Divide array into chunks

        // Compute start and end indices for the current thread
        int start = thread_id * chunk_size;
        int end = (start + chunk_size > ARRAY_SIZE) ? ARRAY_SIZE : (start + chunk_size);

        for (int i = start; i < end; i++) {
            z[i] = x[i] + y[i];                     // Perform addition for assigned chunk
        }
    }
}
```

It could be observed that:
```txt
Naive: 96 threads took 3.405434 seconds
Optimized adjacent: 1 thread(s) took 0.960027 seconds
Optimized adjacent: 2 thread(s) took 1.167018 seconds
Optimized adjacent: 3 thread(s) took 0.861504 seconds
...
Optimized adjacent: 95 thread(s) took 0.924595 seconds
Optimized adjacent: 96 thread(s) took 0.957607 seconds
Optimized chunks: 1 thread(s) took 0.942389 seconds
Optimized chunks: 2 thread(s) took 0.473273 seconds
Optimized chunks: 3 thread(s) took 0.321469 seconds
...
Optimized chunks: 95 thread(s) took 0.583043 seconds
Optimized chunks: 96 thread(s) took 0.587023 seconds
Congrats! All vector tests passed!
```

The chunk division method achieves a better result by leveraging the locality principle, implying a higher cache hit rate.

### Exercise 2: Dot Product, OMP Critical and Reduction

The dot product of two vectors is more complicated: Though the product of two objects could be done simultaneously, the threads may try to update the same shared variable (the sum) at the same time.
$$
\text{res} = \sum_{i}x[i] \times y[i]
$$
The update process should not be done in parallel, thus we need to have a section naturally prevents multiple threads from reading and writing to the same data, called the **critical section**.

The implementation is direct:
```c
#include <omp.h>

// Naive Approach
double dotp_naive(double* x, double* y, int arr_size) {
    double global_sum = 0.0;
    for (int i = 0; i < arr_size; i++)
        global_sum += x[i] * y[i];
    return global_sum;
}

// Critical Keyword
double dotp_critical(double* x, double* y, int arr_size) {
    double global_sum = 0.0;
    #pragma omp parallel for
    for (int i = 0; i < arr_size; i++) {
        double product = x[i] * y[i];
        #pragma omp critical
        {
            global_sum += product;
        }
    }
    return global_sum;
}
```

However, the critical implementation does not operate as we wished:
```txt
Naive: 1 thread took 0.804527 seconds
Critical: 1 thread(s) took 4.220957 seconds
Critical: 2 thread(s) took 23.494756 seconds
Critical: 3 thread(s) took 30.598199 seconds
...
```
- In a critical section, only **one thread at a time** is allowed to update the shared variable.
- Other threads must wait their turn, even if they have finished their computation and are ready to contribute.

Thus, the time is proportional to the number of threads. (wow!)

We consider dividing the update process into two steps:
1. Each thread update their _private_ copies of the variable simultaneously
2. The OpenMP _automatically combines_ the private copies into the global variable at the end of the parallel region

This is referred to as **reduction**. The figure below illustrates the difference:


![](attachment/Lab%2010/2.png)

The implementation:
```c
// Reduction Keyword
double dotp_reduction(double* x, double* y, int arr_size) {
    double global_sum = 0.0;
    #pragma omp parallel for reduction(+:global_sum)
    for (int i = 0; i < arr_size; i++) {
        global_sum += x[i] * y[i];
    }
    return global_sum;
}

// Manual Reduction
double dotp_manual_reduction(double* x, double* y, int arr_size) {
    double global_sum = 0.0;

    #pragma omp parallel
    {
        double local_sum = 0.0;
        #pragma omp for
        for (int i = 0; i < arr_size; i++) {
            local_sum += x[i] * y[i];
        }
        #pragma omp critical
        {
            global_sum += local_sum;
        }
    }
    return global_sum;
```

The result is:
```txt
Reduction Optimized: 1 thread(s) took 0.788235 seconds
Reduction Optimized: 2 thread(s) took 0.408900 seconds
Reduction Optimized: 3 thread(s) took 0.277832 seconds
...
Reduction Optimized: 95 thread(s) took 0.599967 seconds
Reduction Optimized: 96 thread(s) took 0.550007 seconds
Manual reduction: 1 thread(s) took 0.812890 seconds
Manual reduction: 2 thread(s) took 0.429523 seconds
Manual reduction: 3 thread(s) took 0.290430 second
...
Manual reduction: 95 thread(s) took 0.812562 seconds
Manual reduction: 96 thread(s) took 0.818877 seconds
Congrats! All dotp tests passed
```