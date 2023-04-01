# Ingress on Kind

## Prerequisite

The configuration in the `kind` lesson must be used.

## Introduction
In the world of Kubernetes, the Nginx Ingress Controller is a popular tool for routing external traffic to your Kubernetes services. It acts as a gateway for incoming requests, allowing you to control traffic flow and apply routing rules based on the request's attributes.

In this article, we will guide you through the process of setting up and configuring an Nginx Ingress Controller on a Kubernetes in Docker (Kind) cluster. We will explain the basic concepts of Kubernetes Ingress, dive into the installation process of the Nginx Ingress Controller, and explore some advanced configuration options.

By the end of this article, you will have a working Nginx Ingress Controller that you can use to route external traffic to your Kubernetes services running on the Kind cluster. You will also gain a better understanding of how to configure and customize your Nginx Ingress Controller to meet your specific needs.

## Install the Ingress Controller

Let's start by deploying the Nginx Ingress Controller on the Kind cluster. This command downloads the necessary deployment YAML file from the official Nginx Ingress Controller repository on GitHub and applies it to the cluster. This command ensures that the Nginx Ingress Controller is running and ready to handle incoming requests.

`kubectl apply --filename https://raw.githubusercontent.com/kubernetes/ingress-nginx/master/deploy/static/provider/kind/deploy.yaml` 

The next command waits for the Nginx Ingress Controller pod to be ready. It monitors the status of the pod in the ingress-nginx namespace and waits for it to be ready, indicating that the Nginx Ingress Controller is fully deployed and functional.

`kubectl wait --namespace ingress-nginx --for=condition=ready pod --selector=app.kubernetes.io/component=controller --timeout=90s` 

Once the Nginx Ingress Controller is deployed and ready, we can use the `ingress.yaml` file to create an Ingress resource. This file defines a simple Ingress resource that routes incoming requests to a Kubernetes service called "my-ingress". The "my-ingress" service is running a pod that serves an HTTP response on port 80. The host in the Ingress resource is set to "myingress.localingress.com".

Finally, we have the `pod.yaml` file, which defines a simple pod running an Nginx container. The container listens on port 80 and serves a default Nginx welcome page.


Overall, these commands and YAML files provide a simple example of how to deploy an Nginx Ingress Controller and create an Ingress resource to route external traffic to a Kubernetes service.

## Modify /etc/hosts to be able to connect to the Ingress

Get the IP address of our kind node’s Docker container first by running:

`docker container inspect kind-control-plane --format '{{ .NetworkSettings.Networks.kind.IPAddress }}'`

Then add an entry to `/etc/hosts` with the IP address found that looks like:

172.18.0.2 myingress.localingress.com
Finally, we can curl myingress.localingress.com:

`curl myingress.localingress.com`

## If the system doesn't allow to mess with the /etc/hosts

then a docker container will be used as a bastion-host to do the job.

`docker run --add-host myingress.dustinspecker.com:172.18.0.2 --net kind --rm curlimages/curl:7.71.0 myingress.dustinspecker.com`