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

#### Amazon SNS and Amazon SQS
Amazon products based on Pub/Sub (Queue/Topic) pattern for async processing providing `Loosely coupled architecture`.

**Amazon Simple Queue Service [SQS]**
Basically a QUEUE instance hosted my AWS
- Send/Store/Receive messages (containing payload) between pieces of software at any volume

**Amazon Simple Notification Service [SNS]**
Basically a TOPIC instance hosted by AWS
- Pub/Sub approach where a Publisher publish to a Topic and N subscribers can read the same message from that topic.

---

### Amazon Serverless Solutions

Serverless: You can't see or access the underlying infrastructure that are hosting your app (so you also don't have to manage, like patching, etc) 

**AWS Lambda**: Where you can `upload your code` in a "Lambda Function", `configure a Trigger`, the server `waits for the Trigger`, when the trigger is Met, the code is run in a env you should not worry about it.
- AWS Lambda scale your Lambda functions according to your workload input.
- Are designed to `run code < 15 minutes`, so it does't work for heavy proccess like Deep Learning, is suitable for things like back end services handling requests for example. 

If you want a middle term between serverless, you can choose containers options (Docker Container), where you can deploy your app in a container and "manage the container configs".

- **Amazon Elastic Container Service (ECS)**: Help to run containerized appls at scale without need to manage your container orchestration software.
- **Amazon Elastic Kubernetes Service (EKS)**: Similar thing but with different tooling and features.

They can both run on top of EC2 (your own instance) but you need a way/tool to manage containers (container orchestration), if you don't want to do that, you can use `AWS Fargate` is a serveless compute plataform for ECS/EKS.

### Summary of when use each of AWS solutions:
- **Amazon EC2**: Traditional applications and want full access to the OS
- AWS Lambdas: short running functions; event driven appls, DON'T want to manage server at all.  
- **Amazon ECS** or **Amazon EKS**: Run Docker container based workloads on AWS, then choose your plataform
  - EC2: That you manage
  - AWS Fargate: Serveless environment that is managed for you

---

### High availability and fault tolerance

AWS has global infra structure, meaning that your app can run across multiple locations, it provides HA and FaultTolerance in case of disaster in a specific place/context.

#### Region
Regions are places where AWS put multiple data centers to serve your business needs, like São Paulo have a bunch of servers, you can decide to deploy your App there so Latency between São Paulo customers and your App are going to be low.

Regions are separated, meaning that one region DOES NOT send/share data with other regions, unless you grant this permission, this is useful for security complience, like government ones for example.
Some business requirements to choose your app region:
- Compliance: Like Data must live in a specific place.
- Proximity: How close your app is from your main costumers (latency)
- Feature Availability: Some regions may not have all AWS features (may AWS did not install the hardware in there yet)
- Pricing: Some regions are more expansive from others, Brazil is much more expansive than US for example.

#### Zones
Group of Data Center or One Data center is called `Zone` (or AZ), and Region is consisted in N `Zones` that are phisically isolated. The Zones are not next to each other, this to try to prevent disasters. AWS recomends running at least 2 availability zones in a Region.

AWS provides "Regional Services" solutions, like the ELB (Elastic Load Balancer), that run across multiple regions to direct the workload balance between in your instances that may be running multiple across regions/zones.

---

#### CDN
CDN (Content Delivery Network) is basically a local copy of the central server that you place closer to the customer, like, if your App is far way from a country that has many customers, your place a CDN close to that country to have lower latency. This copy can be resources, files, etc.
**Amazon has a CDN service** called `Amazon CloudFront`

#### Amazon CloudFront
Helps deliver data, app, videos with low-latency, uses `Edge Locations`, it's like Edge Places where the copy of your content is placed to be closer to some places and reduce latency. You can push your content to a specific list of edge locations depending on your needs.
Example: Your App runs on Brazil, but the Customer is from Tokyo, Instead of requiring your customers to get their data from Brazil, you can cache a copy locally at an edge location that is close to your customers in China.
![Edge Loc](AmazonEdgeLocationEx.png)

Edge locations runs CDNs and also other services like DNS services like `Amazon Route 53`.
We also have `AWS Outposts`, they basically install their service into your own building to run AWS Infrastucture in your Data Center (Extend AWS infrastructure and services to your on-premises data center) 

---

#### How to interact with these AWS Resources?
Basically through API Calls (Application Programming interfaces), you can create Lambda Functions, EC2 Instances.
To interact with the AWS Services:

**AWS Management Console**: Browser based and can manage the AWS Resources visually, good for:
- Test environments
- View AWS bills
- View monitoring
- Work with non-technical resources
![Edge Loc](AWSManagemenConsole.png)


**AWS Command Line Interface (CLI)**: Make API calls using the terminal on your PC. Like this you can create scripts to perform specific actions interacting with AWS services and automate this.

**AWS Software Development Kits (SDKs)**: Interact with AWS resources using varios programming languages (like a Lib/JAR)

**AWS Elastic Beanstalk**: Your basically tell this service what's your App code and desired config (how many instances, load balancer configs, etc), and it will manage it for you. This config is saved and can be replicable easilly in the future, the ideia of this solution is to focus on your business and let the services take care of the Infra Structure for you.

**AWS Cloud Formation**: Infrastructure as code tool used to define AWS resources to be deployed, you define this in a JSON or YML file listing the resources you want, so you have a template that can be reused to deploy that resources everytime you need. `It helps you define WHAT you want but not worring about HOW`. It supports a bunch of AWS services like: Storage, Analytics, ML, etc.

---

#### Amazon Virtual Private Cloud (Amazon VPC)
Lets you create a logically isolated section on the AWS Cloud where you can put AWS resource in networks we define. They can be public (internet access) or private (not internet). The public and private groupoing of resources are called sub-nets they are ranges of IP addresses in our VPC.