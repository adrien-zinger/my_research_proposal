# Title

Design and realisation of a multithreaded unordered expression language for adaptive execution and queueing theory applications.

# Abstract

This research proposes the development of a new programming language primarly based on a runtime with an multithreaded approach to execute expressions. The expressions are fundamentally unordered and the language has a feature of lazy evaluation like in haskell. By enabling runtime adaptition of execution, this language aims t create a flexible and efficient computational envirronment. Furthermore, the dynamic expression ordering naturally forms a plateform for experimental studies in queueing theory, providing opportunities to model and analyze complex systems. This work will contribute both to programming language theory and to the modeling and simulation of queueing networks.

# Introductio

Traditional programming languages enforce a strict ordering of computations.
Some compilers can change the order of some operations, as well, processors with
out-of-order features will be able to compute multiple parts of machine codes in
parrallel. But in any of that cases, the high level code will show a strict
ordering. Functional languages like Haskell introduce laziness to defer
computation but do not primarly target multithreading or environmental
adaptation.

Rust and C++ provides libraries to achieve that work, like Tokio and Executor.
Rust proposes an adaptative way by letting the user redefine the behavior of
async/await and the features are well designed and completely respond to
real-world requirements. In other words, in these languages, you can easilly
design a program that fit with your envirement with functions like `wait-all`,
`then` and `timeout`, but more the project grows, more it became difficult to
analyse and modify the asynchronous behaviors.

There is a growing need for programming paradigms that are inherently
concurrent, adaptable and suitable for modeling real world applications.

This research seeks to design a new language where:

- Every expression is independently executable and possibly evaluated in
  different threads
- Expressions are fundamentally unordered unless explicit constraints
- The execution order can adapt or dynamically at runtime based on resource
  availability, or by a specific configuration directly where the expression is
  coded
- Arrays and some expressions are lazilly evaluated
- We can access to the libc with bindings or directly

This research propose to create a toolchain with an interpreter and a compiler
both coded in Rust. The compiler should be able to use C89 as intermediate
language.

# Research Questions

1. How can a programming language be designed to treat all expressions in any
order without sacrificing correctness and linearizability conditions?
2. What mechanisms can allow the runtime system to reorder expression evaluation
based on the execution context?
3. How can the language's runtime serve as a realistic environment for modeling
and testing queueing systems?
4. How to compare performances with other functional languages?


# Objectives

The goals are to design a programming language with a prototype of a complete
toolchain. With severals libraries that offer network comunication and local
file operations.

- Develop a prototype capable of adaptive execution scheduling.
- Compare performance with benchmark's programs with other traditional
  approaches.
- Apply models to simulate and analyse queueing theory scenarios.
- Evaluate the adaptability of the language.

# Literature Review

Multithreading and concurrency. Studying models such as actors, futures,
corroutines. Looking the Rust Tokio crate reviews about the issues they had and
how they resolved them compared with the latest queueing theory researchs. I.e.
Tokio use to have multiple job queues that are LIFO and implement job stealing,
this is not the first approach of the new language, but it's still to explore.

Language design. Look at Haskell and Rust design to handle errors. How to store
shared variables and identify static immutable memory region. Use primarly
lock-free and wait-free algorithms to minimize the impacts of multithreading.
Understanding the Haskell's approach to deffered computation and how it impacts
the language design, and how in haskell the side effects impacts the developper
and his way to code.

Modern runtime schedulers will be examined for insights and comparaisons.

# Methodology

1. Language specification. Define the syntax and the semantics of the language.
2. Create a scalable interpreter capable to read and execute a program.
3. Design some cases studies and benchmarks involving computation tasks and
   queueing models.
4. Evaluation metrics, parrallel execution efficiency, context switching
   overhead, runtime latency, linearizability and correctness guarantees.


# Expected contribution

- A new language model for unordered multithreaded computation.
- New techniques for using execution environments as experimental platforms for
  queueing theory.
- A language with both interpreter and compiler. With powerfull static analisys.
- Insights to the interplay between program evaluation order and system
  performance.


# Timeline

