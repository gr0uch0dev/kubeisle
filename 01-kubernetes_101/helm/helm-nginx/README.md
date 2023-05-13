# [4] Package simple `nginx` application into an helm chart

## What is Helm?
Helm is a tool to package, deploy, and manage Kubernetes applications. Helm helps you manage Kubernetes applications — Helm Charts help you define, install, and upgrade even the most complex Kubernetes application.

### What is a package manager?
A package manager is a tool that simplifies the process of installing, updating, and removing packages. A package is a preconfigured group of resources that are deployed as a unit. Helm is a package manager for Kubernetes.

### Why do I need Helm?
Helm is useful for installing and managing Kubernetes applications. If you have a complex application, Helm can make it simple to:
- Roll out updates
- Rollback to previous versions
- Share applications with others
- Make easy the separation of environments (dev, test, prod)

## Introduction

The idea is to now package inside an `helm` Chart what has been created in the [k8s-nginx](../k8s-nginx/README.md) lab.

With `helm` we can abstract the `k8s` manifests and packaging them into a single artifact that takes the name of `Chart`.

It is clear that following the lab linked earlier is required for a better understanding of the following steps.

## Installing Helm

Helm is distributed as a single binary file. Download the latest release and unpack it.

```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh
```
Check that the package has been correctly installed prompting `helm version` .

## Getting started

The current folder structure is

```
.
├── Chart.yaml
├── templates
│   ├── configmap.yaml
│   ├── deployment.yaml
│   └── svc.yaml
└── values.yaml
```

The `Chart.yaml` file contains the metadata of the `Chart` and the `templates` folder contains the `k8s` manifests that will be deployed in the cluster.

Bear in mind that inside the templates file, instead of hard-coding the values, we are using the `values.yaml` file to pass the values to the `k8s` manifests. This is a good practice to follow when creating `helm` charts because it allows to override the values at the time of the deployment.

Let's take as an example the file `templates/svc.yaml` and see how the values are references inside the `values.yaml` file.

The `templates/svc.yaml` looks like this:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.services.nginxService.name }}
  namespace: {{ .Values.metadata.namespace }}
spec:
  type: LoadBalancer
  ports:
    - port: 8080
      targetPort: 80
  selector:
    app: nginx
```

As it is possible to see, the values for the name and the namespace are taken from the `values.yaml` file and they are not directly hard-coded.

Giving a look to the `values.yaml` file:

```yaml
metadata:
  name: "nginx-deployment"
  namespace: "helm-nginx"

services:
  nginxService:
    name: "nginx-service"
```

Before going into further details, let's create a namespace in which the `helm` chart will be deployed.

### Creation of the Namespace

Create the `helm-nginx` namespace to hold the resource that will be created by `helm`

```
kubectl create namespace helm-nginx
```

### Installation of the Chart
Now, to install the helm chart run the following command:

``` bash
helm install helm-nginx-lab .
```

The file `values.yaml` it is not required to be passed to the `helm install` command because it is already present in the `helm` chart. Although, if the installation is part of a CI/CD pipeline, it is possible to override the values of the `values.yaml` file by passing a different file with the `--values` flag.

This is an example of the power of `helm` because it allows to have a single `helm` chart that can be deployed in different environments (dev, test, prod) by overriding the values of the `values.yaml` file.

### Check the installation
Check that the `helm` package is installed running `helm list`

```
$ helm list
NAME          	NAMESPACE	REVISION	UPDATED                               	STATUS  	CHART                      	APP VERSION
helm-nginx-lab	default  	1       	2023-03-26 15:17:25.13609936 -0400 EDT	deployed	helm-nginx-laboratory-0.0.1	0.0.1      
```

Get the IP address of the NodePort service that is running in `helm-nginx` namespace

```
kubectl get svc -n helm-nginx
```

The service should appear here.

Get the IP of the node using kubectl
    
    ```
    kubectl get nodes -o wide
    ```

Get the IP address of the Node from here and open a terminal on your computer. Prompt:

```bash
curl <IP_ADDRESS>:30080
```

and you should see the `nginx` welcome page.

Congratulations! You have just deployed your first `helm` chart.