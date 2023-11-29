---
title: Bounded buffer
weight: 300
assignment: mandatory
draft: false
---

<h2 class="subtitle">Mandatory assignment</h2>

![](/v1/images/threads-and-synchronization/bounded-buffer-medium-details.png?width=555px)

## Introduction

The bounded-buffer problems (aka the producer-consumer problem) is a classic
example of concurrent access to a shared resource. A bounded buffer lets
multiple producers and multiple consumers share a single buffer. Producers write data
to the buffer and consumers read data from the buffer.

- Producers must block if the buffer is full.
- Consumers must block if the buffer is empty.

## Preparations

Among the [slides][slides] you find self study material about classic
[synchronization problems][bb-slides]. In these slides the bounded buffer
problem is described. Before you continue, make sure to read the self study
slides about the bounded buffer problem.

[slides]: https://github.com/uu-dsp-os-ospp-2020/os-slides

[bb-slides]: https://github.com/uu-dsp-os-ospp-2020/os-slides/blob/master/m4-self-study-synchronisation-problems.pdf

## Synchronization 

A bounded buffer with capacity `N` has can store `N` data items. The places used
to store the data items inside the bounded buffer are called slots. Without
proper synchronization the following errors may occur.

- The producers doesn’t block when the buffer is full.
- Two or more producers writes into the same slot.
- The consumers doesn't block when the buffer is emtpy.
- A Consumer consumes an empty slot in the buffer. 
- A consumer attempts to consume a slot that is only half-filled by a producer.
- Two or more consumers reads the same slot.
- And possibly more ...

## Overview of the provided source code 

You will implement the bounded buffer as an [abstract datatype][wp-adt]. These
are the files you will use to implement and test your implementation. 

bounded_buffer.h
: The internal representation of the bounded buffer and the public API is
defined in `module-4/mandatory/src/bounded_buffer.h`.

bounded_buffer.c
: The implementation of the API will be in the
  `module-4/mandatory/src/bounded_buffer.c` file.

bounded_buffer_test.c
: To test your various aspect of your implementation a collection of tests are provided in the
  `module-4/mandatory/src/bounded_buffer_test.c` file. Here you can add your own
  tests if you want. 
  
bounded_buffer_stress_test.c
: The `module-4/mandatory/src/bounded_buffer_test.c` program make it easy to
  test the implementation for different sizes of the buffer and different numbers
  of producers and consumers.
  
[wp-adt]: https://en.wikipedia.org/wiki/Abstract_data_type

## Supported platforms

The provided code has been developed and tested on the department Linux system
and macOS. Most likely you will be able to use any resonable modern version of
Linux. 

<!--
If you use Windows you must use the department Linux system for this assignment.
You may still use your private computer to access the department Linux system
using [SSH][ssh] but make sure to log in to one of
the [Linux hosts][linux-hosts].

[ssh]: http://www.it.uu.se/datordrift/faq/ssh?lang=enforce

[linux-hosts]: https://www.it.uu.se/datordrift/maskinpark/linux

[piazza]: https://piazza.com/class/jqzauvl4hqm6v9?cid=80#

--> 

## Semaphores

You will us the [psem](psem) semaphores to synchronize the bounded buffer. 

## Data structures

To implement the bounded buffer you will use two C [structures][struct-tutorial]
and an [array][array-tutorial] of C structures.

[array-tutorial]: https://www.tutorialspoint.com/cprogramming/c_arrays.htm
[struct-tutorial]: https://www.tutorialspoint.com/cprogramming/c_structures.htm

### Tuple

The buffer will store [tuples][wp-tuple] (pairs) with two integer 
elements `a` and `b`. A tuple is represented by the following C struct. 

``` C
typedef struct {
  int a;
  int b;
} tuple_t;
```

[wp-tuple]: https://en.wikipedia.org/wiki/Tuple

### Buffer array

To implement the bounded buffer a finite size **array** in memory is shared by a
number for producer and consumer threads.

- Producer threads “produce” an item and place the item in the array.
- Consumer threads remove an item from the array and “consume” it.

In addition to the shared array, information about the state of the buffer must
also be shared.

- Which slots in buffer are free?
- To which slot should the next data item be stored?
- Which parts of the buffer are filled?  
- From which slot should the next data item be read?

In the below example three data items B, C and D are currently in the buffer. On
the next write data will be written to index `in = 4 `. On the next read
data will be read from index `out = 1`.

![](/v1/images/threads-and-synchronization/bounded-buffer-details.png?width=666px)


### Buffer struct

The buffer is represented by the following C struct. 

``` C
typedef struct {
  tuple_t *array;
  int     size;
  int     in;
  int     out;
  psem_t  *mutex;
  psem_t  *data;
  psem_t  *empty;
} buffer_t;
```

The purpose of the struct members are described below. 

tuple_t *array
: This pointer will point to a dynamically allocated array of buffer items of
type `tuple_t`. 

int size
: The number of elements in the array, i.e., the size of the buffer. 

int out
: The index in the array where the next items should be produced. 

int in
: The index in the array from where the next item should be consumed. 

psem_t *mutex
: A binary semaphore used to enforce mutual exclusive updates to the buffer. 

psem_t *data
: A counting semaphore used to count the number of items in the buffer. This
semaphore will be used to block consumers if the buffer is empty. 

psem_t *empty
: A counting semaphore used to count the number of empty slots in the buffer.
This semaphore will be used to block producers if the buffer is full.


## Critical sections and mutual exclusion

All updates to the buffer state must be done in a critical section. More
specifically, mutual exclusion must be enforced between the following critical
sections:

- A **producer writing** to a buffer slot and **updating** `in`. 
- A **consumer reading** from a buffer slot and **updating** `out`.

A **binary semaphore** can be used to protect access to the critical sections. 

## Synchronize producers and consumers 

Producers must block if the buffer is full. Consumers must block if the buffer is
empty. Two **counting semaphores** can be used for this.

Use one **semaphore** named **empty** to count the empty slots in the buffer. 

- **Initialise** this semaphore to **N**.  
- A **producer** must **wait** on this semaphore before writing to the buffer.
- A **consumer** will **signal** this semaphore after reading from the buffer.

Use one **semaphore** named **data** to count the number of data items in the buffer. 

- **Initialise** this semaphore to **0**. 
- A **consumer** must **wait** on this semaphore before reading from the buffer. 
- A **producer** will **signal** this semaphore after writing to the buffer.

## Initialization example

A new bounded buffer with 10 elements will be represented as follows. 

<img src="/v1/images/threads-and-synchronization/buffer-internals-example.png" style="width:444px;"/>

The `empty` semaphore counts the number of
empty slots in the buffer and is initialized to `10`. Initially there are no
data element to be consumed in the buffer and the `data` semaphore is
initialized to `0`. The `mutex` semaphore will be used to enforece mutex when
updating the buffer and is initialized to `1`.


## Implementation

To complete the implementation you must add code at a few places. 

### Pointers to C structs

Make sure you know how to use [pointers to C strutcs][struct-pointer-tutorial]
before you continue. 

[struct-pointer-tutorial]: https://www.programiz.com/c-programming/c-structures-pointers

### Step by step workflow

You will complete the implementaion step by step.

For each step you will need to add code to a single function in the 
`module-4/mandatory/src/bounded_buffer.c` source file. For each step there
is also a test in the  `module-4/mandatory/src/bounded_buffer_test.c` source
file that should pass without any failed assertions. 

In the terminal, navigate to the `module-4/mandatory` directory. Use [make][wp-make] to compile the program.

[wp-make]: https://en.wikipedia.org/wiki/Make_(software)

``` text
$> make
```

Run the test(s).

``` text
$> ./bin/bounded_buffer_test
```

An example of a failed test where the assertion that the buffer size should be
10 fails. 

``` C
==== init_test ====

Assertion failed: (buffer.size == 10), function init_test, file src/bounded_buffer_test.c, line 24.
```

When a test passes you will see the following output. 

``` text
Test SUCCESSFUL :-)
```


### Step 1 - Buffer initialization

You must complete the buffer initialization. 

Function
: `buffer_init`

Test
: `init_test`

Make sure to initialize all members of the `buffer_t` struct. 

### Step 2 - Buffer destruction

You must complete the buffer initialization. 

Function
: `buffer_destroy`

Test
: `destroy_test`

Deallocate all resources allocated by `buffer_init()`.

- Use [free()][free-tutorial] to dealloacte the buffer array. 
- Use `psem_destroy()` to deallocate all semaphores. 
- Set all pointers to `NULL` after dealloacation to avoid [dangling
       pointers][wp-dangling-pointer].
  

[free-tutorial]: https://www.tutorialspoint.com/c_standard_library/c_function_free.htm

[wp-dangling-pointer]: https://en.wikipedia.org/wiki/Dangling_pointer

### Step 3 - Print the buffer

Function
: `buffer_print`

Test
: `print_test`

Add code to print all elements of the buffer array. 

### Step 4 - Put an item into the buffer

Function
: `buffer_put`

Test
: `put_test`

Here you need to: 

- add the needed synchronization
- update the `buffer->in` index make sure it wraps around. 

### Step 5 - Get an item out of the buffer

Function
: `buffer_get`

Test
: `get_test`

Here you need to: 

- add the needed synchronization
- update the `buffer->out` index make sure it wraps around.

### Step 6 - Concurrent puts and gets

For this step you only need to run the `concurrent_put_get_test`. If the test
doesn't terminate you most likely have made a mistake with the synchronization of
`buffer_put` and/or `buffer_get` that result in a deadlock. 

## Stress testing 

It is now time to test the bounded buffer with multiple concurrent producers and
consumers. In `module-4/mandatory/src/bounded_buffer_stress_test.c` you find a
complete test program that creates:

- a bounded buffer of size `s`
- `p` producer threads, each producing `n` items into the buffer
- `c` consumer threads, each consuming `m` items from the buffer

For the test program to terminate, the total number of consumed items (`c*m`)
must equal the total number of items produced (`p*n`). Run the stress test. 

``` text 
$> ./bin/bounded_buffer_stress_test
```

### Default stress test

The default stress test will now be exuded. On success you should see output
similar to the following. 

``` text 
Test buffer of size 10 with:

 20 producers, each producing 10000 items.
 20 consumers, each consuming 10000 items.

Verbose: false

The buffer when the test ends. 

---- Bounded Buffer ----

size: 10
  in: 0
 out: 0

array[0]: (7, 9994)
array[1]: (15, 9999)
array[2]: (13, 9998)
array[3]: (4, 9999)
array[4]: (7, 9995)
array[5]: (13, 9999)
array[6]: (7, 9996)
array[7]: (7, 9997)
array[8]: (7, 9998)
array[9]: (7, 9999)

---------------------draft: false
---


====> TEST SUCCESS <====
```

Note the `====> TEST SUCCESS <====` at the end. 

### Overiding the default parameters

The default values for test parameters can be overrided using the following
flags. 

| Flag | Description                               | Argument         |
|:----:|-------------------------------------------|------------------|
| -s   | Size of the buffer                       | positive integer |
| -p   | Number of producer threads                | positive integer |
| -n   | Number of items produced by each producer | positive integer |
| -c   | Number of consumer threads                | positive integer |
| -m   | Number of items consumed by each consumer | positive integer |
| -v   | Turns on verbose output                    | no argument      |
   
In the following example, the default buffer size 10 is changed to 3. 

``` text 
$> ./bin/bounded_buffer_stress_test -s 3
```

Experiment with different values for the test parameters. 

## Code grading questions

Here are a few examples of questions that you should be able to answer, discuss
and relate to the source code of you solution during the code grading.

- What do we mean by a counting semaphore?
- What happens when you do wait on counting semaphore?
- What happens when you do signal on a counting semaphore?
- Explain how producers and consumers are synchronized in order to: 
    - block consumers if the buffer is empty
    - block producers if the buffer is full.
- Explain why mutex locks cannot be used to synchronize the blocking of
  consumers and producers. 
- Explain why you must ensure mutual exclusive (mutex) when updating the the
  buffer `array` and the `in` and `out` array indexes. 
- Explain how you achieve mutual exclusion (mutex) when updating the the
  buffer `array` and the `in` and `out` array indexes.
