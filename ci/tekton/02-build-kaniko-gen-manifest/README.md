# CI Build with Kaniko, Manifest, and Open PR

```shell
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/git-cli/0.4/git-cli.yaml
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/main/task/github-open-pr/0.2/github-open-pr.yaml
```

```shell
# Make sure to keep the file as https://github.com/tektoncd/catalog/blob/main/task/git-cli/0.4/README.md#using-ssh-credentials
kubectl create secret generic ssh-github --from-file=${HOME}/.ssh
```

```shell
# Generate a Github Token with permissions to open PRs, see more at https://github.com/tektoncd/catalog/blob/main/task/github-open-pr/0.2/README.md
kubectl create secret generic github-token --from-literal=token=<YOUR-TOKEN-HERE>
```
