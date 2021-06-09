?> In this task, we will learn how to clean up k8s resources and objects.

> Tips: Search `delete` in https://kubernetes.io/docs/reference/kubectl/cheatsheet/

```bash
k delete <kind> <name> -l <label>

k run pod1 --image=nginx
k run pod2 --image=nginx
k get pod
k delete pod/pod1
k label pod pod2 env=dev
k delete pod -l env=dev

# force delete
 k delete <kind> <name> --grace-period=0 --force

# enjoy the last command
k delete all --all
```
