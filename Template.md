**FOLLOW THIS TEMPLATE BY COPYING IT INTO YOUR OWN PAGE**

Up to five students can be part of a team that contributes to a page.
List the UNI and names of your team members, and what each person contributed in sufficient detail that the staff can identify your contributions.

* jm5530 Jacob Million
  * High level overview and examples
* mc5672 Michael Carrion
  * Alternatives & Why Modin

## Modin

### The Problem and Solution

* Explain the problem that it solves.
* How does the technology solve the problem?
* How it relates to concpts from 4111.  
* **NOTE: illustrating the relationship with concepts from this class IS BY FAR THE MOST IMPORTANT ASPECT of this extra credit assignment**

Remember, the more detailed and thorough you contrast the technology with the topics in class the better.
We've discussed the relational model, constraints, SQL features, transactions, query execution and optimization, and recovery.   Pick a subset of topics to really dive deep and compare and contrast the similarities and differences between our discussion in class and the technology.

---
## Alternatives & Why Modin
* What are the alternatives, and what are the pros and cons of the technology compared with alternatives?  (What makes it unique?)
### **Alternatives –**

1. **Standard/Vanilla Pandas**

2. **Dask DataFrame (DaskDF):**  
   DaskDF extends pandas by partitioning large DataFrames into smaller pandas DataFrames and scheduling their execution across multiple cores/distributed systems. Using lazy evaluation, DaskDF constructs a task graph that executes when triggered, optimizing task dependencies and resource usage. DaskDF is part of the broader Dask ecosystem and integrates well with other Dask tools (such as Dask Distributed, Dask-ML, or Dask-Kubernetes). 

3. **Koalas:**  
   Koalas offers a pandas-like API on top of Apache Spark, allowing users to leverage Spark’s distributed computing capabilities without having to learn Spark’s native APIs. It bridges the gap between pandas, which is optimized for single-node execution, and Spark, which excels at processing large datasets across distributed clusters. Koalas also uses lazy evaluation to optimize and defer computations until explicitly executed.


### Why Modin? Pros and Cons –

1. **Partitioning and Parallelization** 


   *Modin:*  
   Modin supports both row- and column-oriented partitioning. Further, Modin can reshape the partitioning as necessary for the corresponding operation, based on whether the operation is row-parallel, column-parallel, or cell-parallel (independently applied to each unit cell). This approach enables Modin to support both row-parallel and column-parallel operations efficiently, such as transpose, median, and quantile.  


   *DaskDF and Koalas:*  
   Both DaskDF and Koalas partition DataFrames row-wise. Conceptually, the DataFrame is broken down into horizontal partitions along rows, where each partition is independently processed if possible. (NB: This approach is analogous to relational databases!) This strategy works well for operations like row-based filters or simple transformations but is less efficient for column-based computations such as statistical aggregations.  

2. **API Coverage**


   *Modin:*  
   Modin supports over 90% of the pandas API, making it one of the most comprehensive scalable pandas alternatives. This high coverage ensures that existing pandas workflows can run on Modin with minimal changes.  


   *DaskDF and Koalas:*  
   Both DaskDF and Koalas support approximately 55% of the pandas API. Specifically, they do not implement certain functionality that would deviate from the row-wise partitioning approach. For example, DaskDF doesn’t support `iloc`, `median`, `quantile`, or column-wise operations such as `apply(axis=0)`.  

3. **Execution Semantics**  


   *Modin:*  
   Modin employs eager execution, where operations are executed immediately upon invocation. This mirrors pandas’ behavior, making it intuitive for users already familiar with pandas (and requires no code modifications unlike DaskDF and Koalas).  


   *DaskDF and Koalas:*  
   Both DaskDF and Koalas use lazy evaluation, deferring computation until explicitly triggered (i.e., by calling `.compute()` in DaskDF or `.to_pandas()` in Koalas). Lazy evaluation enables optimization of task graphs or query plans but may introduce additional latency for execution. Further, this places a lot of optimization responsibility on the user, forcing them to think about when it would be useful to inspect the intermediate results or to delay doing so.  

4. **Ordering Semantics**  


   *Modin:*  
   By default, pandas preserves the order of the DataFrame, so users can expect a consistent, ordered view as they operate on their DataFrame. Modin preserves pandas’ ordering semantics, ensuring consistency in indexing and result order. This makes it easier to adopt Modin for workflows that rely on the precise ordering of rows or columns.  


   *DaskDF and Koalas:*  
   Neither DaskDF nor Koalas guarantee the preservation of row order in a DataFrame. For example, DaskDF sorts the index for optimization purposes, which results in occasional deviations from pandas’ strict ordering semantics. Further, DaskDF does not support multi-indexing or sorting (unlike Modin, which supports both of these capabilities).  

5. **Compatibility with Computational Frameworks**  


   *Modin:*  
   Modin is designed to run on a variety of systems and to support a variety of APIs. Currently, Modin supports running on both Dask and Ray (a separate framework designed for building scalable/distributed applications). Additionally, Modin is continually expanding to support other popular data processing APIs (such as SQL).  


   *DaskDF and Koalas:*  
   DaskDF is inherently tied to the Dask framework, and Koalas is built on Apache Spark. Unfortunately, they cannot be ported to other computational frameworks.  

6. **Performance**


   On operations supported by all systems, Modin provides substantial speedups. Further, Modin provides substantial speedups even on operators not supported by DaskDF and Koalas. Thanks to its flexible partitioning schemes, Modin provides benefits on  a variety of operations, such as joins.  


### Sources
1. [Modin Documentation](https://modin.readthedocs.io/en/latest/getting_started/why_modin/modin_vs_dask_vs_koalas.html#)  
2. [Dask Documentation](https://docs.dask.org/en/stable/user-interfaces.html)  
3. [Koalas Documentation](https://koalas.readthedocs.io/en/latest/)  
---

### Tutorial

**Note: Installation is less relevant than a tutorial highlighting the main concepts as they relate to 4111.**

#### Example

Construct and describe a compelling example for the technology that is motivated by one member's project 1 application.  The example and tutorial must be original.

While working on Project 1, reading in data was something that needed to be considered. While our team eventually opted to use generated data, the question of efficient data intake remains. The Python Pandas library allows for user friendly data work. Modin spoofs this library, providing the same functionality at a faster rate. While in class we discussed optimization techniques such as projection pushdown, being careful about ourer loop vs inner loop joins, and different, optimized queries, Modin takes another approach to improve overall runtime. It works by parallelizing operations, allowing multiple operations to run simultaniously. This is useful, especially when reading in large CSV files, as one might do for Project 1. Below is a tutorial of how Modin is used, and how simple it is to switch from Pandas to this library.

#### Tutorial

Here is a short tutorial that both shows the speedup from Pandas to Modin, in the context of loading in disease data for Project 1:
https://colab.research.google.com/drive/1Cc2hDk2_dAtDbLcVuFmhcIiaFU8KpTog?usp=sharing
