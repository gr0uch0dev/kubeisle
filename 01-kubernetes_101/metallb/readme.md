# What is MetalLB

ref: https://metallb.universe.tf/installation/
ref: https://metallb.universe.tf/configuration/

MetalLB is a load-balancer implementation for bare metal Kubernetes clusters, allowing you to utilize network load-balancers in environments where external load-balancers are not available or cost-prohibitive. In essence, MetalLB is designed to enable Kubernetes services to be exposed externally using standard load-balancing protocols, such as BGP, ARP, and IGMP.

Kubernetes is a powerful container orchestration tool that can run containerized applications across a cluster of computers. However, it does not include a native load-balancer for distributing network traffic to the applications running on the cluster. This is where MetalLB comes in - it provides a scalable and flexible load-balancing solution that can be easily integrated with Kubernetes.

MetalLB can be installed as a Kubernetes add-on, allowing it to be seamlessly integrated into your existing Kubernetes infrastructure. Once installed, it provides a layer of abstraction that allows Kubernetes services to be accessed using virtual IP addresses (VIPs), which can then be load-balanced across the nodes in the cluster.

Overall, MetalLB is a powerful tool that can greatly enhance the functionality and performance of Kubernetes clusters, particularly in bare metal environments. With MetalLB, you can easily expose your Kubernetes services to the outside world, without the need for expensive external load-balancers.

## MetalLB and Kind

Installing MetalLB using the manifest is a straightforward process that involves deploying a set of Kubernetes manifests to your cluster. These manifests define the MetalLB components, such as the controller and speaker, and specify the configuration parameters, such as the IP address pool.

One of the advantages of using MetalLB is that it works well with Kind (Kubernetes in Docker), which is a popular tool for running Kubernetes clusters locally for development and testing purposes. With Kind, you can easily create a multi-node Kubernetes cluster using Docker containers, and then install MetalLB using the manifest.

To install MetalLB on a Kind cluster, you would simply download the MetalLB manifest files and apply them to your cluster using the kubectl command-line tool. Once installed, you can configure MetalLB to use a specific IP address range for load-balancing, and then create Kubernetes services that use this range.

Install the MetalLB using `kubectl apply -f https://raw.githubusercontent.com/metallb/metallb/v0.13.9/config/manifests/metallb-native.yaml`

## Configure the IP address pool

Get the IP address of our kind node’s Docker container first by running:

`docker container inspect kind-control-plane --format '{{ .NetworkSettings.Networks.kind.IPAddress }}'`

In order to assign an IP to the services, MetalLB must be instructed to do so via the IPAddressPool CR.

All the IPs allocated via IPAddressPools contribute to the pool of IPs that MetalLB uses to assign IPs to services.

``` yaml
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: ipaddress-pool
  namespace: metallb-system
spec:
  addresses:
  - 172.18.0.10-172.18.0.50
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: l2advertisement
  namespace: metallb-system

```

Replace the range of addresses with one matching within the network of your `kind` bridge.

Multiple instances of IPAddressPools can co-exist and addresses can be defined by CIDR, by range, and both IPV4 and IPV6 addresses can be assigned.

### Example Pod

The files `pod.yaml` and `service.yaml` are useful to test the configuration. Apply their manifest and then explore the newly created service.

`kubectl get svc`

you should see under `external-ip` the IP address of the Service that you can hit using `curl`

`curl {external-ip-service}`

And you should get a 200 Response.