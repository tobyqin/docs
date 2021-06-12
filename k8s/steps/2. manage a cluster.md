?> In this task, we will warm up ourselves with `kubectl`, try to play with some basic commands.

## Basic commands

```bash
k cluster-info
k config

# get
k get node
k get pod

# useful - get shortcut to avoid typing long names
k api-resources 

k get cj
k get cm
k get job -o yaml
k get events

# inspect
k describe <resource>
k get <resource> -o yaml

# create
k run <pod>
k create <resource>

# edit
k edit <resource>

# delete
k delete <resource>

# useful
k logs <pod>
k exec <pod>
k label <resource>
```
