?> In this task, we will deploy a pod to k8s cluster.

Source code for all upcoming tasks: <https://github.com/tobyqin/docs/tree/master/k8s/demo>

```yaml
# pod-hello.yaml
apiVersion: v1
kind: Pod
metadata:
  name: hello
spec:
  containers:
    - name: hello
      image: tobyqin/hello
      env:
        - name: NAME
          value: 'Toby Qin'
        - name: MESSAGE
          value: 'Kubernetes'
```

## Command

```bash
k create -f pod.yaml

# test it by enter it
k exec  -it hello -- sh
/app # wget -S http://127.0.0.1:5000
# Connecting to 127.0.0.1:5000 (127.0.0.1:5000)
#   HTTP/1.0 200 OK
#   Content-Type: text/html; charset=utf-8
#   Content-Length: 451
#   Server: Werkzeug/2.0.1 Python/3.8.7
#   Date: Sat, 12 Jun 2021 09:49:47 GMT

# Ctrl + D to exit
```

## Shortcut

```bash
# run a pod
kubectl run busybox --image=busybox -- sleep 6000

# run a command
kubectl run busybox --image=busybox -it -- echo "AMD Yes!"

# enter pod shell
kubectl exec -it busybox -- sh
```
