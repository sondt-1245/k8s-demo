# K8s demo với Kind
## Tạo cụm k8s dưới local bằng Docker
- Cài đặt [kind](https://kind.sigs.k8s.io/) 
- Cài đặt [kubectl](https://kubernetes.io/docs/tasks/tools/)

Cụm bao gồm 1 control plane à 2 node
```
kind create cluster --config kind-config.yaml
```

- Cài đặt thêm [kubens và kubectx](https://github.com/ahmetb/kubectx) khi làm việc với nhiều k8s cluster

## Deployment 

```
kubectl apply -f deployment/deployment.yaml

## Chúng ta sẽ có 1 Workload gồm các thành phần này
NAME                                    READY   STATUS    RESTARTS   AGE
pod/nginx-deployment-57f4678764-9mbpr   1/1     Running   0          5s
pod/nginx-deployment-57f4678764-pzwv6   1/1     Running   0          5s
pod/nginx-deployment-57f4678764-rvjqf   1/1     Running   0          5s

NAME                    TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)   AGE
service/kubernetes      ClusterIP   10.96.0.1      <none>        443/TCP   5d22h
service/nginx-service   ClusterIP   10.96.71.226   <none>        80/TCP    5s

NAME                               READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   3/3     3            3           5s

NAME                                          DESIRED   CURRENT   READY   AGE
replicaset.apps/nginx-deployment-57f4678764   3         3         3       5s

```

để test khả năng cân bằng tải trên deployment

```
kubectl apply -f curl-test-pod.yaml

###
# Chúng ta sẽ thấy response từ các pod nginx khác nhau
###
OK: 10 MiB in 20 packages
<html>
<head>
    <title>Welcome to nginx!</title>
</head>
<body>
<h1>Hello World from nginx-deployment-57f4678764-pzwv6</h1>
</body>
</html>
<html>
<head>
    <title>Welcome to nginx!</title>
</head>
<body>
<h1>Hello World from nginx-deployment-57f4678764-9mbpr</h1>
</body>
</html>
<html>
<head>
    <title>Welcome to nginx!</title>
</head>
<body>
<h1>Hello World from nginx-deployment-57f4678764-rvjqf</h1>
</body>
</html>
<html>
<head>
    <title>Welcome to nginx!</title>
</head>
<body>
<h1>Hello World from nginx-deployment-57f4678764-9mbpr</h1>
</body>
</html>

```