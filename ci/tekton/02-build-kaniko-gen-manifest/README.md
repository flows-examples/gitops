# CI Build with Kaniko, Manifest, and Open PR

This pipeline is slightly more complex than [the previuos example](../01-build-kaniko/README.md). This example assumes that you read and understood the previous one.

1. After building the image, the pipeline then generates the SonataFlow manifest based on your project and the built image
2. Next, the pipeline prepares a PR to update the [config repository](https://argo-cd.readthedocs.io/en/stable/operator-manual/declarative-setup/) with SonataFlow manifests
3. Finally, it opens the PR in the target repo.

The idea is to update the application status in a Kubernetes cluster.

## Prereqs

See [main prereqs](../README.md).

Create a secret to hold your ssh credentials. For security reasons, you may create a new SSH key and configure in your Github account.

```shell
# Make sure to keep the file as https://github.com/tektoncd/catalog/blob/main/task/git-cli/0.4/README.md#using-ssh-credentials
kubectl create secret generic ssh-github --from-file=${HOME}/.ssh
```

Create a secret to hold a [Github token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens#fine-grained-personal-access-tokens) to open PRs in your organization:

```shell
# Generate a Github Token with permissions to open PRs, see more at https://github.com/tektoncd/catalog/blob/main/task/github-open-pr/0.2/README.md
kubectl create secret generic github-token --from-literal=token=<YOUR-TOKEN-HERE>
```

## Install required Tasks

See [main Install required Tasks](../README.md), additionally you must install:

```shell
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-cli/0.4/git-cli.yaml
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/github-open-pr/0.2/github-open-pr.yaml
```

## Pipeline Instalation

To install the pipeline in the target namespace just run the following command from this directory:

```shell
kubectl apply -f sonataflow-kaniko-genman-pipeline.yaml
```

You can see the pipeline installed in the Tekton Dashboard.

## Run the Pipeline

Besides the required inputs from the [previous example](../01-build-kaniko/README.md#run-the-pipeline), you need:

- `repo-config-url`: the configuration Github repository. E.g. where you keep the kubernetes manifests to update the cluster state.
- `github-token-secret`: the Secret that holds the Github Token [to open a PR](#prereqs).
- `github-user` and `github-email`: the Github user and e-mail that owns the token and that will open the PR.

[Edit to change with your data](sonataflow-kaniko-genman-pipeline-run.yaml), then you can then create the `PipelineRun` manifest:

```shell
kubectl create -f sonataflow-kaniko-genman-pipeline-run.yaml
```

You may fire a new run anytime by just creating new runs using this manifest. You may also change the parameters to run your own project.
