在pod的生態中,本身就是一個小小世界,pod跟pod之間不影響,當建立新的pod時,基本上不會跟被刪除的pod有所關聯.
因此當使用redis等功能時就會很麻煩,只要pod重啟,裡面的內容就會消失.
但是通常redis內都會儲存一些狀態類的東西,消失的話可能會導致整個服務停擺,那麼這時該怎麼處理呢?
就是statefulset上場的時候了.

## StatefulSets
需要用到StatefulSets的情況沒那麼常見,但是當你服務擁有以下要求,就會需要StatefulSets了:
1. 穩定&獨立的網路標籤(固定的pod name或 host name)
2. 穩定的persistent storage(當關掉電源後資料不會因此消失,還是能存取相同資料的儲存庫,揮發式儲存庫的相反(Volatile storage 當電源關閉後,資料就會消失))
3. 有序 逐一緩慢地擴充(scaling)和部屬(deployment)的pod
4. 有序 自動地滾動更新(rolling updates)的pod

上面這些情境都適合StatefulSets.

## 限制
使用StatefulSets會有以下限制
1. pod的儲存必須[PersistentVolume Provisioner](https://github.com/kubernetes/examples/blob/master/staging/persistent-volume-provisioning/README.md)基於storage class或者管理者事先提供.
2. 基於資料安全,擴充(scaling)以及刪除不會刪掉相關的儲存卷(volumes)
3. 需要Headless Service來處理pod的network identity.

## 範例

```
apiVersion: v1
kind: Service
metadata:
  name: nginx
  labels:
    app: nginx
spec:
  ports:
  - port: 80
    name: web
  clusterIP: None
  selector:
    app: nginx
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: web
spec:
  selector:
    matchLabels:
      app: nginx # has to match .spec.template.metadata.labels
  serviceName: "nginx"
  replicas: 3 # by default is 1
  template:
    metadata:
      labels:
        app: nginx # has to match .spec.selector.matchLabels
    spec:
      terminationGracePeriodSeconds: 10
      containers:
      - name: nginx
        image: k8s.gcr.io/nginx-slim:0.8
        ports:
        - containerPort: 80
          name: web
        volumeMounts:
        - name: www
          mountPath: /usr/share/nginx/html
  volumeClaimTemplates:
  - metadata:
      name: www
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "my-storage-class"
      resources:
        requests:
          storage: 1Gi
```
(來源:https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/)

上面的範例有幾個重點:
1. 名為nginx的Headless Service,用於控制網路的domain.
2. 名為web的StatefulSet,依照spec可看出會有三個nginx的容器被建立在獨立的pod中.
3. volumeClaimTemplates透過PersistentVolume Provisioner提供穩定的儲存庫.

上面這種yaml檔就可以建立出StatefulSet了,在這章簡單帶過StatefulSet的使用情境以及yaml,下一章則會介紹StatefulSet內pod的Identity和擴充以及更新策略
