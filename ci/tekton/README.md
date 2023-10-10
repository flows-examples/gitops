# CI with Tekton

Collection of Tekton Pipelines to build SonataFlow objects.

Please follow all the steps in this instructions before running the examples.

## Prereqs

- [Install Tekton](https://tekton.dev/docs/installation/local-installation/)
- (Optional, but recommended) [Install Tekton Dashboard](https://tekton.dev/docs/dashboard/)

## Install required Tasks

Assuming your Tekton installation is working, you must create one required task by every pipeline in the target namespace (the namespace where you will run the pipeline):

```shell
kubectl apply -f https://github.com/tektoncd/catalog/tree/main/task/git-clone/0.9
```

## Available Examples

1. [Build a SonataFlow with Kaniko](01-build-kaniko/README.md)
2. [Build a SonataFlow with Kaniko, and open PRs to update Kubernetes manifests](02-build-kaniko-gen-manifest/README.md)
