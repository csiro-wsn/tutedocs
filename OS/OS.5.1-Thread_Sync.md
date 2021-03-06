# CSSE4011: Tute 5.1 - Thread Synchronization 

## **1.0 Motivation**

In the previous tutorial, basic thread creation and usage was covered. In this tutorial, we aim to cover thread synchronization.


## **1.1 Thread Synchronization Summary**

Thead synchronization is typically used for sharing of resources without interference (mutual exclusion) and for coordinating the thread functionality and interactions within the operating system. 

For example, think of a resource where multiple threads write to a shared resource. It is important to ensure that only one writer is currently writing to the resource, to avoid interference and data corruption. In such a case, synchronization must be used between the writer threads to ensure intended functionality. Additionally, the application could use coordination between reader and writer threads, so that readers only read when data is ready. 

In an embedded environment, an application might want to enforce that only one thread is accessing hardware (senors etc..) at a time, or to indicate that a particular hardware resource is ready to use. In such a case, synchronization must be used. 

## **2.0 Thread Synchronization and Communication in Zephyr**

## **2.1 Thread Synchronization in Zephyr**

Zephyr offers multiple synchronization primitives, such as:

    1. Semaphores          [1]
    2. Mutexs              [2]
    3. Condition Variables [3]


Refer to the API guides (links section) for detailed information on implementing each of the above. In this tute, we will explore using semaphores to synchronize the blinky thread created in *OS.4-Threading*. 

## **2.2 Semaphore Implementation**

Within Zephyr, a semaphore can be defined by the use of the following macro

```
K_SEM_DEFINE(sem_name, 0, 1);
```

or, using the function  *k_sem_init()*

```
struct k_sem sem_name;

void
main(void)
{
    k_sem_init(&sem_name, 0, 1);
}
```

The typical application of a semaphore can be as seen below:

```
void
interrupt_handler(void *arg)
{
    /* Interrupt indicative of some data ready */
    k_sem_give(&data_ready_sem);
}

void
consumer_thread(void)
{
    while(1)
    {
        if (k_sem_take(&data_ready_sem, K_MSEC(850) != 0)) {
            /* 
             * Data not received within expected timeout of 850ms, 
             * fallback subroutine...
             */
             continue;
        }

        /* Semaphore was attained within time out */

        /* Read the data, and do some work... */
        k_msleep(500);
    }
}
```
The example above, shows a scenario where a semaphore is used to signal by an interrupt handler that some data is ready. Similarly, semaphores can be used between multiple threads for coordination and signaling. 

Refer to [1], for additional implementation information. 

## **2.3 Mutex in Zephyr**

A mutex in Zephyr RTOS is a kernel object that implements the traditional functionality of a mutex. A mutex can allow multiple threads to safely access and share hardware or software resources [2]. Where a semaphore **may allow finite access** (i.e counting semaphore) to a resource, mutexs only allow a thread to access one resource at a time (a locking mechanism). 

The mutex implementation api within Zephyr is functionally similar to that of the semaphore api outlined in ***section 2.2***. See [2] for Zephyr mutex api reference.

## **2.4 Condition Variables in Zephyr**

Zephyr allows for 'Condition Variables' to be used as a synchronization primitive, where, threads can wait on until a particular condition has occurred. Waiting threads will be in a queue when a particular state of execution is not desired (by waiting on the condition).


Zephyr API [3] shows the following example to be  a typical conditional variable implementation with two threads. In the ***main*** thread, 

```
K_MUTEX_DEFINE(mutex);
K_CONDVAR_DEFINE(condvar)

void main(void)
{
    k_mutex_lock(&mutex, K_FOREVER);

    /* block this thread until another thread signals cond. While
     * blocked, the mutex is released, then re-acquired before this
     * thread is woken up and the call returns.
     */
    k_condvar_wait(&condvar, &mutex, K_FOREVER);
    ...
    k_mutex_unlock(&mutex);
}
```
and in the ***worker*** thread,

```
void worker_thread(void)
{
    k_mutex_lock(&mutex, K_FOREVER);

    /*
     * Do some work and fulfill the condition
     */
    ...
    ...
    k_condvar_signal(&condvar);
    k_mutex_unlock(&mutex);
}
```

### **2.4.1 Condition Variable Typical Use**

Condition variables should be used with a mutex to signal changing conditions/states from one thread to another. Condition variables are not ***events*** in that they are not the ***condition*** itself.

**Refer to the implementation API [3] for more details.**

## **3.0 Tutorial Question**

> 1. Use two semaphores to enforce synchronization between the two blinky threads created in tute OS.4-Threading.

The implementation should only use delays to indicate that the led visibly blinks. 

## **3.1 Sample Solution**

A sample solution is uploaded in the docs repository. Find located within,

> tute_solutions/OS-5.1_tute/src/

This code can be built with:

> west build -p -b <board_name>

and flashed with

> west flash -r 'runner'

Refer to the board flashing tutorials for additional build/flash guides.

# Links

[1]https://docs.zephyrproject.org/latest/reference/kernel/synchronization/semaphores.html
[2]https://docs.zephyrproject.org/latest/reference/kernel/synchronization/mutexes.html
[3]https://docs.zephyrproject.org/latest/reference/kernel/synchronization/condvar.html
