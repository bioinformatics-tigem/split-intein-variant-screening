# Learn C by Examples

While studying C, I kept running into the same kind of questions that probably come up for many other students:

- What does this syntax actually mean?
- Why does this work?
- What changes if I write it in a slightly different way?
- What is the compiler really checking here?
- What is happening in memory?
- Why does a tiny example sometimes feel harder than it looks?

At some point, I started wondering what kind of material would have helped me the most while learning these concepts.

This repository is my answer to that question.

I am not trying to write the next big C book here. There are already great books for that. A textbook is still the right place when you need formal definitions, complete language rules, or a rigorous reference.

What I am trying to build is the resource I would have liked to keep open next to the book while studying.

The focus here is less on formal coverage and more on understanding: small programs, variations of the same idea, common mistakes, questions that naturally come up while studying, and examples that can be compiled, changed, broken and debugged.

Sometimes that means spending a lot of time on a very small piece of code. That is intentional. If a line such as

```c
typedef double (*DoubleOperation)(double);
```

looks simple after someone explains it but confusing the first time you meet it, this repository is meant to stay on that line long enough to make it clear.

---

## What this repository is trying to be

This repository is meant to be:

- a study companion;
- a collection of small, compilable examples;
- a place where common doubts are treated as part of learning;
- a portfolio project that shows clean structure, clear code and educational intent.

It is **not** trying to replace a formal C book.

---

## A few core ideas at a glance

### 1. Pointers are about values, addresses and dereferencing

<p align="center">
  <img src="assets/pointer-basics.svg" alt="Pointer basics diagram" width="900">
</p>

A pointer parameter means that the function expects a **pointer value** as input.
That usually means an address, but the important thing is to think in terms of **type contract**:

$$
\texttt{setToZero} : \texttt{int *} \rightarrow \texttt{void}
$$

So this is valid:

```c
int number = 10;
setToZero(&number);
```

and this is valid too:

```c
int *pointer = &number;
setToZero(pointer);
```

because `pointer` already stores an address.

Relevant note: [Pointers](docs/foundations/pointers.md)

### 2. Function pointers are still contracts

<p align="center">
  <img src="assets/function-pointer-contract.svg" alt="Function pointer contract diagram" width="900">
</p>

A declaration such as:

```c
typedef double (*DoubleOperation)(double);
```

can be read as:

$$
\texttt{DoubleOperation} : \texttt{double} \rightarrow \texttt{double}
$$

That makes questions like these much easier to reason about:

```c
operation
*operation
operation(3.0)
(*operation)(3.0)
```

They do not all mean the same thing.
Some represent the function, some call the function, and some produce the returned value.

Relevant note: [Function pointers](function-pointers/README.md)

### 3. Arrays and pointers are related, but they are not the same thing

<p align="center">
  <img src="assets/array-decay.svg" alt="Array decay diagram" width="900">
</p>

In most expressions, an array name decays to a pointer to its first element:

$$
\texttt{values} \longrightarrow \texttt{&values[0]}
$$

and indexing can be understood through pointer arithmetic:

$$
\texttt{values[i]} = \texttt{*(values + i)}
$$

This is why a function such as:

```c
void printFirst(const int *values);
```

can be called with an array:

```c
int values[] = {10, 20, 30};
printFirst(values);
```

Relevant notes:
- [Arrays and array decay](docs/foundations/arrays.md)
- [Pointer arithmetic](docs/foundations/pointer-arithmetic.md)

### 4. Recursion becomes clearer when you see the flow

<p align="center">
  <img src="assets/recursion-flow.svg" alt="Recursion flow diagram" width="900">
</p>

The idea is not just “the function calls itself”.
The important pieces are:

- a **base case**;
- a recursive step that moves toward that base case;
- suspended calls that continue only after the deeper call returns.

For factorial, the mathematical idea is:

$$
\operatorname{factorial}(n) =
\begin{cases}
1 & \text{if } n = 0 \\
 n \cdot \operatorname{factorial}(n-1) & \text{if } n > 0
\end{cases}
$$

Relevant note: [Recursion](recursion/README.md)

---

## How to use this repository

Each topic follows the same idea:

1. read the short explanation;
2. compile and run the smallest example;
3. change something and predict what should happen;
4. enable compiler warnings and read them;
5. move to the larger example only when the smaller one is clear;
6. try the exercises without copying the existing implementation.

The repository is currently centered on function pointers and recursion, with supporting notes on the C concepts and tools needed to understand them well.

---

## Topics

### Foundations

These notes are useful on their own and also work as prerequisites for the larger examples.

| Topic | Why it matters |
| --- | --- |
| [Pointers](docs/foundations/pointers.md) | Values, addresses, dereferencing, pointer parameters |
| [Pointer arithmetic](docs/foundations/pointer-arithmetic.md) | Moving through arrays and understanding indexing |
| [Functions](docs/foundations/functions.md) | Prototypes, calls and contracts |
| [Arrays and array decay](docs/foundations/arrays.md) | Why arrays work the way they do in expressions and parameters |
| [`typedef`](docs/foundations/typedef.md) | Naming complex types clearly |
| [Compile-time, link-time and runtime errors](docs/foundations/errors.md) | Understanding when and why different errors happen |

### C examples

| Module | Focus |
| --- | --- |
| [Function pointers](function-pointers/README.md) | Function-pointer syntax, contracts, arrays of function pointers, `myclass` example |
| [Recursion](recursion/README.md) | Base case, recursive step, call flow, trace examples |
| [Exercises](exercises/README.md) | Practice without hiding behind large programs |

### Tools

| Tool note | Focus |
| --- | --- |
| [Compiling with GCC and Clang](docs/tools/gcc.md) | Basic compilation commands and warning flags |
| [Building with Make](docs/tools/make.md) | Reproducible builds and project structure |
| [Debugging with GDB](docs/tools/gdb.md) | `run`, `break`, `next`, `step`, `continue`, `display`, `jump` |

---

## Suggested learning path

If you are starting from scratch, this order works well:

1. [Pointers](docs/foundations/pointers.md)
2. [Arrays and array decay](docs/foundations/arrays.md)
3. [Pointer arithmetic](docs/foundations/pointer-arithmetic.md)
4. [Functions](docs/foundations/functions.md)
5. [`typedef`](docs/foundations/typedef.md)
6. [Compile-time, link-time and runtime errors](docs/foundations/errors.md)
7. [Function pointers](function-pointers/README.md)
8. [Recursion](recursion/README.md)
9. [GDB](docs/tools/gdb.md)

---

## Before you start

You need a C compiler and Make.

On a Debian or Ubuntu based Linux system:

```bash
sudo apt update
sudo apt install build-essential gdb
```

Check that the tools are available:

```bash
gcc --version
make --version
gdb --version
```

Clang can also be used for the examples:

```bash
make CC=clang
```

---

## Build everything

From the root of the repository:

```bash
make
```

Executables are written to `bin/`.

You can also build only one group:

```bash
make function-pointers
make recursion
```

To remove generated executables:

```bash
make clean
```

If Make is new to you, start here:

[Building with Make](docs/tools/make.md)

The examples are compiled with:

```text
-std=c17 -Wall -Wextra -Wpedantic
```

Warnings are part of the learning process. C can accept code that is legal enough to compile but still suspicious, unsafe or simply not what you intended.

---

## Repository structure

```text
learn-c-educational/
├── README.md
├── assets/
│   ├── pointer-basics.svg
│   ├── function-pointer-contract.svg
│   ├── array-decay.svg
│   └── recursion-flow.svg
├── Makefile
├── docs/
│   ├── foundations/
│   │   ├── pointers.md
│   │   ├── pointer-arithmetic.md
│   │   ├── functions.md
│   │   ├── arrays.md
│   │   ├── typedef.md
│   │   └── errors.md
│   └── tools/
│       ├── gcc.md
│       ├── make.md
│       └── gdb.md
├── function-pointers/
│   ├── README.md
│   ├── 01_basic_pointer.c
│   ├── 02_operation_array.c
│   ├── 03_apply_to_array.c
│   └── myclass/
│       ├── myclass.h
│       ├── myclass.c
│       └── main.c
├── recursion/
│   ├── README.md
│   ├── 01_factorial.c
│   ├── 02_sum_to_n.c
│   ├── 03_trace_recursion.c
│   └── 04_gcd.c
└── exercises/
    └── README.md
```

---

## Why the examples are small

The point is not to hide C behind a framework. The point is to make the language visible.

A small example makes it easier to answer questions such as:

```text
What value is stored here?
What is an address?
What exactly does * mean in this line?
Is the function being referenced or called?
Which call owns this local variable?
What does the compiler know from this function prototype?
```

Once those questions are clear, the same ideas become much easier to recognize in larger projects.

---

## Code quality

The examples intentionally use practices that are worth carrying into larger C projects:

- explicit function prototypes;
- explicit function-pointer signatures;
- `const` for read-only data where useful;
- `size_t` for array lengths;
- checks after dynamic allocation;
- `free` for allocated memory;
- strict compiler warnings;
- a reproducible Makefile;
- CI builds with GCC and Clang;
- examples that can be compiled independently.

This is still a learning repository, so clarity is preferred over cleverness.

---

## Learning philosophy

The best way I have found to understand C is not only to read working code, but also to ask what would happen if one small part changed.

For example:

```c
operation
*operation
operation(3.0)
(*operation)(3.0)
```

These expressions are close enough to look almost interchangeable when function pointers are new, but they do not all represent the same thing.

The same approach is used throughout the repository: start from a small difference, understand what C means by it, and then build the larger concept from there.

---

## Next topics

Good candidates for future modules are:

- scope, `extern` and `static`;
- strings;
- dynamic memory;
- structures in more depth;
- files;
- command-line arguments;
- processes and system calls.

The goal is to keep the same format: explanation, minimal example, larger example, common questions and exercises.
