# 部署PiggyMetrics到Kubernetes

## 部署MongoDB

### 部署MongoDB到OpenShift/Kubernetes


```bash
helm repo add bitnami https://charts.bitnami.com/bitnami

helm upgrade --install mongodb bitnami/mongodb -f kubernetes/mongodb/values.yaml
```

参考文档：
- https://artifacthub.io/packages/helm/bitnami/mongodb
- https://ksingh7.medium.com/deploy-mongodb-on-openshift-using-helm-9430f58ebb3
- https://github.com/bitnami/charts/issues/932


转发Kubernetes上MongoDB端口到本地，以便本地可以连接到MongoDB：

```bash
kubectl port-forward svc/mongodb 27017:27017
```

参见[mongodb](https://github.com/xdevops-caj-lab-cloudnative-tk-msa/mongodb) 验证是否可以正常连接到MongoDB。

## 部署应用

### 部署exchange-rate服务

```bash
kubectl apply -f kubernetes/exchange-rate
```

### 部署gateway服务

```bash
kubectl apply -f kubernetes/gateway
```

### 部署auth服务

```bash
kubectl apply -f kubernetes/auth-service
```

### 部署account服务

```bash
kubectl apply -f kubernetes/account-service
```

### 部署statistics服务

需要先将ConfigMap中的`RATES_URL`替换为你的exchange-rate服务的API URL。

```bash
kubectl apply -f kubernetes/statistics-service
```

## 测试应用

验证Pods：

```bash
kubectl get pods
```

在浏览器中，打开gateway服务的路由URL来访问PiggyMetrics，然后创建一个新的账户来测试。
