---
title: C programming exercise
linktitle: Programming exercise
content: exercise
weight: 30
---

Before starting working on the tutorials and programming assignments you should make sure you
are familiar with a few [important C programming concepts](important-concepts). 
To test your C programming skills you are encouraged to solve the programming
exercise described below. 


## Clone repository

Before you continue, you must clone the [c-address-book][repo] repository.
From the terminal, navigate to a directory where you want the cloned directory
to be created and execute the following command. 

[repo]: https://github.com/os-ospp-dsp/c-address-book

``` text
$ git clone https://github.com/os-ospp-dsp/c-address-book.git
```

## address_book.h

The functions you need to implement are already declared in `address_book.h`.
You should also define the structures you will need in `address_book.h`. You are
free to create more functions if you want.

## address_book.c

In the file `address_book.c` you should implement the functions declared in
`adress_book.h`. 

## main.c

The `main()` function, which is the entry point of your program will be in a
file called `main.c`.

<!--
## C89

Your code should be using only standard C89 features and functions, especially
variable-lenth-arrays from C99/C11 are not allowed. C++ is not allowed.
-->

## Representing a person

Create a struct Person that will be used to represent a person. This struct
should store:


* The full name
* The age
* The phone number

It is up to you to choose the right datatypes for the fields of the structure.

## Representing the address book

Create a **struct** `Address_book` that will contain a **pointer** to an
**array** of **struct** `Person`, as well as the size of this array (the number
of persons in the address book).

## Printing a person

Create a function `print_person()` that takes a **pointer** to a `Person` **structure** and
prints its details on the standard output. 

A possible output for a person named
John Doe, 42 years old, with the phone number +46712345678:

``` shell
Name: John Doe
Age: 42
Phone number: +46712345678
```

## Printing an address book

Create a function `print_address_book()` that takes a **pointer** to an address book
and prints its details on the standard output. Make use of the `print_person()`
function you just created.

A possible output for an address book containing two
entries is:

``` shell
==== Address book (2 entries) =====

Name: John Doe
Age: 42
Phone number: +46712345678

Name: Foo Bar
Age: 24
Phone number: +46787654321
```


## Creating an address book

We will now read information from the user and store it into an address book.

Create a `create_address_book()` function. This function should:

* Create (dynamically) an empty address book.
* Read from the standard input the number of persons that the user intends to put into the address book.
* Dynamically allocate an array of **struct** `Person` of the correct size and store a pointer to it in the address book. You are not allowed to use Variable Length Arrays!
* In a loop, read from the standard input the information you need for every person to be stored in the address book. Assume that the inputs are correct, so you are not expected to validate them.
* Return the address book.

**Hints**: Dynamic allocation is done with `malloc()`. Reading from the standard input can be done using `scanf()` or `fgets()`.
