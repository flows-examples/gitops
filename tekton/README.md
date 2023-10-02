# SonataFlow Tekton Pipelines

<!--
TODO:
- Document the simple workflow, limitations, what to expect, requirements, and premisses
-->

Create image push secret

```shell
kubectl create secret docker-registry quay-creds --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

Install the required tasks:

```shell
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml
kubectl apply -f https://api.hub.tekton.dev/v1/resource/tekton/task/kaniko/0.6/raw
```

Install the pipeline

```shell
kubectl apply -f sonataflow-project-pipeline.yaml
```

Run a build

```shell
kubectl create -f sonataflow-sample-parallel-build.yaml
```
