# k8s-nginx

Release a simple nginx web server on kubernetes with a custom webpage

## Objective

- Acquire familiarity with Service of type NodePort
- Acquire familiarity with a ConfigMap
- Acquire familiarirty with a Deployment

## Getting started

In this guide we will release a simple nginx web server on Kubernetes that has a custom webpage (customized using a ConfigMap). The server will be exposed to the outside world using a Service of type NodePort.

### First things first: create a namespace
A namespace in Kubernetes is a virtual cluster inside the cluster. It is used to isolate resources and to organize them.
To create a namespace, create a file called namespace.yaml and paste the following content:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: k8s-nginx
  labels:
    name: k8s-nginx
```

Now apply the namespace:

```bash
kubectl apply -f namespace.yaml
```

You can list the namespaces

### Configmap: the index.html page
A configmap is deployed inside a namespace and is mounted as a volume in the nginx container. The index.html file is used as the index page of the web server.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: index-html-configmap
  namespace: k8s-nginx
data:
  index.html: |
    <html>
    <h1>Welcome to our simple Lab!</h1>
    </html
```

Let's apply the ConfigMap:
    
```bash
kubectl apply -f configmap.yaml
```

### Service: the NodePort service

Now we will deploy a service of type NodePort that will expose the nginx web server to the outside world. The service will be deployed in the same namespace as the nginx web server.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: k8s-nginx
spec:
  type: NodePort
  ports:
    - port: 8080
      targetPort: 80
      nodePort: 30080
  selector:
    app: nginx
```

```bash
kubectl apply -f svc.yaml
```

This service will be exposed on port 30080 of the worker node. The service will forward the traffic to port 80 of the nginx web server. The type of the service is NodePort. A NodePort service is a kind of service that exposes the service on a port of the worker node. The service is accessible from outside the cluster using the IP address of the worker node and the port of the service. The service will forward the traffic to the pods that have the label app: nginx.

In short:
- The port 8080 of the service is the port of the service inside the cluster (basically the ClusterIp port).
- The port 80 of the service is the port of the nginx web server.
- The port 30080 of the service is the port of the service outside the cluster.

To know more about the types of services available, see the lesson on services.

### Deployment: the nginx web server

Now we will deploy the nginx web server. The nginx web server will be deployed in the same namespace as the service and the configmap.

The deployment is a piece of software more interesting than the others two and fairly (in this case) more complex.

The deployment uses the selector to select the pods that will be managed by the deployment. It will manage the pods that have the label app: nginx and create 3 replicas of the nginx web server.

In a deployment you specify (under the containers section) the containers that will be deployed. In this case we have only one container (nginx) that will be deployed. Under the container the mounting point of the previously created configmap is specified. The configmap is mounted in the /usr/share/nginx/html/ directory of the nginx container. This is the directory where the index.html file is located. The index.html file is the index page of the web server.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: k8s-nginx-deployment
  namespace: k8s-nginx
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.22.1
        ports:
        - containerPort: 80
        volumeMounts:
        - name: nginx-index-file
          mountPath: /usr/share/nginx/html/
      volumes:
      - name: nginx-index-file
        configMap:
          name: index-html-configmap
```

Once again, let's apply the deployment:

```bash
kubectl apply -f deployment.yaml
```

### Testing the setup and observe the state of the components

Check that the Configmap is created correctly by prompting:
    
    ```bash
    kubectl get configmap -n k8s-nginx
    ```

If all is good, you should see the following output:

    ```bash
    NAME                   DATA   AGE
    index-html-configmap   1      2m
    ```

Now let's check if the Service has been correctly created:
    
        ```bash
        kubectl get svc -n k8s-nginx
        ```

If all is good, you should see the following output:

    ```bash
    NAME            TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
    nginx-service   NodePort
    ```

And finally, let's check if the Deployment has been correctly created:
    
        ```bash
        kubectl get deployment -n k8s-nginx
        ```

If all is good, you should see the following output:

    ```bash
    NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
    k8s-nginx-deployment   3/3     3            3           2m
    ```

Now let's test the setup. To do so, we need to know the IP address of the worker node. To know the IP address of the worker node, prompt:

```bash
kubectl get nodes -o wide
```
Get the IP address from here and open a terminal on your computer. Prompt:

```bash
curl <IP_ADDRESS>:30080
```

If you get
    
        ```bash
        <html>
        <h1>Welcome to our simple Lab!</h1>
        </html
        ```
    
    then everything is working as expected.

Congratulations! You have successfully released a simple nginx web server on Kubernetes with a custom webpage.
 