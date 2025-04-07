?> In this task, we will deploy an application with `Deployment` object type.

A better guide: https://kubernetes.io/docs/tutorials/configuration/configure-java-microservice/configure-java-microservice-interactive/

In this task you will learn:

- [ConfigMap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)
- [Deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)
- [livenessProbe and readinessProbe](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

## ConfigMap

> Tips: get some interesting images from https://www.sohu.com/a/122861143_355150

```yaml
apiVersion: v1
data:
  NAME: Toby
  MESSAGE: Deployment
  IMAGE: 'http://img.mp.itc.cn/upload/20161228/fbd7ab3c3d5b45a3bacd0c058762a73f_th.jpeg'
kind: ConfigMap
metadata:
  name: app-config
```

```bash
k ge configmap
k describe configmap app-config
```

## Deployment

```yaml
# deploy.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kitty-deployment
  labels:
    app: kitty
spec:
  replicas: 3
  selector:
    matchLabels:
      app: kitty
  template:
    metadata:
      labels:
        app: kitty
    spec:
      containers:
        - name: hello
          image: tobyqin/hello
          envFrom:
            - configMapRef:
                name: app-config
          ports:
            - containerPort: 5000
          readinessProbe:
            httpGet:
              path: /
              port: 5000
              scheme: HTTP
            periodSeconds: 15
            failureThreshold: 5
            timeoutSeconds: 1
          livenessProbe:
            httpGet:
              path: /health
              port: 5000
              scheme: HTTP
            initialDelaySeconds: 60
            periodSeconds: 15
            successThreshold: 1
            failureThreshold: 3
            timeoutSeconds: 1
```

## Command

```bash
k create -f deploy.yaml

# inspect
$ k get deploy
NAME               READY   UP-TO-DATE   AVAILABLE   AGE
kitty-deployment   3/3     3            3           22s

$ k get rs
NAME                          DESIRED   CURRENT   READY   AGE
kitty-deployment-597484896f   3         3         3       28s

$ k get pod
NAME                                READY   STATUS      RESTARTS   AGE
kitty-deployment-597484896f-6wjjq   1/1     Running     0          49s
kitty-deployment-597484896f-vhvvt   1/1     Running     0          49s
kitty-deployment-597484896f-w2fx4   1/1     Running     0          49s

$ k exec -it kitty-deployment-597484896f-w2fx4 -- env | sort
FLASK_APP=app
HOME=/root
HOSTNAME=kitty-deployment-597484896f-w2fx4
IMAGE=http://img.mp.itc.cn/upload/20161228/fbd7ab3c3d5b45a3bacd0c058762a73f_th.jpeg
MESSAGE=Deployment
NAME=toby
KUBERNETES_PORT=tcp://10.245.0.1:443
...

```

## Shortcut

```bash
# create a deployment
kubectl create deploy my-deploy --image=nginx --port=80 --replicas=2

# get logs of deploymnt
kubectl logs deploy/my-deploy

# scale up deployment
kubectl scale --replicas=3 deploy/my-deploy

# get deployment history
kubectl rollout history deployment/my-deploy

# set deployment image
kubectl set image deployment/my-deploy nginx=nginx:v2

# rollback deployment
kubectl rollout undo deployment/my-deploy
```
