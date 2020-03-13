---
title: 'Concurrency'
type: "docs"
weight: 60
---

## Concurrency

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


To achieve concurrent programming, there are two main paradigms used:
- Eventuals
- Cooutines

## Eventuals
Eventuals describe an object that acts as a proxy for a result that is initially unknown, usually because the computation of its value is not yet complete. 

### Async/Await
Async methods are intended to be non-blocking operations. An await expression in an async subprogram doesn’t block the current thread while the awaited task is running. Instead, the expression signs up the rest of the subprogram as a continuation and returns control to the caller of the async subprogram and it means “Once this is done, execute this function”. It’s basically a “when done” hook for your code, and what is happening here is an async subprogram, when executed, returns a coroutine which can then be awaited. This is done usually in one thread, but can be done in multiple threads too, but thread invocations are invisible to the programmer in this case.
```
pro main(): int = {
    doItFast() | async                                             // compiler knows that this subprogram has an await routine, thus continue when await rises
    .echo("dosomething to echo")
                                                                   // the main program does not exit until the await is resolved
}
fun doItFast(): str = {
    result = client.get(address).send() | await                    // this tells the subprogram that it might take time
    .echo(result)
}
```
## Coroutines
A coroutine is a task given form the main thread, similar to a subprogram, that can be in concurrent execution with other tasks of the same program though other routines.  A **worker** takes the task and runs it, concurrently. Each task in a program can be assigned to one or multiple workers. 

Three characteristics of coroutine distinguish them from normal subprograms:
- First, a task may be implicitly started, whereas a subprogram must be explicitly called. 
- Second, when a program unit invokes a task, in some cases it need not wait for the task to complete its execution before continuing its own. 
- Third, when the execution of a task is completed, control may or may not return to the unit that started that execution.
- Fourth and most importantly, the execution of the routine is entirely independent from main thread.

In fol to assign a task to a worker, we use the symbols `[>]` 

### Channels

FOL provides asynchronous channels for communication between threads. Channels allow a unidirectional flow of information between two end-points: the Transmitter and the Receiver. It creates a new asynchronous channel, returning the tx/tx halves. All data sent on the Tx (transmitter) will become available on the Rx (receiver) in the same order as it was sent. The data is sent in a sequence of a specifies type `seq[type]`. `tx` will not block the calling thread while `rx` will block until a message is available.

```
pro main(): int = {
    var channel: chn[str];
    for (0 ... 4) {
        [>]doItFast() | channel[tx]                                             // sending the output of four routines to a channel transmitter
                                                                                // each transmitter at the end sends the close signal
    }
    var fromCh1 = channel[rx][0]                                                // reciveing data from one transmitter, `0`
}

fun doItFast(i: int; found: bol): str = {
    return "hello"
}
```

If we want to use the channel within the function, we have to clone the channel's tx and capture with an ananymus subprogram: Once the channels transmitter goes out of scope, it gets disconnected too.
```
pro main(): int = {
    var channel: chn[str];                                                      // a channel with four buffer transmitters
    var sequence: seq[str];

    for (0 ... 4) {
        [>]fun()[channel[tx]] = {                                               // capturin gthe pipe tx from four coroutines
            for(0 ... 4){
                "hello" | channel[tx]                                           // the result are sent fom withing the funciton eight times
            }
        }                                                                       // when out of scope a signal to close the `tx` is sent
    }

    select(channel as c){
        sequence.push(channel[rx][c])                                           // select statement will check for errors and check which routine is sending data
    }
}
```

### Locks - Mutex

Mutex is a locking mechanism that makes sure only one task can acquire the mutexed varaible at a time and enter the critical section. This task only releases the mutex when it exits the critical section. It is a mutual exclusion object that synchronizes access to a resource. 

In FOL mutexes can be passed only through a subprogram. When declaring a subprogram, instead of using the borrow form with `( // borrowing variable )`, we use double brackets `(( // mutex ))`. When we expect a mutex, then that variable, in turn has two method more: 

- the `lock()` which unwraps the variable from mutex and locks it for writing and 
- the `unlock()` which releases the lock and makes the file avaliable to other tasks
```

fun loadMesh(path: str, ((meshes)): vec[mesh]) = {                              // declaring a *mutex and *atomic reference counter with double "(( //declaration ))"
    var aMesh: mesh = mesh.loadMesh(path) 
    meshes.lock()
    meshes.push(aMesh)                                                          // there is no need to unlock(), FOL automatically drops at the end of funciton
                                                                                // if the function is longer, then we can unlock to not keep other tasks waiting
}

pro main(): int = {
    ~var meshPath: vec[str];
    ~var meshes: vec[mesh];
    var aFile = file.readfile(filepath) || .panic("cant open the file")

    each( line in aFile.line() ) { meshPath.push(line) };
    for(m in meshPath) { [>]loadMesh(m, meshes) };
}
```

