## Kubernetes Resource requests and Limits

## Introduction
The advent of technologies for orchestrating and managing containers have become seamless with the help of Kubernetes. While its ok to spin up several instances or replica's of a pod, it is also very important to know how much resources each container in a pod uses and how these containers consume resources in response to load and traffic. Having a mechanism in place to manage resources like CPU, Disk resources and Memory is of utmost importance. Kubernetes provides a way to do that and its called Resources requests and limits

> Requests are what the container is guaranteed to get. If a container requests a resource, Kubernetes will only schedule it on a node that can give it that resource

> Limits, on the other hand, make sure a container never goes above a certain value. The container is only allowed to go up to the limit, and then it is restricted. 

It means that a request is the amount of that resources that the system will guarantee for the container, and Kubernetes will use this value to decide on which node to place the pod while a limit is the maximum amount of resources that Kubernetes will allow the container to use

## Why Should you specify resource requests and limits?
- Managing Node Resources
- It's possible for containers to use more resources than they should because of load and traffic if the node has enough resources
- Helps the scheduler to know which node to place the pods
- Assists kubelets in enforcing resource requests and limits and makes sure they dont use more than what has been assigned

You can manage resources requests on a container level or namespace level

## Container Level
Let's look at a sample pod definition file specifying resource requests and limits on the container level

> By default Kubernetes set a limit of 1vcpu and 512Mi on containers if you don't specify any

```
apiVersion: v1
kind: Pod
metadata:
  name: frontend
spec:
  containers:
  - name: db
    image: mysql
    env:
    - name: MYSQL_ROOT_PASSWORD
      value: "password"
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
  - name: wp
    image: wordpress
    resources:
      requests:
        memory: "64Mi"
        cpu: "250m"
      limits:
        memory: "128Mi"
        cpu: "500m"
``` 
In the pod definition above we have a multi-container pod, the second container is usually referred to as a side-car container or helper container. Each container has a resource limit and request specified. 

### CPU

Limits and requests for CPU resources are measured in CPU units. One CPU, in Kubernetes, is equivalent to 1 vCPU/Core for cloud providers and 1 hyperthread on bare-metal Intel processors. 
They are defined in millicores. If your container needs two full cores to run, you would put the value “2000m”. If your container only needs ¼ of a core, you would put a value of “250m”.

If you specify a CPU request larger than that of your biggest node, then the pod will not be scheduled. 

Unless your app is specifically designed to take advantage of multiple cores, It is usually a best practice to keep the CPU request at ‘1’ or below and run more replicas to scale it out. This gives the system more flexibility and reliability.

When a container is using more than the limits set for the pod then Kubernetes throttles the container. This means the CPU will be artificially restricted, giving your app potentially worse performance! However, it won’t be terminated or evicted. You can use a liveness health check to make sure performance has not been impacted.

### Memory
Memory resources are defined in bytes.

Just like CPU, if you put in a memory request that is larger than the amount of memory on your nodes, the pod will never be scheduled.

Unlike CPU resources, memory cannot be compressed. Because there is no way to throttle memory usage if a container goes past its memory limit it will be terminated.

## Namespace Level

 > Namespaces are a way to divide cluster resources between multiple users (via resource quota). See them as virtual clusters 

Its possible to forget to set resources requests and limit on the container level, setting them on the Namespace level is very crucial as it prevents containers from taking more than their share of the cluster resources. To prevent these scenarios, you can set up ResourceQuotas and LimitRanges at the Namespace level.

After creating a namespace, you can specify ResourceQuotas and LimitRanges to lock them down

To create a namespace, run

```
kubectl create namespace mem-example
``` 
#### ResourceQuota 
A resource quota, defined by a ResourceQuota object, provides constraints that limit aggregate resource consumption per namespace. It can limit the quantity of objects that can be created in a namespace by type, as well as the total amount of compute resources that may be consumed by resources in that project.

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: mem-cpu-demo
spec:
  hard:
    requests.cpu: "1"
    requests.memory: 1Gi
    limits.cpu: "2"
    limits.memory: 2Gi
``` 

Looking at this example, you can see there are four sections. Configuring each of these sections is optional.

**requests.cpu** is the maximum combined CPU requests in millicores for all the containers in the Namespace

**requests.memory** is the maximum combined Memory requests for all the containers in the Namespace. 

**limits.cpu** is the maximum combined CPU limits for all the containers in the Namespace. It’s just like requests.cpu but for the limit.

**limits.memory** is the maximum combined Memory limits for all containers in the Namespace. It’s just like requests.memory but for the limit.

If you are using a production and development Namespace (in contrast to a Namespace per team or service), a common pattern is to put no quota on the production Namespace and strict quotas on the development Namespace. This allows production to take all the resources it needs in case of a spike in traffic.

#### LimitRange
By default, containers run with unbounded compute resources on a Kubernetes cluster. With resource quotas, cluster administrators can restrict resource consumption and creation on a namespace basis. Within a namespace, a Pod or Container can consume as much CPU and memory as defined by the namespace's resource quota. There is a concern that one Pod or Container could monopolize all available resources. A LimitRange is a policy to constrain resource allocations (to Pods or Containers) in a namespace.


```
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 100Mi
      cpu: 600m
    defaultRequest:
      memory: 50Mi
      cpu: 100m
    max:
       memory: 256Mi
       cpu: 1000m
    min:
       memory: 10Mi
       cpu: 10m
    type: Container
``` 
The **default** section sets up the default limits for a container in a pod. If you set these values in the limitRange, any containers that don’t explicitly set these themselves will get assigned the default values.

The **defaultRequest** section sets up the default requests for a container in a pod. If you set these values in the limitRange, any containers that don’t explicitly set these themselves will get assigned the default values.

The **max** section will set up the maximum limits that a container in a Pod can set. The default section cannot be higher than this value. Likewise, limits set on a container cannot be higher than this value. It is important to note that if this value is set and the default section is not, any containers that don’t explicitly set these values themselves will get assigned the max values as the limit.

The **min** section sets up the minimum Requests that a container in a Pod can set. The defaultRequest section cannot be lower than this value. Likewise, requests set on a container cannot be lower than this value either. It is important to note that if this value is set and the defaultRequest section is not, the min value becomes the defaultRequest value too.

## Conclusion
While your Kubernetes cluster might work fine without setting resource requests and limits, you will start running into stability issues as your teams and projects grow. Adding requests and limits to your Pods and Namespaces only takes a little extra effort, and can save you from running into many headaches down the line!