## Package the chart via Chart Repository

Now that the Chart configuration has been tested you can package it using `helm package`

## Why do I need to package a helm chart?
Packaging a helm chart is a way to distribute the chart to other users. The chart can be distributed via a chart repository or via a tarball. A package is distributed inside a Helm Repository. a Helm repository is a web server that is able to serve the `index.yaml` and the chart tarballs (it will be covered in the next lab).

## How to package a helm chart?

To package a helm chart you need to run `helm package` from the directory that contains the `Chart.yaml` file. In our case the directory is `helm-nginx`. The command to run is:

``` bash
helm package ../helm-nginx
```

A file with the name `helm-nginx-laboratory-0.0.1.tgz` will be created in the current directory. The name of the file is composed by the name of the chart, the version of the chart and the extension `.tgz`

## Install the artifact

The generated artifact can be installed using `helm install` command. The command to run is:

``` bash
helm install helm-nginx-lab ../helm-nginx-laboratory-0.0.1.tgz
```

As you can see, in this case you do not need to worry about any details of the chart. You just need to know the name of the chart and the version of the chart. The chart will be installed in the `default` namespace. Clearly, you can override the namespace and the values file as well. Let's see an example:

``` bash
helm install helm-nginx-lab ../helm-nginx-laboratory-0.0.1.tgz --namespace helm-nginx --values ../helm-nginx/values.yaml
```

Congratulations! You have just installed a helm chart from a tarball. In the next lab an example of creation of a helm repository will be shown.