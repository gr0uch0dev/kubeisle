# Helm Repository

## What is a Helm Repository?

A Helm repository is a web server that is able to serve the `index.yaml` and the chart tarballs. The `index.yaml` file contains the list of the charts that are available in the repository. The chart tarballs are the packages that can be installed using `helm install` command.

A Helm repository is useful in the context of a CI/CD pipeline. The CI/CD pipeline can build the chart and push it to the Helm repository. The Helm repository can be used by the CD part of the pipeline to install the chart in the target environment. In a company environment the Helm repository can be used to distribute the charts centrally and be used by different teams.

## Using Github Pages as Helm Repository

It is possible to use the `raw.githubusercontent` (`https://raw.githubusercontent.com`) to act as a webserver.

`https://raw.githubusercontent.com/gr0uch0dev/{REPO}/master/helm-artifacts/index.yaml??token=<TOKEN_PROVIDED_BY_GITHUB>`

This is how it is possible to use Github Pages as Helm repository. The only requirement is to have a `index.html` file and the chart tarballs in the repository. The `index.html` file can be created using `helm repo index` command. The tarball used in this example is the one created in the previous lab.

Let's create and push the `index.html` file to the repository.

```bash
helm repo index .
```

Once the file has been created, the `index.html` and the tarball can be pushed to the repository, which of course should have been created for this purpose.

```bash
git add .
git commit -m "Add helm repository"
git push
```

Once setup, the repository can added and used to install the chart.

### Generate the access token
Generate a personal access token on Github. This is needed to get the contents from the private repository where you host the helm packages.

```
Github User info -> Settings -> Developer Settings -> Personal Access Tokens (classic) -> Generate token by allowing only all the entries under the 'repo' section
```

## Add the repository and install the chart

To add the repository:

```bash
helm repo add myorg --username <GITHUB_USERNAME> --password <GITHUB_PERSONAL_ACCESS_TOKEN> https://raw.githubusercontent.com/gr0uch0dev/{REPO}/master/helm-artifacts/
```

To install the chart:

```bash
helm install helm-nginx helm-nginx/helm-nginx-laboratory
```

Congratulations! You have created a Helm repository and used it to install a chart.
