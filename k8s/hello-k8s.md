# hello-k8s

A hello world guide to play with k8s.

## About author

My name is Toby Qin, I am a DevOps engineer works with containerization technology everyday. I had passed my [Certified kubernetes Application Developer](https://www.cncf.io/certification/ckad/) exam few weeks ago. I create this repo to guide newbies hit their road to cloud.

![ckad](images/ckad.png)

Welcome to follow me on [github](https://github.com/tobyqin).

## Introducing kubernetes

- https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/
- https://kubernetes.io/docs/tutorials/kubernetes-basics/
- https://kubernetes.io/docs/tutorials/

## Setup kubernetes clusters

You have multiple options to setup a kubernetes cluster, I list them down as:

## Local setup (not recommended)

It is the hard way of learning, due to the network condition of China, I would not recommend you try this way.

1. minikube - https://minikube.sigs.k8s.io/docs/start/
2. kind - https://kind.sigs.k8s.io/
3. docker desktop - https://www.docker.com/products/docker-desktop
4. kubeadm - https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/

If you are really want to try the local cluster, I would vote for `docker desktop` and `kind`.

## Managed k8s clusters

To learn the kubernetes concepts and play with it, using managed k8s cluster is the best choice.

1. digital ocean - https://m.do.co/c/82dee83362d8
2. aws - https://aws.amazon.com/cn/kubernetes/
3. kode kloud - https://kodekloud.com/purchase?product_id=1521964
4. tencent tke - https://console.cloud.tencent.com/tke2/

I would recommend `digital ocean` and `tecent tke` when you picked a managed k8s cluster, because they will give you coupon to try anything for free at the very beginning.

Each platform will have good document or quick guide to help you setup the cluster, please refer to docs there.

## Connect to your k8s cluster

Usually we will connect to a k8s cluster by `kubectl`, see more:

- https://kubernetes.io/docs/tasks/tools/

Bookmark the cheatsheet, it is super helpful.

- https://kubernetes.io/docs/reference/kubectl/cheatsheet/

Now edit `~/.kube/config` with your cluster info, for example:

```yaml
apiVersion: v1
clusters:
- cluster:
    certificate-authority-data: LS0tLS1C...
    server: https://kubernetes.docker.internal:6443
  name: docker-desktop
contexts:
- context:
    cluster: docker-desktop
    user: docker-desktop
  name: docker-desktop
current-context: docker-desktop
kind: Config
preferences: {}
users:
- name: docker-desktop
  user:
    client-certificate-data: LS0tLS1CRUdJT...
    client-key-data: LS0tLS1CRUdJTi...
```

### Tips: speed up `kubectl` command

```bash
# add bellow config to ~/.bashrc
source <(kubectl completion bash)
alias k=kubectl
complete -F __start_kubectl k
```

## Enjoy your k8s journey

It is time to play with k8s, I had designed some steps to let you hand on k8s in `/steps` folder, feel free to explore them :-)

If you want to learn more, I would recommend you go to the official document site to finish the tasks:

- https://kubernetes.io/docs/tasks/configure-pod-container/
- https://kubernetes.io/docs/tasks/manage-kubernetes-objects/
- https://kubernetes.io/docs/tasks/configmap-secret/
- https://kubernetes.io/docs/tasks/inject-data-application/
- https://kubernetes.io/docs/tasks/run-application/
- https://kubernetes.io/docs/tasks/job/

You may try more tasks at https://kubernetes.io/docs/tasks/

## Tasks designed by me

  - [0. Setup cluster](steps/0.%20setup%20cluster.md)
  - [1. Install tools](steps/1.%20install%20tools.md)
  - [2. Manage cluster](steps/2.%20manage%20a%20cluster.md)
  - [3. Work in namespace](steps/3.%20work%20in%20namespace.md)
  - [4. Work with Docker](steps/4.%20work%20with%20docker.md)
  - [5. Run a Pod](steps/5.%20run%20a%20pod.md)
  - [6. Run a Job](steps/6.%20run%20a%20job.md)
  - [7. Deploy an app](steps/7.%20deploy%20an%20app.md)
  - [8. Expose an app](steps/8.%20expose%20an%20app.md)
  - [9. Delete resources](steps/9.%20delete%20resources.md)

Happy coding!

