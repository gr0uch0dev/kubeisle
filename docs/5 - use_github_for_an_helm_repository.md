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





