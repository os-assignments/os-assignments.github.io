---
title: Simple bank simulation
assignment: higher-grade
points: 1
weight: 211
draft: false
---

<h2 class="subtitle">Optional assignment for higher grade (1 point)</h2>

{{% notice  style="tip" title="New assignment" %}}
  This is a new  1 point higher grade assignment replacing the [N Thread barrier
  ](./n-thread-barrier) assignment. 

{{% /notice %}}

In this higher grade assignment you will do a simple simulation of money
transfers between bank accounts. 

## Git pull

In order to get the new source files and the updated `Makefile` for this new
assignment, you need to pull the updates from the GitHub repo. 

``` bash
git pull
```

## Overview

In the `higher-grade/src` directory you find the following files. 

bank.h
: Header file with the API of the simple bank.

bank.c
: The implementation of the bank API.

bank_test.c
: A program testing the implemented bank API.


## The bank API

The bank API is defined in `bank.h`.

``` C 

/**
 *  A bank account represented by the following C structure.
 *
 *  NOTE: You may need to add members to this structure.
 */
typedef struct {
  int balance;

} account_t;

/**
 * account_new()
 *   Creates a new bank account.
 *
 * balance
 *   The initial balance for the new account.
 *
 * NOTE: You may need to add more parameters.
 *
 */
account_t* account_new(unsigned int balance);

/**
 * account_destroy()
 *   Destroys a bank account, freeing the resources it might hold.
 *
 * account:
 *   Pointer to a bank account.
 *
 */
void account_destroy(account_t* account);

/**
 * transfer()
 *   Attempts to transfer money from one account to another.
 *
 * amount:
 *   Amount to transfer.
 *
 * from:
 *   The account to transfer money from. Money should only be transferred if
 *   thee are sufficient funds.
 *
 * to:
 *   The account to transfer the money to.
 *
 */
void transfer(int amount, account_t* from, account_t* to);

```

What more is needed to be added to the structure to implement the barrier? For example, do you need to add:

- one ore more pthread mutex locks?
- anything else?

## Add synchronization

You must add the needed synchronization to avoid data races. 

## Prevent deadlocks

You must make sure to prevent deadlocks. 

## Pthread mutex locks

This is how you can declare and initialize a [Pthread mutex lock][pthread-mutex]. 


``` C
pthread_mutex_t mutex;

if (pthread_mutex_init(&mutex, NULL) < 0) {
  perror("Init mutex lock");
  exit(EXIT_FAILURE);
}
```

[pthread-mutex]: https://man7.org/linux/man-pages/man3/pthread_mutex_lock.3p.html



## Testing the bank API implementation 

In `bank_test.c` you find a working program testing your implementation. 


## Compile and run the test

Compile:

``` bash session
make
```

, and run the test program: 

``` bash session
./bin/bank_test
```


## Example of incorrect synchronization

This is an example of incorrect synchronization.

``` bash session

Account Amount
------------------
0	      500
1	      0
2	      0
3	      200
4	      0
5	      0
6	      0
7	      0
8	      0
9	      0
------------------
   Sum: 700

Round 1 of 100

7 --- 200 ---> 2 Insufficient funds
5 --- 250 ---> 8 Insufficient funds
2 --- 050 ---> 7 Insufficient funds
3 --- 150 ---> 0
3 --- 150 ---> 2

Account Amount
------------------
0	      650
1	      0
2	      150
3	      -100
4	      0
5	      0
6	      0
7	      0
8	      0
9	      0
------------------
   Sum: 700

ERROR: Negative balance due to DATA RACE!
```

## Example of correct  synchronization

This is an example showing two round of correct synchronization.

``` bash session
Account Amount
------------------
0	      500
1	      0
2	      0
3	      200
4	      0
5	      0
6	      0
7	      0
8	      0
9	      0
------------------
   Sum: 700

Round 1 of 100

8 --- 150 ---> 5 Insufficient funds
3 --- 250 ---> 8 Insufficient funds
1 --- 250 ---> 9 Insufficient funds
4 --- 100 ---> 8 Insufficient funds
3 --- 200 ---> 2

Account Amount
------------------
0	      500
1	      0
2	      200
3	      0
4	      0
5	      0
6	      0
7	      0
8	      0
9	      0
------------------
   Sum: 700

SUCCESS: System invariant not broken!

Total amount of money was initially 700 and is now 700.

Round 2 of 100

7 --- 250 ---> 4 Insufficient funds
1 --- 050 ---> 2 Insufficient funds
7 --- 250 ---> 9 Insufficient funds
3 --- 050 ---> 2 Insufficient funds
0 --- 250 ---> 5

Account Amount
------------------
0	      250
1	      0
2	      200
3	      0
4	      0
5	      250
6	      0
7	      0
8	      0
9	      0
------------------
   Sum: 700

SUCCESS: System invariant not broken!

Total amount of money was initially 700 and is now 700.
```

## Deadlock

You must make sure the simulation never deadlocks. If a deadlock occur, the
simulation will halt, for example like this. 

```bash_session
Round 96 of 100

6 --- 250 ---> 0 Insufficient funds
5 --- 150 ---> 8
2 --- 100 ---> 4
8 --- 100 ---> 1
2 --- 100 ---> 6

Account Amount
------------------
0	      100
1	      150
2	      0
3	      0
4	      150
5	      0
6	      100
7	      100
8	      100
9	      0
------------------
   Sum: 700

SUCCESS: System invariant not broken!

Total amount of money was initially 700 and is now 700.

Round 97 of 100

2 --- 100 ---> 5 Insufficient funds
0 --- 050 ---> 6
7 --- 050 ---> 1
```
