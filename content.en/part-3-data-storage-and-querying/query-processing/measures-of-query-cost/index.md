---
title: 'Measures of Query Cost'
weight: 2
references:
    videos:
        - youtube:rKN60UnVsMw
        - youtube:coqVU1xJfFg
    links:
        - https://www.geeksforgeeks.org/measures-of-query-cost-in-dbms/
        - https://www.javatpoint.com/estimating-query-cost
    books:
        - b1:
            title: Query Processing in Database Systems
            url: https://www.google.co.in/books/edition/Query_Processing_in_Database_Systems/AOGpCAAAQBAJ?hl=en&gbpv=0
        - b2:
           title: Database Systems
            url: https://www.google.co.in/books/edition/Database_Systems/9m382yDgxRsC?hl=en&gbpv=0
---

# Measures of Query Cost

There are multiple possible evaluation plans for a query, and it is important to be able to compare the alternatives in terms of their (estimated) cost, and choose the best plan. To do so, we must estimate the cost of individual operations, and combine them to get the cost of a query evaluation plan. Thus, as we study evaluation algorithms for each operation later in this chapter, we also outline how to estimate the cost of the operation.

The cost of query evaluation can be measured in terms of a number of dif- ferent resources, including disk accesses, CPU time to execute a query, and, in a distributed or parallel database system, the cost of communication (which we discuss later, in Chapters 18 and 19).

In large database systems, the cost to access data from disk is usually the most important cost, since disk accesses are slow compared to in-memory operations. Moreover, CPU speeds have been improving much faster than have disk speeds. Thus, it is likely that the time spent in disk activity will continue to dominate the total time to execute a query. The CPU time taken for a task is harder to estimate since it depends on low-level details of the execution code. Although real-life query optimizers do take CPU costs into account, for simplicity in this book we ignore CPU costs and use only disk-access costs to measure the cost of a query-evaluation plan.

We use the _number of block transfers_ from disk and the _number of disk seeks_ to estimate the cost of a query-evaluation plan. If the disk subsystem takes an average of _tT_ seconds to transfer a block of data, and has an average block-access time (disk seek time plus rotational latency) of _tS_ seconds, then an operation that transfers _b_ blocks and performs _S_ seeks would take _b_ ∗ _tT_ \+ _S_ ∗ _tS_ seconds. The values of _tT_ and _tS_ must be calibrated for the disk system used, but typical values for high-end disks today would be _tS_ \= 4 milliseconds and _tT_ \= 0_._1 milliseconds, assuming a 4-kilobyte block size and a transfer rate of 40 megabytes per second.2

We can refine our cost estimates further by distinguishing block reads from block writes, since block writes are typically about twice as expensive as reads (this is because disk systems read sectors back after they are written to verify that the write was successful). For simplicity, we ignore this detail, and leave it to you to work out more precise cost estimates for various operations.

The cost estimates we give do not include the cost of writing the final result of an operation back to disk. These are taken into account separately where required. The costs of all the algorithms that we consider depend on the size of the buffer in main memory. In the best case, all data can be read into the buffers, and the disk does not need to be accessed again. In the worst case, we assume that the buffer can hold only a few blocks of data—approximately one block per relation. When presenting cost estimates, we generally assume the worst case.

In addition, although we assume that data must be read from disk initially, it is possible that a block that is accessed is already present in the in-memory buffer. Again, for simplicity, we ignore this effect; as a result, the actual disk-access cost during the execution of a plan may be less than the estimated cost.

The **response time** for a query-evaluation plan (that is, the wall-clock time required to execute the plan), assuming no other activity is going on in the computer, would account for all these costs, and could be used as a measure of the cost of the plan. Unfortunately, the response time of a plan is very hard to estimate without actually executing the plan, for the following reasons:

**1\.** The response time depends on the contents of the buffer when the query begins execution; this information is not available when the query is opti- mized, and is hard to account for even if it were available.

**2\.** In a system with multiple disks, the response time depends on how accesses are distributed among disks, which is hard to estimate without detailed knowledge of data layout on disk.

Interestingly, a plan may get a better response time at the cost of extra resource consumption. For example, if a system has multiple disks, a plan _A_ that requires extra disk reads, but performs the reads in parallel across multiple disks may finish faster than another plan _B_ that has fewer disk reads, but from only one disk. However, if many instances of a query using plan _A_ run concurrently, the overall response time may actually be more than if the same instances are executed using plan _B_, since plan _A_ generates more load on the disks.

As a result, instead of trying to minimize the response time, optimizers gen- erally try to minimize the total **resource consumption** of a query plan. Our model of estimating the total disk access time (including seek and data transfer) is an example of such a resource consumption–based model of query cost.

