---
title: Portable semaphores
weight: 150
draft: false
---

Linux and macOS natively supports different types of semaphores. In order to
have a single (simple) semaphore API that works on both Linux and macOS you will
use a portable semaphore library named `psem`.

In the `psem/psem.h` header file you find the documentation of the
portable semaphore API.

## Example usage

A small example program that demonstrates how to use the sempaphores provided by
the `psem` library. 

``` c
#include "psem.h"

int main(void) {
  // Create a new semaphore and initialize the semaphore counter to 3. 
  psem_t *sem = psem_init(3);

  // Wait on the semaphore. 
  psem_wait(sem);

  // Signal on the semaphore. 
  psem_signal(sem);

  // Destroy the semaphore (deallocation)
  psem_destroy(sem)
}  
```

## Example program 

In `mandatory/src/psem_test.c` you find a complete example program with
two threads (main and a pthread) that synchronize their execution using a `psem`
semaphore. 

### Compile

In the terminal, navigate to the `examples` directory. Use
[make][wp-make] to compile the program.

[wp-make]: https://en.wikipedia.org/wiki/Make_(software)

``` bash session
make
```

### Run 

Run the program from the terminal.

``` bash session
./bin/psem_test
```

