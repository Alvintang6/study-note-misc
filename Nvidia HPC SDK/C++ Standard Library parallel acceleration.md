

Following notes is collecting from Nvidia HPC SDK manual https://docs.nvidia.com/hpc-sdk/pdf/hpc231c++_par_alg.pdf and [GPU Acceleration with the C++ Standard Library Course](https://courses.nvidia.com/courses/course-v1:DLI+S-AC-08+V1/) 

## Chapter 1.INTRODUCTION
From C++ 17 defines three execution policies:
‣ std::execution::seq: Sequential execution. No parallelism is allowed.
‣ std::execution::par: Parallel execution on one or more threads.
‣ std::execution::par_unseq: Parallel execution on one or more threads, with
each thread possibly vectorized.

The C++ Standard grants compilers great freedom to choose if, when, and how
to execute algorithms in parallel as long as the forward progress guarantees the
user requests are honored. For example, std::execution::par_unseq may be
implemented with vectorization and std::execution::par may be implemented with
Introduction
C++ Parallel Algorithms Version 23.1 | 2
a CPU thread pool. It is also possible to execute parallel algorithms on a GPU, which
is a good choice for invocations with sufficient parallelism to take advantage of the
processing power and memory bandwidth of NVIDIA GPU processors.


## Chapter 2.NVC++ COMPILER PARALLEL SUPPORT
GPU acceleration of C++ Parallel Algorithms is enabled with the ```-stdpar=gpu``` commandline option to NVC++. If ```-stdpar=gpu``` is specified (or ```-stdpar``` without an argument),
almost all algorithms that use a parallel execution policy are compiled for offloading to
run in parallel on an NVIDIA GPU:
```nvc++ -stdpar=gpu program.cpp -o program```
```nvc++ -stdpar program.cpp -o program```
Acceleration of C++ Parallel Algorithms with multicore CPUs is enabled with the
```-stdpar=multicore``` command-line option to NVC++. If ```-stdpar=multicore``` is specified,
all algorithms that use a parallel execution policy are compiled to run in on a multicore
CPU:
```nvc++ -stdpar=multicore program.cpp -o program```


 ### 2.2. Comparing the performence under different parallel strategy 


```shell
g++ -std=c++17 -Ofast -march=native -DNDEBUG -o testgxx test.cpp
```

```shell
sudo apt install libtbb-dev
nvc++ -std=c++17 -O4 -fast -stdpar=multicore -o testnv_mp test.cpp
```

```shell
nvc++ -std=c++17 -O4 -fast -stdpar=gpu -o testnv_gpu test.cpp
```

Considering following sample code, for g++ build we remove ```std::execution::par```.
```c++
#include <algorithm>
#include <iostream>
#include <execution>
#include <chrono>
#include <vector>
#include <stddef.h>


int main()
{
    std::vector<int> arr(100000000);

    auto start = std::chrono::high_resolution_clock::now(); 
    std::for_each( std::execution::par, begin(arr), std::end(arr), 
                    [arrptr = arr.data()](auto & x)
                    { 
                        ptrdiff_t i = &x - arrptr;
                        x = i*100 + 2;
                    }
                  
                  );
    auto end = std::chrono::high_resolution_clock::now();
    std::cout<<arr.at(5)<<std::endl;
    std::chrono::duration<double> diff = end - start;
    std::cout << "elapsed time = " << diff.count()<<std::endl;
    return 0;
}

```


```
root@DESKTOP-M63LVN6:/mnt/c/Users/jie/Desktop/WSL/nvidia_hpc/compiler_performence_test# ./testnv_gpu
502
elapsed time = 0.0289199 s
root@DESKTOP-M63LVN6:/mnt/c/Users/jie/Desktop/WSL/nvidia_hpc/compiler_performence_test# ./testnv_mp
502
elapsed time = 0.0334202 s
root@DESKTOP-M63LVN6:/mnt/c/Users/jie/Desktop/WSL/nvidia_hpc/compiler_performence_test# ./testgxx
502
elapsed time = 0.0652803 s
```