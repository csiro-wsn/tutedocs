# CSSE4011: Tute 5.1 - Thread Synchronization 

## **1.0 Motivation**

In the previous tutorial, basic thread creation and usage was covered. In this tutorial, we aim to cover thread synchronization.


## 1.1 Thread Synchronization Summary

Thead synchronization is typically used for sharing of resources without interference (mutual exclution) and for coordinating the thread functionality and interactions within the operating system. 

For example, think of a resource where multiple threads write to a shared resource. It is important to ensure that only one writer is currently writing to the resource, to avoid interference and data corruption. In such a case, synchronization must be used between the writer threads to ensure intended functionality. Additionally, the application could use coordination between reader and writer threads, so that readers only read when data is ready. 

In an embedded environment, an application might want to enforce that only one thread is accessing hardware (senors etc..) at a time, or to indicate that a particular hardware resource is ready to use. In such a case, synchronization must be used. 

## **2.0 Thread Synchronization and Communication in Zephyr**

## 2.1 Thread Synchronization in Zephyr

Zephyr offer multiple synchronization primitives, such as:

    1. Semaphores          [1]
    2. Mutexs              [2]
    3. Condition Variables [3]


Refer to the API guides (links section) for detailed information on implementing each of the above. In this tute, we will explore using semaphores to syncrhonize the blinky thread created in *OS.4-Threading*. 

## **3.0 Tutorial Question**

> 1. Use two semaphores to enforce syncrhonization between the two blinky threads created in tute OS.4-Threading.

The implementation should only use delays to indicate that the led visibly blinks. 

## **3.1 Sample Solution**

A sample solution is uploaded in the docs repository. Find located within,

> tute_solutions/OS-5.1_tute/src/

This code can be built with:

> west build -p -b <board_name>

and flashed with

> west flash -r 'runner'

Refer to the board flasing tutorials for additional build/flash guides.

# Links

[1]https://docs.zephyrproject.org/latest/reference/kernel/synchronization/semaphores.html
[2]https://docs.zephyrproject.org/latest/reference/kernel/synchronization/mutexes.html
[3]https://docs.zephyrproject.org/latest/reference/kernel/synchronization/condvar.html
