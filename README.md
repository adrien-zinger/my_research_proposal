# Title

An inherently concurrent and multi-threaded language with adaptive scheduling runtime
and queueing theories applications.

# Abstract

This research proposal compiles many design ideas of a general-purpose
programming language. Some thematic/features presented in this document
may be a bit terse and not logically connected
to each other at the first shot. But making a real-world usable, and an efficient
programming language with an innovative paradigm isn't that easy, and many of the ideas
remains to be proved.

So the main purpose of the research is to create a language which have inherently
concurrent and multi-threading mechanisms. Even after reading books about compilers and interpreters,
*The dragon book*, *Crafting Interpreters*, *Introduction to Compilers and Language Design*, writing
an innovative language with a different paradigm is daunting. Trying to make a efficient and
solid language is also very challenging in many parts, making that kind of project very productive.


This research primarily design Eniem (name subject to change) based on a runtime with a
concurrent and optionally multi-threaded approach. In Eniem classics expressions
are fundamentally unordered. It includes lazy evaluation of everything, and still go
forward in the execution.

Where many other languages provide only tools and libraries to obtain
asynchronous, the subject of that research natively implement secured memory
sharing, multi-threading, job scheduling and light concurrency.

That new language aims to be flexible, easy to learn, and contains
powerful features, providing opportunities to model and analyze complex
systems. That multi-threaded paradigm enable runtime adaptation of execution, job
class specifications, that can be used both in a real-world environment for
any developments requiring parallelism/concurrency, and in a study environment to
experiment some networks in queueing theory.

With that innovating paradigm, Eniem could enable runtime adaptation of the execution.
It includes job (expression) class specifications, dynamic reclassification. In addition,
Eniem toolchain provides a powerful type checking and security features thanks to
an abstract interpreter that can be enabled which makes the code safe and correct.

# Project on GitHub Right Now

A [first attempt of the prototype](https://github.com/adrien-zinger/eniem-langage) is already
in development and accessible to public. The GitHub project presentation (README, contributing,
documentation) are missing at this time. But some example of code demonstrate a first idea of
the syntax of the language.

# Pre introduction

Developing a simple programming language is already
a challenge if you enable some features like recursions, dynamic arrays, type checking,
static analysis, memory safety. It open that much opportunities of studying unexplored theories
and trying new ideas. Then, starting to make all the program concurrent will insert another
layer of difficulty: how is it concurrent, what the runtime looks like, is it really
efficient and how to implement the memory management, the mutability, et cetera.
There is a lot of subject of research in one project to present, so lets dump here, before the
real introduction, a simple list of references that inspired this document.

- [A dependently Typed Assembly Language](https://www.cs.cmu.edu/~rwh/papers/dtal/OGI-CSE-99-008.pdf)
- [You should make a new terrible programming language](https://ntietz.com/blog/you-should-make-a-new-terrible-programming-language/)
- [An Experimental Integration of io_uring and Tokio: An Asynchronous Runtime for Rust](https://www.researchgate.net/publication/363947992_An_Experimental_Integration_of_io_uring_and_Tokio_An_Asynchronous_Runtime_for_Rust)
- Rust Atomics and Locks: Low-Level Concurrency in Practice, by Mara Bos
- [A History of Haskell: being lazy with class](https://www.researchgate.net/publication/221501761_A_history_of_Haskell_Being_lazy_with_class)
- [Lazy Functional State Threads](https://www.researchgate.net/publication/2295326_Lazy_Functional_State_Threads)
- [Compiling with Abstract Interpretation](https://www.normalesup.org/~dlesbre/files/publications/2024-pldi-compiling-with-abstract-interpretation.pdf)

# Introduction

In traditional programming language, the more recurrent paradigm is a strict
ordering of expression evaluation. For instance, this is what the front-end of C and C++
gives. The output generated by such programs reflects what the developer require.
In some other functional programming languages, like in Rust and Haskell, there is
much to tell because they introduced both parallelism with lazy evaluations.
But none primarily target multi-threading or environmental adaptation as the
proposed language here.

It has been shown that making evaluation unordered in a program can be very
powerful. In many context in high level, the development of a system tends to use threads,
coroutines, futures, promises, etc. At low levels, compilers, like GCC aims to reorder some
expressions to make generated machine code faster based on the build environment.
At a lower level, an out-of-order processor is able to evaluate expressions of the same
program in multiple cores if no fences are detected.

The implementation of a language where parallel evaluation
is the paradigm makes sens, nevertheless the obvious weakness is that it can be difficult
for the developer to understand what his code actually do.

That's why the language design has to be easy to use to make development of complex system possible. Generally a
code is interpreted sequentially, they are several reason for that and one of them is that developers
thinks sequentially (most of them). If Haskell is known as a hard to learn language because of his features of concurrency and lazy
evaluation and so on, in a very wildly used language like JavaScript, concurrency is very used
in many context by programmers, beginners or masters. This is because in JavaScript, the
concurrency is mono-threaded and managed by a virtual
machine which makes accesses to the memory safe, and error handling understandable.
In that kind of context, a developer can built a very complex systems of states machines
with a large scale of jobs.

Mono-threading like in JavaScript can be considered as a weakness respecting to other languages. But JSC has
demonstrated that a simple concurrency can be very efficient with a good implementation of memory management.

> - [Comparative Study of Refactoring Haskell and Erlang Programs](https://www.researchgate.net/publication/220703707_Comparative_Study_of_Refactoring_Haskell_and_Erlang_Programs)
> - [Kyle Simpson- You Don't Know JS, Async and Performance](https://drive.google.com/file/d/1q-FkIvx6oBEcZ-pli_jkFFPr6B7d7VBA/view)
> - [Self Reference - Binding JSC in Rust](https://github.com/adrien-zinger/rusty_jsc)

There is a growing need for programming paradigms to have one that inherently implement
concurrency and adaptable modeling. Rust and C++ both provides libraries which are
by definition limited
by what Rust and C++ provides to create a program. It is well known that bigger a system is,
harder it became to modify it in a real-world context. Many projects suffer of big refactoring because
of the dependency of a library.

Note: Rust proposal of defining async/await behaviour is not enough and is today controversial.

> - [The Rust I wanted had no future](https://news.ycombinator.com/item?id=36193326)
> - [Experimental async / await support for Tokio](https://tokio.rs/blog/2018-08-async-await)

While multi-threading programming is becoming a wildly used approach to design a system, due
to the observed performance and the global ideology spreed by developer's influencers. There
is still open issues, partially resolved for fitting within specific usecase. The opportunity to
use previous mistakes as lesson is very interesting in this research. In real-world applications,
promises implementation shown to be quite difficult, and category theory demonstrate another
controversial way which finally is considered better than before. For other example, Tokio crate faced at
stuttering and big latency issues, which were patched but still very rigid.

> - [CLR thread pool injection, stuttering problems](https://joeduffyblog.com/2006/07/08/clr-thread-pool-injection-stuttering-problems/)
> - [Jon Gjengset - Decrusting the tokio crate](https://www.youtube.com/watch?v=o2ob8zkeq2s)
> - [Category Theory Promise/A+](https://brianmckenna.org/blog/category_theory_promisesaplus)
> - [Reducing tail latencies with automatic cooperative task yielding](https://tokio.rs/blog/2020-04-preemption)

To conclude, this research aims to design a new language with a complete toolchain (static analyser, interpreter
and at term a transpiler into C89) where every expression are fundamentally unordered jobs, written like if it were
sequential, with lazy evaluation and inherent parallelism.

# Research Questions

1. How to design a language to treat all expressions in any order without sacrificing correctness
   and linearizability conditions?

The language should be able to recognize pure and impure function to minimize the use of mutexes. Early
in the code analysis, the toolchain must recognize static, immutable and mutable variables to mitigate
the access overhead to a shared memory between threads. There is a pist to explore were the language
would accept mutability only in some defined arenas (which remains to be studied), which can theoretically make the implementation
of the memory management easier.

> - [Implementation Strategies for Mutable Value Semantics](https://www.jot.fm/issues/issue_2022_02/article2.pdf)
> - [Pure vs Impure functions](https://dev.to/sanspanic/pure-vs-impure-functions-50aj)
> - [Simple, Fast, and Practical Non-Blocking and Blocking Concurrent Queue Algorithms](https://www.cs.rochester.edu/~scott/papers/1996_PODC_queues.pdf)
> - [Rayon data parallelism](https://smallcultfollowing.com/babysteps/blog/2015/12/18/rayon-data-parallelism-in-rust/)

2. What mechanisms can allow the runtime system to reorder expression evaluation
based on the execution context?

Said that the language in this research aims to be used in real-world and study environment, the runtime
has be able to dynamically reorder expression evaluation. The development of a program with Eniem must be handful to tell
that a job has a higher/less priority than another. Most of the needs here have to be defined after studying
the state of art in queueing theory and asynchronous programming.

> - [Making the Tokio scheduler 10x faster](https://tokio.rs/blog/2019-10-scheduler)

3. How can the language's runtime serve as a realistic environment for modeling an efficient and handful system?

In first part of the design, multi-threading and concurrent systems require such mechanisms
like `wait_all`, `then`, `timeout` to be implemented. In the other hand, the toolchain must
contains handful tools to compare performances by slightly modifying job classes, runtime
configurations and jobs implementations. The code have to not be broken at each refactoring
of the global behavior of the system. Finally, insights about program's performances are often hard
to get when a project scale and inherent functionalities are big requirements in real-world applications
and in study cases.

> - [A Product-form Network for Systems with Job Stealing Policies](https://dl.acm.org/doi/10.1145/3643845)
> - [The Gittins Policy is Nearly Optimal in the M/G/k under Extremely General Conditions](https://www.cs.cmu.edu/~harchol/Papers/Sigmetrics21a.pdf)

# Objectives

The goals are:

1. To design a complete prototype of a language and his toolchain with some
libraries as network communication, input and outputs, parsing, etc. Directly binded with
libc.

2. Develop benchmark's programs with the designed language and other traditional
   approach (C, C++, Haskell, Rust)

3. After an evaluation of the adaptability/flexibility of the language.
   Apply different models of queueing theory, analyse and compare scenarios.

> - [No-op compiler benchmarking](https://scot.tg/2025/01/07/no-op-compiler-benchmarking/)

# Methodology

1. Language specification. Define the syntax and the semantics of the language.
2. Create a scalable interpreter capable to read and execute a program.
3. Design some cases studies and benchmarks involving computation tasks and
   queueing models.
4. Evaluation metrics, parallel execution efficiency, context switching
   overhead, runtime latency, linearizability and correctness guarantees.


# Contributions to computer science

- A new language that primarily use multi-threading, lazy evaluation and unordered computation.
- New techniques for experimental platforms in queueing theory.
- New techniques for real-world scalable applications development.
- Complete toolchain with type checking, abstract interpretation, interpreter and compiler.
- Implementation of benchmarks of queueing networks with job stealing (multiple jobs queues),
  single queue with/out jobs reordering.
- Insights to the interplay between program evaluation order and system performance.

# Timeline

| Phases                        | Duration (Months) |
|-------------------------------|-------------------|
| Literature Review & Design    | 0–6               |
| Prototype Development         | 7–18              |
| Experimental Design & Testing | 19–30             |
| Analysis                      | 31–36             |
| Dissertation Writing          | 37–42             |
| Defense Preparation           | 43–48             |

