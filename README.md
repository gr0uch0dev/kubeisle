# README

## Prerequisites

Kubernetes cluster up and running

`kubectl` installed and configured to manage the above cluster


## Intro

Following are the objectives of this lab 

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


