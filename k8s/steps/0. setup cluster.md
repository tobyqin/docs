?> In this task, we will setup a k8s cluster, see https://kubernetes.io/docs/tutorials/

> Tips: you should always keep a doc window opened to learn k8s, it is the best resource you should use.

## Local setup

Recommend https://kind.sigs.k8s.io/docs/user/quick-start/

```bash
# Windows
choco install kind

# Mac
brew install kind

# Create cluster
kind create cluster

# Delete cluster
kind delete cluster
```

## Managed cluster setup

- Option 1: https://m.do.co/c/82dee83362d8
- Option 2: https://console.cloud.tencent.com/tke2/
- Option 3: https://kodekloud.com/purchase?product_id=1521964

It might take 5~30 minutes to finish this task.

![](../images/managed-k8s.png)

If you cannot setup the cluster successfully, you can use the interactive shell in docs site.

## Plan B

We can use interactive tutorials to learn kubernetes.

- https://kubernetes.io/docs/tutorials/configuration/configure-java-microservice/configure-java-microservice-interactive/
