# README

## Intro

Kubernetes is one of the most popular container orchestration systems in use today, enabling users to deploy, manage, and scale containerized applications in a highly efficient and scalable way. However, the complexity of Kubernetes can make it difficult for new users to get started and fully utilize all of its features.

This is where the Kubernetes Laboratory comes in. This repository contains a set of exercises and examples that cover various aspects of Kubernetes, from the basics of deployment and configuration to more advanced topics like security and monitoring. The laboratory provides a hands-on experience with Kubernetes, enabling users to learn by doing and gaining a deeper understanding of how Kubernetes works.

The laboratory is designed to be accessible to users of all levels, whether you're just getting started with Kubernetes or you're an experienced user looking to expand your knowledge. Each section of the laboratory provides clear and concise instructions, along with sample code and configuration files, to help you get up and running quickly.

By working through the exercises in this laboratory, you'll gain a solid foundation in Kubernetes and be able to confidently deploy and manage containerized applications in a variety of settings. Whether you're a developer, system administrator, or DevOps engineer, the skills you'll learn in this laboratory will be valuable in your work with Kubernetes and help you advance your career.

## Disclaimer

Please note that this laboratory is developed and tested on a Linux Fedora 6.2.7-200.fc37.x86_64. Therefore, some of the commands and settings may not be compatible with other operating systems such as Mac or Windows.

Furthermore, we have used Kind as the Kubernetes engine, but other engines like Minikube or K3s can also be used. It is important to adapt the commands and configurations to fit your hardware and software setup accordingly.

## Sections

The laboratory is divided into several sections, each of which focuses on a specific aspect of Kubernetes.

### [1] Kubernetes

This section provides an introduction to Kubernetes, including how to set up a Kind cluster on a Linux machine, deploy a simple application using Kubernetes services, and use the Helm package manager. Additionally, this section covers how to implement a load balancer using MetalLB and an Ingress controller using Nginx.

- Setup Kind cluster on a Linux Machine
- Simple application: k8s-nginx
- Package Manager: Helm
- Kubernetes Services
- Load Balancer: MetalLB
- Ingress: Nginx Ingress Controller

### [2] Configuration

In this section, you'll learn how to manage secrets in Kubernetes, including how to dynamically inject secrets into your application and use the Vault secret manager to securely store and manage your secrets.

- Dynamic Secret Injection
- Secret Manager: Vault

### [3] Monitoring (Operational)

Monitoring is a critical aspect of running Kubernetes in production, and this section covers the basics of monitoring with Prometheus and Alert Manager. You'll learn how to set up a Prometheus server, configure metrics scraping, and create alerts based on those metrics.

- Prometheus
- Alert Manager

### [4] Hardening

Security is always a concern when running Kubernetes, and this section covers how to evaluate the attack surface of your Kubernetes setup and harden the configuration accordingly. You'll also learn how to implement Istio service mesh to secure your microservices and secure your images to prevent unauthorized access.

- Evaluate attack surface of current setup and harden the configuration consequently
- Service Mesh: Istio
- Hardening clusters(least privilege) and workloads (secure images)
- Expose your cluster safely over the Internet


### [5] Monitoring (Security)

This section focuses on security monitoring, specifically how to use agents like Filebeat to collect security-related logs from your Kubernetes cluster and forward them to an Elasticsearch cluster. You'll also learn how to evaluate EDR (Endpoint Detection and Response) tools for detecting and responding to security incidents.

- Introduce security logging and monitoring using agents like Filebeat. Forward logs to Elasticsearch cluster
- Evaluate EDR
 
### [6] Routing Logs

Centralized logging is essential for troubleshooting and analyzing application performance, and this section covers how to send all logs produced in your Kubernetes cluster to a single source, specifically Elasticsearch.

- Send all logs produced to a single source (Elasticsearch)

### [7] Alerting

In this section, you'll learn how to define use cases for security alerts in ELK stack covering known attack patterns. This will help you detect and respond to security incidents quickly and effectively.

- Define use cases for security alerts in ELK stack covering known attack patterns

### [8] CI/CD

Continuous Integration and Continuous Delivery (CI/CD) is a critical aspect of modern software development, and this section covers how to implement a proper CI/CD pipeline using Jenkins, including SAST, DAST, and container scanning using open-source tools. You'll also learn how to implement CD using ArgoCD to deploy applications to your Kubernetes cluster.

- CI using Jenkins (build proper pipeline with SAST,DAST,container scanning using opensource tools)
- CD using ArgoCD


### Malware Analysis and Honeypots

This section is currently under development and will cover how to use Kubernetes for malware analysis and honeypots. Stay tuned for updates!

- To be defined



