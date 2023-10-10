# CI with Tekton

Collection of Tekton Pipelines to build SonataFlow objects.

Please follow all the steps in this instructions before running the examples.

## Prereqs

- [Install Tekton](https://tekton.dev/docs/installation/local-installation/)
- (Optional, but recommended) [Install Tekton Dashboard](https://tekton.dev/docs/dashboard/)

## Install required Tasks

Assuming your Tekton installation is working, you must create two required tasks by this pipeline in the target namespace (the namespace where you will run the pipeline):

```shell
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-cli/0.4/git-cli.yaml
kubectl apply -f https://api.hub.tekton.dev/v1/resource/tekton/task/github-open-pr/0.2/raw

```
