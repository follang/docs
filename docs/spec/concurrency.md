---
title: 'Concurrency'
type: "docs"
weight: 60
---

Concurrency is the ability of different tasks of a program to be executed out-of-order or in partial order, without affecting the final outcome. This allows for parallel execution of the concurrent tasks, which can significantly improve overall speed of the execution in multi-processor and multi-core systems. In more technical terms, concurrency refers to the decomposability property of a program into order-independent or partially-ordered tasks.

There are two distinct categories of concurrent task control. 

- The most natural category of concurrency is that in which, assuming that more than one processor is available, several program tasks from the same program literally execute simultaneously. This is physical concurrency - **parallel programming**. 
- Or programm can assume that there are multiple processors providing actual concurrency, when in fact the actual execution of programs is taking place in interleaved fashion on a single processor. This is logical concurrency **concurrent programming**. 

From the programmer’s points of view, concurrency is the same as parallelism. It is the language’s task, using the capabilities of the underlying operating system, to map the logical concurrency to the host hardware.
 
There are at least four different reasons to use concurrency:
- The first reason is the speed of execution of programs on machines with multiple processors.
- The second reason is that even when a machine has just one processor, a program written to use concurrent execution can be faster.
- The third reason is that concurrency provides a different method of conceptualizing program solutions to problems.
- The fourth reason for using concurrency is to program applications that are distributed over several machines, either locally or network.

## Tasks
A task is a unit of a program, similar to a subprogram, that can be in concurrent execution with other units of the same program. Each task in a program can support one thread of control. 

Three characteristics of tasks distinguish them from subprograms. 
- First, a task may be implicitly started, whereas a subprogram must be explicitly called. 
- Second, when a program unit invokes a task, in some cases it need not wait for the task to complete its execution before continuing its own. 
- Third, when the execution of a task is completed, control may or may not return to the unit that started that execution.

```
>>> declaration[options] name(arguments)[captures]: returntype[options] = { implementation; };
```

## Channels
```
pro main(): int = {

    var []something: str = { non };

    >>> fun doItFast()[[]something]: int[64] = {
        
    }
}
```
