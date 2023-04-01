# Create your Kind Cluster

## Kubernetes
Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. It provides a highly scalable and resilient infrastructure for running applications in containers and enables teams to easily deploy and manage containerized applications across different environments.

## Kind
Kind, short for Kubernetes in Docker, is a tool that allows you to run a local Kubernetes cluster inside a Docker container. It provides an easy way to test Kubernetes applications and configurations without having to set up a full-scale production-like cluster. Kind uses the Docker container engine to create a lightweight, single-node Kubernetes cluster, which can be used for development, testing, and experimentation purposes. It also supports multiple node clusters, allowing for more complex testing scenarios. Overall, Kind is a useful tool for developers who want to test and experiment with Kubernetes in a local environment.

## Get Started with Kind

### Installation

Follow this page to install Kind on your Machine:

https://kind.sigs.k8s.io/docs/user/quick-start/#installing-from-release-binaries

### Setup the cluster

In this folder there's a .yaml file indicating the configuration for the cluster. In our example a cluster with three components will be created.

- 1 x Control plane (mandatory)
    - The control plane will also have compatibility for Ingress.
- 2 x Worker nodes


### Create the cluster

Once the file is read, the cluster is ready to be deployed.

`kind create cluster --config kind-config-with-ingress.yaml`

### Install Kubectl to communicate with the Cluster

ref: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/

Kubectl is a command-line tool used for managing Kubernetes clusters. It allows you to deploy, inspect, and manage your applications running on a Kubernetes cluster. Kubectl communicates with the Kubernetes API server to perform various tasks such as creating, deleting, and updating resources like pods, services, and deployments. With kubectl, you can view logs, monitor resource utilization, and troubleshoot issues in your Kubernetes applications. Overall, kubectl is an essential tool for developers and administrators working with Kubernetes.


```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

kubectl version --client
```

### Tips

- Make sure the architecture of the computer matches both `kind` and `kubectl`. To get information about the machine's architecture, issue in the terminal `uname -i`
