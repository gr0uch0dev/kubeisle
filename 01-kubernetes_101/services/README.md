# Services in Kubernetes

In Kubernetes, services are used to provide network access to a set of pods. Services define a logical set of pods and policies for accessing them. In this README, three types of services will be treated:

- ClusterIP
- NodePort
- LoadBalancer.

Examples are provided to test the different types of service. Remember that in the LoadBalancer case, you must install a LoadBalancer Provider in your cluster first. See related section. A `netshoot` pod is also provided to test the connectivity from within the cluster.

## ClusterIP
A ClusterIP service exposes the service on a cluster-internal IP address. This type of service is only accessible from within the cluster and is often used for communication between different parts of an application.

When a ClusterIP is defined, two parameters must be defined:
- **port**: representing the port on which the Service will be exposed on.
- **targetPort**: the port on which your application listens to (i.e. your nginx webserver)

With these parameters defined, the service is ready to be contacted from **within** the cluster.

```bash
curl <svc-ip>:<port>
```

## NodePort
A NodePort service exposes the service on a specific port on **each node** in the cluster. The service is accessible using the node's IP address and the NodePort. This type of service is typically used for exposing a service externally to the cluster or for accessing a service from outside the cluster. NodePort is an extension of the ClusterIP type, meaning that it will have the same parameters as the ClusterIP, and an extra one related to its functionality.

When a NodePort is defined, three parameters must be defined:
- **port**: representing the port on which the Service will be exposed on.
- **targetPort**: the port on which your application listens to (i.e. your nginx webserver)
- **nodePort**: the port exposed on each Node of your cluster.

With these parameters defined, the service is ready to be contacted in two ways:

```bash
curl <svc-ip>:<port> # from within the cluster, same as before
curl <node-ip>:<nodePort> # from outside the cluster
```

## LoadBalancer
A LoadBalancer service exposes the service using an external load balancer. This type of service is often used for exposing a service externally to the cluster and is commonly used in cloud environments that provide load balancer services. Commonly but not forcefully, it is possible to install a Load Balancer in a local Kubernetes cluster. (See related section)

When a LoadBalancer is defined, two parameters must be defined:
- **port**: representing the port on which the Service will be exposed on.
- **targetPort**: the port on which your application listens to (i.e. your nginx webserver)
With these parameters defined, the service is ready to be contacted in two ways:

```bash
curl <loadbalancer_ip> # from outside the cluster
curl <svc-ip>:<port> # from within the cluster, same as cluster-ip
```

Implementation used in this lab: MetalLB

# So, what is an Ingress?
Ingress is a Kubernetes resource that allows you to expose HTTP and HTTPS routes from outside the cluster to services within the cluster. It acts as a reverse proxy for incoming traffic and can provide additional features such as TLS termination and URL-based routing. Ingress is typically used in conjunction with a LoadBalancer service to provide external access to services within the cluster. When used together, the LoadBalancer service provides external access to the cluster, and the Ingress routes traffic to specific services within the cluster based on the requested URL.


Implementation used in this lab: NGinx Ingress Controller

To know more about Ingress, see related lab.