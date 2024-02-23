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

The simple bank API is defined in `bank.h`.

A single bank account is represented by the following C structure. 

``` C 
typedef struct {
  int balance;

} account_t;

```

What more is needed to be added to the structure?

- One ore more pthread mutex locks?
- Anything else?

These are the functions in the simple bank API. 

```C
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
 * return value:
 *   Return -1 if not sufficient funds in the from account. Otherwise returns 0.
 *
 */
int transfer(int amount, account_t* from, account_t* to);

```


## Add synchronization

You must add the needed synchronization.

## Prevent deadlocks

You must make sure to prevent deadlocks. 

## Pthread mutex locks

This is an example of how you can declare and initialize a [Pthread mutex lock][pthread-mutex]. 


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


## Compile and run the test of the simple bank API.

Compile:

``` bash session
make
```

, and run the test program: 

``` bash session
./bin/bank_test
```


## Example of correct synchronization

This is an example showing two rounds of correct synchronization.

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

From  Amount    To  Result

7 ---- 200 ---> 8   Insufficient funds
1 ---- 200 ---> 7   Insufficient funds
2 ---- 200 ---> 7   Insufficient funds
0 ---- 150 ---> 4   Ok
3 ---- 200 ---> 0   Ok

Account Amount
------------------
0	      550
1	      0
2	      0
3	      0
4	      150
5	      0
6	      0
7	      0
8	      0
9	      0
------------------
     Sum: 700

Total amount of money was initially 700 and is now 700.

System invariant (conservation of money) not broken.

```

## Example of incorrect synchronization

In this example, everything looks ok in round 4, but in round 5 a race condition
is detected. 

``` bash session
Round 4 of 100

From  Amount    To  Result

8 ---- 050 ---> 3   Insufficient funds
1 ---- 200 ---> 7   Insufficient funds
4 ---- 100 ---> 6   Insufficient funds
2 ---- 150 ---> 6   Insufficient funds
4 ---- 150 ---> 7   Insufficient funds

Account Amount
------------------
0	      500
1	      50
2	      0
3	      50
4	      0
5	      0
6	      100
7	      0
8	      0
9	      0
------------------
     Sum: 700

Total amount of money was initially 700 and is now 700.

System invariant (conservation of money) not broken.

Round 5 of 100

From  Amount    To  Result

4 ---- 250 ---> 9   Insufficient funds
9 ---- 150 ---> 3   Insufficient funds
8 ---- 100 ---> 0   Insufficient funds
0 ---- 100 ---> 5   Ok
0 ---- 100 ---> 9   Ok

Account Amount
------------------
0	      400
1	      50
2	      0
3	      50
4	      0
5	      100
6	      100
7	      0
8	      0
9	      100
------------------
     Sum: 800

Total amount of money was initially 700 and is now 800.

RACE CONDITION: System invariant (conservation of money) broken!
```


## Deadlock

You must make sure the simulation never deadlocks. If a deadlock occur, the
simulation will halt, for example like this. 

```bash_session
Round 57 of 100

From  Amount    To  Result

1 ---- 250 ---> 7   Insufficient funds
6 ---- 150 ---> 3   Insufficient funds
6 ---- 100 ---> 3   Insufficient funds
9 ---- 100 ---> 7   Insufficient funds
8 ---- 100 ---> 5   Ok

Account Amount
------------------
0	      50
1	      50
2	      50
3	      50
4	      0
5	      100
6	      0
7	      100
8	      300
9	      0
------------------
     Sum: 700

Total amount of money was initially 700 and is now 700.

System invariant (conservation of money) not broken.

Round 58 of 100

From  Amount    To  Result

0 ---- 200 ---> 1   Insufficient funds
2 ---- 050 ---> 7   Ok
3 ---- 200 ---> 7   Insufficient funds
```
