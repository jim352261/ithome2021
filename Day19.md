在k8s內有著不同種類的pod組合，ReplicaSet StatefulSets，那麼剩下的DaemonSet到底是什麼呢？
這邊就來介紹他。
## 什麼是DaemonSet
DaemonSet會確保所有(或者某些)nodes上運行著特定功能的pod。當nodes加入cluster時就會在它們上面新增一個pod，當nodes從cluster上被移除時，那些pod也會被移除。如果DaemonSet被刪掉的話，pod也會跟著一起被刪掉。
下面是某些daemonsets的使用範例：
1. 在nodes上面運行cluster storage的daemon，例如[glusterd](https://github.com/gluster/gluster-kubernetes)或[ceph](https://docs.ceph.com/en/pacific/)
2. 在node上運行收集log的daemon，例如[Fluentd](https://www.fluentd.org/) 或 [logstash](https://www.elastic.co/logstash/)
3. 在node上運行監控node的daemon，例如[Prometheus Node Exporter](https://prometheus.io/docs/guides/node-exporter/) [collectd](https://collectd.org/) 或者 [Datadog agent](https://docs.datadoghq.com/agent/)等等

### DaemonSet範例
在這邊使用官方提供的範例，首先建一個yaml名為daemonset.yaml，並將下面內容複製進去。
```
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd-elasticsearch
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd-elasticsearch
  template:
    metadata:
      labels:
        name: fluentd-elasticsearch
    spec:
      tolerations:
      # this toleration is to have the daemonset runnable on master nodes
      # remove it if your masters can't run pods
      - key: node-role.kubernetes.io/master
        operator: Exists
        effect: NoSchedule
      containers:
      - name: fluentd-elasticsearch
        image: quay.io/fluentd_elasticsearch/fluentd:v2.5.2
        resources:
          limits:
            memory: 200Mi
          requests:
            cpu: 100m
            memory: 200Mi
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      terminationGracePeriodSeconds: 30
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
```

然後執行

```
kubectl apply -f daemonset.yaml
```

有幾點要特別注意，當daemonset建立後，就不建議修改`.spec.selector`，一但修改了可能會造成daemonset無法認得pod，使pod變成孤兒。以及`.spec.selector`必須和`.spec.template.metadata.labels`對的上，不然會被API server 阻擋掉。

要檢查是否有建立成功，可以用下面指令確認
```
kubectl get pods -l name=fluentd-elasticsearch -A
```
或者想要看到更細的資訊
```
kubectl get pods -l name=fluentd-elasticsearch -A -o wide
```

你就可以看到下圖
![https://ithelp.ithome.com.tw/upload/images/20210926/20129607QybuSbjLMN.png](https://ithelp.ithome.com.tw/upload/images/20210926/20129607QybuSbjLMN.png)

特別的是，當你直接使用get pods是看不到的，這是因為get pods會去撈名為default的namespace內的資源，但是daemonset不在namespace內，而是在kube-system內，所以你必須用
```
kubectl get pods -n kube-system
```
才能夠看到你剛剛建立的pod，而namespace也是下一章要講解的議題