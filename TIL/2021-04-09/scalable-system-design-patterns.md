# Scalable System Design Patterns

Looking back after 2.5 years since [my previous post on scalable system design techniques](http://horicky.blogspot.com/2008/02/scalable-system-design.html), I've observed an emergence of a set of commonly used design patterns. Here is my attempt to capture and share them.

## Load Balancer

In this model, there is a dispatcher that determines which worker instance will handle the request based on different policies. The application should best be "stateless" so any worker instance can handle the request.

This pattern is deployed in almost every medium to large web site setup.

![](https://1.bp.blogspot.com/_j6mB7TMmJJY/TLnj_mWL50I/AAAAAAAAAgg/JFPsfGcAenI/s400/p1.png)

## Scatter and Gather

In this model, the dispatcher multicast the request to all workers of the pool. Each worker will compute a local result and send it back to the dispatcher, who will consolidate them into a single response and then send back to the client.

This pattern is used in Search engines like Yahoo, Google to handle user's keyword search request ... etc.

![](https://2.bp.blogspot.com/_j6mB7TMmJJY/TLlDyOK60HI/AAAAAAAAAfI/JreI7fqvohA/s400/P2.png)

## Result Cache

In this model, the dispatcher will first lookup if the request has been made before and try to find the previous result to return, in order to save the actual execution.

This pattern is commonly used in large enterprise application. Memcached is a very commonly deployed cache server.

![](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLlEpBawVMI/AAAAAAAAAfQ/Jp8vbVYnF0s/s400/P3.png)

## Shared Space

This model also known as "Blackboard"; all workers monitors information from the shared space and contributes partial knowledge back to the blackboard. The information is continuously enriched until a solution is reached.

This pattern is used in JavaSpace and also commercial product GigaSpace.

![](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlFf-b8lPI/AAAAAAAAAfY/Poy8V0eH1gA/s400/P4.png)

## Pipe and Filter

This model is also known as "Data Flow Programming"; all workers connected by pipes where data is flow across.

[This pattern](http://www.eaipatterns.com/PipesAndFilters.html) is a very common EAI pattern.

![](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLlGIM4IDiI/AAAAAAAAAfg/nQgVADmUl5w/s400/P5.png)

## Map Reduce  
  
The model is targeting batch jobs where disk I/O is the major bottleneck. It use a distributed file system so that disk I/O can be done in parallel.

This pattern is used in many of Google's internal application, as well as implemented in open source [Hadoop](http://hadoop.apache.org/) parallel processing framework. I also find this pattern [can be used in many many application design scenarios](http://horicky.blogspot.com/2010/08/designing-algorithmis-for-map-reduce.html).

![](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlHPyMkTII/AAAAAAAAAf4/McnK_GGkYpw/s400/P7.png)

Bulk Synchronous Parellel

This model is based on lock-step execution across all workers, coordinated by a master. Each worker repeat the following steps until the exit condition is reached, when there is no more active workers.

1.  Each worker read data from input queue  
    
2.  Each worker perform local processing based on the read data
3.  Each worker push local result along its direct connection

This pattern has been used in Google's [Pregel graph processing model](http://horicky.blogspot.com/2010/07/google-pregel-graph-processing.html) as well as the [Apache Hama](http://incubator.apache.org/hama/) project.

![](https://4.bp.blogspot.com/_j6mB7TMmJJY/TLnhYZH7PTI/AAAAAAAAAgY/YHy5K8H6hZA/s400/P8.png)

## Execution Orchestrator

This model is based on an intelligent scheduler / orchestrator to schedule ready-to-run tasks (based on a dependency graph) across a clusters of dumb workers.

This pattern is used in [Microsoft's Dryad project](http://research.microsoft.com/en-us/projects/dryad/)

[![](https://3.bp.blogspot.com/_j6mB7TMmJJY/TLlH_a9WOMI/AAAAAAAAAgI/41l0bvV3fkE/s400/P8.png)

Although I tried to cover the whole set of commonly used design pattern for building large scale system, I am sure I have missed some other important ones. Please drop me a comment and feedback.

Also, there is a whole set of scalability patterns around data tier that I haven't covered here. This include [some very basic patterns underlying NOSQL](http://horicky.blogspot.com/2009/11/nosql-patterns.html). And it worths to [take a deep look at some leading implementations](http://horicky.blogspot.com/2010/10/bigtable-model-with-cassandra-and-hbase.html).