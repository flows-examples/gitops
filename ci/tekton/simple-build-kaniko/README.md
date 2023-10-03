# CI Build with Tekton and Kaniko

This example shows how to build a SonataFlow workflow from a git repository using Tekton and Kaniko.

This pipeline is really simple:

1. First clone the target git repository
2. Tries to find the workflow `id` and `version` to use in the final image. For this, the pipeline requires the main workflow in the project to be named `workflow.sw.json`. (Hint: you can make this pipeline smarter to accept any `.sw.json|yaml|yml` files).
3. Runs the Kaniko build based on the target cloned git repository and pushes the image to the target registry.

## Prereqs

- [Install Tekton](https://tekton.dev/docs/installation/local-installation/)
- (Optional, but recommended) [Install Tekton Dashboard](https://tekton.dev/docs/dashboard/)

## Install required Tasks

Assuming your Tekton installation is working, you must create two required tasks by this pipeline in the target namespace (the namespace where you will run the pipeline):

```shell
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-clone/0.9/git-clone.yaml
kubectl apply -f https://api.hub.tekton.dev/v1/resource/tekton/task/kaniko/0.6/raw
```

## Create a Container Registry Secret

This is an optional step to create a container registry secret to where Kaniko will push the final built image. If your registry doesn't require authentication, you can skip this step.

If your registry doesn't require authentication, you must remove the workspace `image-pull-secret` from the [sonataflow-kaniko-pipeline-run.yaml](sonataflow-kaniko-pipeline-run.yaml) file.

Command to create the secret in the target namespace. Replace `<>` with your info:  

```shell
kubectl create secret docker-registry quay-creds --docker-server=<your-registry-server> --docker-username=<your-name> --docker-password=<your-pword> --docker-email=<your-email>
```

## Pipeline Instalation

To install the pipeline in the target namespace just run the following command from this directory:

```shell
kubectl apply -f sonataflow-kaniko-pipeline.yaml
```

You can see the pipeline installed in the Tekton Dashboard.

## Run the Pipeline

To run the pipeline, just create the following resource:

```shell
kubectl create -f sonataflow-kaniko-pipeline-run.yaml
```

You may fire a new run anytime by just creating new runs using this manifest. You may also change the parameters to run your own project.
