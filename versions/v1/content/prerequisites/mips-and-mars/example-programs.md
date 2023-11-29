---
title: Mips assembly examples
weight: 30
---


To help you refresh your MIPS assembly skills you are strongly encouraged to
study this small collection of example programs. Each program demonstrates a
small collection of features of the MIPS assembly language.

## The mips-examples repository

Before you continue you should already have cloned the
`module-0-mips-examples` repository. If you have not done this already,
follow these [instructions](clone-repository) before you continue. 
For each of the example files in the repository:

1. Read the source code.
2. Load the program in MARS and single step the program.
3. Try to understand how the program works.
4. Add your own experiments.

## hello.s


File
:   `hello.s`

Description
:   A small "Hello World" program.

Assembly directives
:   `.data`, `.asciiz` and `.text`

Instructions
:  `li` and  `la`. 

System calls
:   `print_string` and  `exit`. 

## basics.s

File
:   `basics.s`

Description
:   The basics of MIPS assembly.

Assembly directives
:    `.data`,  `.text`,  `.space`,  `.word` and  `.asciiz`.

Instructions
:  `li`,  `la`,  `lw`,  `add`,  `addi`,  `sw` and  `move`.

System calls
:   `print_string`,  `print_int`,  `print_char` and  `exit`.

## jump_and_branches.s

File
:   `jump_and_branches.s`

Description
:   Unconditional and conditional branches.

Implemented control structures
:   `if-then-else` and  `infinite loop`.

Assembly directives
:    `.data`,  `.text`,  and  `.asciiz`.

Instructions
:   `j`, `blt` and `bge`. 
  
  
## arrays.c

File
:   `arrays.s`

Description
:   Allocation and usage of arrays.

Data structures
:   Array of integers and array of strings.

## subroutines.s

File
:   `subroutines.s`

Description
:   A short introduction to the basic use of subroutines. 

Instructions introduced
:  `jal` (Jump And Link) and  `jr` (Jump Register).

Register introduced
: `$ra` (Return Address)

Concepts introduced
:  The register use convention.
