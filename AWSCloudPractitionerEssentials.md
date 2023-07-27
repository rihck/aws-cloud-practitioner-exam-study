## AWS Cloud Practitioner Essentials Notes

#### Amazon EC2
**Amazon EC2** is a virtualized (Virtual Machine) server inside a phisical amazon service that is shared across multiple applications (multi-tenancy). We can compare the EC2 with a Phisical Server from the real World, that you request and create with the capacity you want.
- You configure the OS you want to use (Linux or IOS)
- You decide what do install there: Internal Applications, Web Servers, Databases, etc.
- EC2 Instances are resizable, you can change the capacity/resources of the server when you want. (vertical scaling)
- You control the networking aspect: type of request allowed to your server, public or private access, etc.
- Amazon EC2 is `CaaS (Compute as a Service)` - You don't need to worry about the phisical server, you just need to worry about the resources you need to run your application.

**Types of EC2 Instances**
Each type has different combination of resources:

- General purpose: (_Balance between CPU, Memory and Networking_) for: Webservers / Compute repositories
- Compute optimized (_Compute intensive tasks_): Gaming servers; High performance computing (HPC); Scientific modeling
- Memory optimized: (Memory intensive tasks): proccess large things in memory in a fast way
- Accelerated computing (_Utilize hardware accelerators_): Floating point number calc, Graphics processing; Data pattern matching
- Storage optimized: High performance for locally stored data

**EC2 Instances Prices**
Ordering: More expansive to less expansive 
- On-Demand: Only pay per duration.
- Sacings Plans: Offers lows price in trade of a commiment to a consistent usage
- Reserved Instances: When you commit for this instance in 1 or 3 year commitment. 
- Spot instances: Can be 90% cheaper than OnDemand BUT the catch here is that AWS can request this instance back anytime, it notifies you 2 minutes before turning the instance down. This type is ideal for proccess that can be interrupted like batches 
- Dedicated Hosts: Basically a phisical server only for you, suitable when you have law requiriments of not sharing the same hardware with other companies/proccess, so in this case it's not a multi-tenancy approach like the default AWS strategy.

#### Scalability
When working on-premise, you have to predict the high work loads and buy a hardware accordingly BUT it can be a waste of money if you don't use most of the time. Your worse, you buy a hardware to handle the average workload and you're not ready for peaks.
![Resources on Demand](resourcesOnDemand.png)

AWS/Cloud is different because you can scape up and down whenever you need.
![Resources on Cloud](resourcesOnCloud.png)

Types of scalling:
- Up (Vertical): Add more resource to the instance
- Out (Horizontal): Add more instances

Amazon has auto-scale solution `Amazon EC2 Auto Scaling to an application`, where you scale up to your demand. In this solution you can configure
- Minimum: Number of instances AWS is going to deploy once you start the app
- Desired: The number of number that is desired (if not provided will use the Minium as desired)
- Maximum: The max instances you want to run, the limit.

---

#### Elastic Load Balancing (ELB)
To handle this situation where you have multiple instances BUT you can balance (load) the load between them.
![Elastic Load Balancer](multipleInstancesWithoutLoadBalancer.png)

Your App can have its own Load Balancer BUT AWS has this Load Balancer solution called `Elastic Load Balancer` where it does it automatically for you.
![Elastic Load Balancer](multipleInstancesWithoutLoadBalancer.png)

The ELB also scales up with no costs if we have more incoming traffic. It can be used for external (client class) and internal traffic (services talking to each other)

---

### Amazon SNS and Amazon SQS
Amazon products based on Pub/Sub (Queue/Topic) pattern for async processing providing `Loosely coupled architecture`.



