?> In this task, we will create namespace and set it up to current context.

## Commands

```bash
# get all namespaces
k get ns

# get current namespace
k config current-context
k config get-contexts 
k config view --minify | grep namespace

# get pod in another namespace
k get pod --namespace=kube-system

# create new namespace
k create ns tobyqin
k create ns <your-id>

# run pod in another namespace
k run pod1 --image=nginx --namespace tobyqin
k get pod # empty
k get pod --namespace=tobyqin # 1 pod running
```

## Config current context

```bash
k config set-context --current --namespace=tobyqin
```

## Test

```bash
k config view # detail of your context
k get pod # 1 pod running
```
