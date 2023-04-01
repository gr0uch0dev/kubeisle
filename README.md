# README

## Intro

Welcome to our GitHub laboratory, where we will cover the fundamentals of Kubernetes and demonstrate how to use some of the most popular tools in the Kubernetes ecosystem. In this laboratory, you will learn how to create a Kubernetes cluster using Kubernetes in Docker (kind), configure the cluster with the required resources, and deploy a simple nginx application using k8s manifests.

We will also show you how to package the nginx application into a Helm chart, which is a package manager for Kubernetes applications. Furthermore, we will demonstrate how to use GitHub to host a private Helm repository, where you can store your charts and deploy them to your Kubernetes cluster.

To manage the deployment of applications and their configurations in Kubernetes, we will introduce ArgoCD, a popular continuous delivery tool for Kubernetes. You will learn how to deploy ArgoCD inside the Kubernetes cluster and use it to synchronize your application manifests and Helm charts with your Kubernetes cluster.

By the end of this laboratory, you will have a better understanding of how to work with Kubernetes and its related tools, and be able to deploy applications on a Kubernetes cluster using best practices.


## Syllabus

[1] Create a cluster using Kubernetes in Docker (`kind`)

[2] Configure the cluster with required resources

[3] Deploy a simple `nginx` application using `k8s` manifests

[4] Package the above `nginx` application into an helm chart

[5] Use `Github` to host a private `Helm` repository

[6] Deploy ArgoCD inside the cluster

[7] Sync ArgoCD with application manifests at point [3]

[8] Sync ArgoCD with helm repository hosted 

## Repository structure

We start from a simple `nginx` application.

Kubernetes manifests are stored  in `kubernetes-nginx`


