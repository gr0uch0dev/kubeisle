# [5] Use Github to host a private Helm repository

From Helm official documentation 

```
A chart repository is an HTTP server that houses an index.yaml file and optionally some packaged charts. When you're ready to share your charts, the preferred way to do so is by uploading them to a chart repository.
```

In the previous lab stages you have created an `index.yaml` inside the `helm-artifacts` folder.

We can use the `raw.githubusercontent` (`https://raw.githubusercontent.com`) to act as a webserver for our content hosted in `helm-artifacts`.

For instance when pushed to Github you can get the content of `index.yaml` reaching an endpoint like the following

https://raw.githubusercontent.com/gr0uch0dev/k8s-helm-argocd/master/helm-artifacts/index.yaml??token=<TOKEN_PROVIDED_BY_GITHUB>

Where `<TOKEN_PROVIDED_BY_GITHUB>` is attached by Github when you click on `Raw` to get the raw content of the file hosted on Github.

With this you can install the `helm` package by referencing the content on Github.

Generate a personal access token on Github. This is needed to get the contents from the private repository where you host the helm packages.

```
Github User info -> Settings -> Developer Settings -> Personal Access Tokens (classic) -> Generate token by allowing only all the entries under the 'repo' section
```

To check that you are able to install the `helm` package also from Github, remove the one previously installed locally

```
helm uninstall helm-nginx-lab
```

Check the `service` resources under `helm-nginx` namespace

```
kubectl get svc -n helm-nginx
```

Make sure that the ones created before is no longer there (`No resources found in helm-nginx namespace.`)

Install the `helm` repo from Github

```sh
helm repo add myorg --username <GITHUB_USERNAME> --password <GITHUB_PERSONAL_ACCESS_TOKEN> https://raw.githubusercontent.com/gr0uch0dev/k8s-helm-argocd/master/helm-artifacts/

```

Update the `helm` repos

```
helm repo update
```

Search for the charts in the repo

```
helm search repo
```

Expect an output similar to the following

```
NAME                           	CHART VERSION	APP VERSION	DESCRIPTION                         
myorg/helm-nginx-laboratory    	0.0.1        	0.0.1      	Nginx built from Helm for Laboratory
```

Install the `helm` chart from this repository

```
helm install helm-nginx-from-github myorg/helm-nginx-laboratory
```

Check that the service is running

```
$ kubectl get svc -n helm-nginx
NAME            TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
nginx-service   LoadBalancer   10.96.98.60   172.18.255.201   8080:30063/TCP   34s

```
