# API Gateways and Service Mesh

# Introduction

If you are very familiar with the Microservices Architecture space, you would have heard about `Service Mesh` and `API Gateways`. These two words have caused a lot of confusion and left unanswered questions in the mind of readers. 

- Can I run a service mesh and API gateway together? 
- Do service mesh and API gateways perform the same tasks? 
- When can I use a Service Mesh or API Gateway
- How do Service Mesh and API Gateways overlap?

In this article, you will learn what a Service Mesh and API Gateway is, When to use them and how they overlap

# What is Service Mesh 

A service mesh is a technology that is used for controlling and managing service-service communication in a cloud-native and distributed environment. They are usually tasked with handling the networking logic needed for microservices to communicate securely in a Kubernetes Cluster.

A Service Mesh is made up of two components
- **The Control Plane**: The Control plane is referred to as the brain of the Service mesh, it interacts with the proxies to ensure service discovery and observability
- **The Date Plane**: They are made up of proxies that sit next to containers in a pod handling traffic between services

![Service Mesh Architecture](https://imgur.com/6yRz1t6.png)

# Service Mesh Features

Some features offered by Service Mesh includes Observability, security, Networking,  traffic management

- **Metrics and Observability**:  Service Mesh supports distributed request tracing. This allows users to monitor the request, failure rates, request, and response latencies. It also provides observability of service behavior, giving the service operators the power to troubleshoot their applications. When your applications are deployed via a service mesh, you automatically collect service and request metrics which provides more information about the currently running application

- **Security**: Service Mesh ensures the communication between services in the cluster is secure. It allows a mutual TLS to be established between services. This ensures data is exchanged over a secure channel

- **Traffic Management**: Service Mesh allows you to shadow or mirror traffic to a particular environment. This can be useful for testing or canary releases and rollouts

There are several options for service mesh in the market today, some of them includes: 
- [Istio](https://istio.io/)
- [Linkerd](https://linkerd.io/)
- [Consul Connect](https://www.consul.io/docs/connect)
- [Kuma](https://kuma.io/)

# What is API Gateway

An API gateway is a vital component in any microservice architecture. It serves as an entry point into your cluster. It's usually tasked with receiving requests from clients and forwarding them to the respective microservice in the cluster. You can also add authentication and monitoring to the API gateway, which gives more insight into the requests and responses flowing from and to your applications.  
![API Gateway](https://i0ba83ftsgi2rzkek1hlusq1-wpengine.netdna-ssl.com/wp-content/uploads/2018/04/API-Gateway-Diagram.jpg)

# API Gateway Features
There are several benefits of using an API Gateway
- **Abstraction**: API gateways help you create an abstraction layer over your microservices. Instead of having to route traffic to each service directly from the client, all the traffic can now go through a single  interface, creating a uniform experience for the clients
- **Monitoring**:  With API gateways, you can also monitor requests and responses from clients. These data can be useful for the operations or support team. 
- **Traffic control**: API Gateways also allows you to control the traffic flowing from and to your cluster. This can prevent your services from being overwhelmed with lots of requests

There are popular examples of API Gateway
- [Zuul](https://github.com/Netflix/zuul)
- [Apigee](https://cloud.google.com/apigee)
- [Nginx API Gateway](https://www.nginx.com/solutions/api-management-gateway/)
- [Kong API Gateway](https://konghq.com/kong/)

# When to Use a Service Mesh

- When you need to ensure a secure service-service communication within a Kubernetes cluster with monitoring and observability, you can consider using a Service Mesh
- It is best to also adopt a Service mesh when you are building applications using a Micro-service architecture that is designed to run in a Kubernetes cluster
- When you need to capture the request and responses in the form of metrics and distributed tracing within a Kubernetes cluster, service mesh can be a good option

# When to Use API Gateways

- When you need a single point of entry into your cluster as opposed to having multiple entries. This can unify the experience of the client and reduce attacks from hackers
- When you want to be able to provide API lifecycle management for developers
- When you need to monitor the requests and responses flowing from and into your cluster

# How Service Mesh and API Gateways Overlap
There are several areas where Service Mesh and API Gateways Overlap

- **Telemetry collection**: API gateways and Service Mesh allows the collection of data in the form of metrics between services.
- **Load balancing**: You can add a load balancer to API gateways to balance the load from the client. Most Service Mesh automatically provides load balancing for HTTP, gRPC, WebSocket, and TCP traffic
- **Traffic shadowing**: Both API gateways and Service Mesh supports traffic mirroring, i.e. They allow you to direct traffic to a specific destination based on certain requirements. They can be useful for testing and Canary releases
- **Rate limiting**: It's also possible to limit requests between services to reduce your applications from being overwhelmed with API or service calls.
- **Authentication**: Service Mesh provides authentication by ensuring a mutual TLS is established between services. API gateways also allow you to perform authentication to ensure the request is coming from the right source

# Conclusion
Service Mesh and API gateways are vital components of a Microservice Architecture. They are built to achieve distinct as well as similar functionalities. As seen above, there are several areas where service mesh and API gateways overlap. While both of them have their benefits, understanding when to use them is very important. 

In this article, you have learned what a Service Mesh and API gateway are, When to use them, and how they overlap. You can learn more about API gateways and Service Mesh [here](https://konghq.com/blog/the-difference-between-api-gateways-and-service-mesh/)

Originally published on [Syntropy blog](https://blog.syntropynet.com/post/api-gateways-and-service-mesh/)