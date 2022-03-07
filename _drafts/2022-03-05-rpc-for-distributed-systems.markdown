---
layout: post
title:  "Application Specific Load Balancing for Distributed Systems with RPC"
date:   2022-03-05 14:01:45 -0700
categories: Programming
---


Distributed Systems design and maintenance has become both a highly sought after skill by employers as well
as much more prevalent in the skillset of the average engineer. Technologies such as [Kubernetes](https://kubernetes.io) 
allow software teams to manage and distribute the load of their applications. The most common implementation of this utilizes
the Kubernetes Deployment and Service, in a configuration as such: 

![Figure 1](/images/rpc-for-distributed-systems-figure1.drawio.png)
<br/>
<small>Figure 1 - A Traditional Load Balanced Application</small>
# Load Balancing

In Kubernetes, there is already a `LoadBalancer` Service built in. Load Balancing creates a layer of availability and scalability
that does not exist in single host operation. There are several different ways to distribute incoming requests but there is 
some nuance in how the service determines *balance*. The most traditional approaches are as follows:


**Fastest Response** - The Load Balancer will send the incoming request to whichever host is currently offering the fastest 
response time. 

**Fewest Servers** - Instead of distributing requests across all hosts uniformly, a *Fewest Server* approach can be taken that 
will load a single server to it's maximum before redistributing requests to the next server. This option is great when an 
application is running in a cloud environment that might incur costs as a result of multiple machines active.

**Least Connections** - The Least Connections approach considers how many active connections there are to each host in the 
pool. As hosts gain and lose connections, the incoming requests are evenly distributed across all hosts in the pool. 

**Least Load** - A Least Load approach is one of the most common ways of distributing incoming requests. The Load Balancer 
simply evaluates and forwards the request to the host that is consuming the least amount of resources (CPU and RAM)

# Application Specific Approaches

Although the Kubernetes Load Balancer is relatively functional out of the box, there quickly becomes some glaring issues 
when a more advanced stack is used. For example, note that in Figure 1, the individual application instances running in 
the traditional load balancing configuration can only communicate with the LoadBalancer itself and not with the other 
instances of the application. The Load Balancer serves as a single point of IO for the application pool and does not 
allow cross communication.

Furthermore, the Kubernetes Load Balancer does not allow any additional logic to be performed other than the evaluation 
described in the above load balancing methods. For example, there might be another metric that is far better suited for effective
balancing. Consider a job that has periodic bursts of high compute, then lies dormant the remainder of the time: 

![Figure 3](/images/rpc-for-distributed-systems-figure-3.png)
<br />
<small>Figure 2 - Periodic Job Runs</small>

Imagine we have a load that can be modeled as such, this is very common. In this case, there isn't a constant load that might
be seen in something like a web server, which receives and responds to incoming requests and a more uniform way at scale. If 
we add a second host to mix, and attempt to load balance these requests, we can run into something like this: 


![Figure 4](/images/rpc-for-distributed-systems-figure-4.png)
<br />
<small>Figure 3 - Multi-Host Job Runs with Traditional Load Balancer</small>

To break this down, the request comes in *after* Node 1 has completed the job assigned to it, the request is received by a Kubernetes
LoadBalancer which assesses the load on both Node 1 and Node 2. The LoadBalancer will see:

```
Node1 -> CPU (0%) 

Node2 -> CPU (50%)
```

and will assign the job to Node1. This will now cause Node1 to be operating at 80% while Node2 continues to operate at 50%. If this
continues to happen, it's possible that Node1 can get assigned more jobs than it has compute to complete in the specified time budget.

How can we solve this? 

# RPC

RPC (Remote Procedure Call) is typically seen as an alternative to [REST](https://en.wikipedia.org/wiki/Representational_state_transfer)
(Representational State Transfer). RPC is an *action oriented* protocol between two remote hosts, while REST is a *resource oriented*
protocol. REST design architectures are often seen in CRUD API's, while RPC architectures are typically used in environments where
high performance and reliability is needed. 

To solve the above issue, we can create a custom `LoadBalancer` service that will accurately depict the realized load on each node
using RPC. Now, it is possible to build the same software using a REST architecture, but the RPC/Kubernetes integration seems
like it was made for each other. 

# Implementation of RPC Principles

RPC Services are defined using a Protocol Buffer, typically written in Google's language, Proto3. Proto3 is a statically typed
language with the sole purpose of building and distributing protocols. The Protobuff to solve the aforementioned problem can 
look something like this: 

```
syntax = "proto3";

message LoadRequest {}

message LoadResponse {
  float computations_per_minute = 1; 
}

service LoadRouter {
    rpc PollLoad(LoadRequest) returns (LoadResponse) {}
}
```

To break this down, we are defining two messages, `LoadRequest`, and `LoadResponse`. *Messages* are simply a specification of what 
a valid TCP packet looks like that can be associated with a `Router`. A Router, is an RPC entity that models a pipe. It allows data
to be sent.

As per this protobuff, this allows a `LoadRequest` message that takes no parameters to a node. In response, a node will return a
`LoadResponse` that contains a float describing how many computations per minute that is being performed. This communication process
is documented in the LoadRouter by the function `PollLoad`. 

