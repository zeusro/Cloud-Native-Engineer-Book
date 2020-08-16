# 【未完】部署Elastic Cloud on Kubernetes (ECK)  

## 缘起

阿米巴集团的 `elasticsearch` 集群是我用 `statefulset` 搭建的。当时我发现几个问题：

1. 调整的时候要非常小心
2. pod挂掉（CrashLoopBackOff）之后不会重启
3. 

比如创建了3个副本 e-0 , e-1 ,e-2 。如果 e-1 突然烂掉了（CrashLoopBackOff），按照 `statefulset` 的设计，是不会自动重启的。

还有一次因为配置错误，导致了故障的出现，因为当时 `flexvolume` 的局限性，我只能选 `hostpath` 作为 `volume` ,结果一翻车整个集群因为磁盘满了，雪崩式崩溃。


## 前置准备

按照 elastic 官方的实现，需要做一点[前置准备](https://github.com/elastic/cloud-on-k8s/blob/master/dev-setup.md)。

```bash
brew install golangci/tap/golangci-lint
```

比较蛋疼的是，`controller-gen`虽说是包含在[controller-tools](https://github.com/kubernetes-sigs/controller-tools)里面，但我在 release 里面找不到。所以就自己编译算了 ~ 

```bash
git clone git@github.com:kubernetes-sigs/controller-tools.git
cd controller-tools
cd cmd/controller-gen
go build
mv controller-gen ~/go/bin
```

## Just Do It！

按照[官方项目](https://github.com/elastic/cloud-on-k8s)
对应的[文档](https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-quickstart.html)操作即可。

这篇英文文章
《[Running and Deploying Elasticsearch Operator on Kubernetes](https://sematext.com/blog/elasticsearch-operator-on-kubernetes/)》也写得很好。我这里主要在它基础上做点补充。

默认情况下会创建一个`elastic-system`的命名空间，之后在这上面创建一些资源

```bash
customresourcedefinition.apiextensions.k8s.io/apmservers.apm.k8s.elastic.co created
customresourcedefinition.apiextensions.k8s.io/elasticsearches.elasticsearch.k8s.elastic.co created
customresourcedefinition.apiextensions.k8s.io/kibanas.kibana.k8s.elastic.co created
clusterrole.rbac.authorization.k8s.io/elastic-operator created
clusterrolebinding.rbac.authorization.k8s.io/elastic-operator created
namespace/elastic-system created
statefulset.apps/elastic-operator created
clusterrole.rbac.authorization.k8s.io/elastic-operator-view created
clusterrole.rbac.authorization.k8s.io/elastic-operator-edit created
serviceaccount/elastic-operator created
validatingwebhookconfiguration.admissionregistration.k8s.io/elastic-webhook.k8s.elastic.co created
service/elastic-webhook-server created
secret/elastic-webhook-server-cert created
```

之后会出现这一类资源。

```bash
NAME                 READY   STATUS    RESTARTS   AGE
elastic-operator-0   1/1     Running   0          3h
➜  Desktop kg all
NAME                     READY   STATUS    RESTARTS   AGE
pod/elastic-operator-0   1/1     Running   0          3h1m

NAME                             TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)   AGE
service/elastic-webhook-server   ClusterIP   10.2.49.167   <none>        443/TCP   3h1m

NAME                                READY   AGE
statefulset.apps/elastic-operator   1/1     3h1m
```

## 参考链接


[1]
quickstart
https://www.elastic.co/guide/en/cloud-on-k8s/current/k8s-quickstart.html#k8s_request_elasticsearch_access

[2]
Elastic Cloud on Kubernetes
https://www.elastic.co/guide/en/cloud-on-k8s/master/index.html

[3]



[4]



[5]




