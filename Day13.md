之前提到了兩種跟pod互動的方式，上一章介紹了port-forward這個方法，這一章則會介紹service。

## 什麼是service
[官方說明](https://kubernetes.io/docs/concepts/services-networking/service/#service-resource)為
a Service is an abstraction which defines a logical set of Pods and a policy by which to access them.
翻過來就是 service是一個抽象概念，其定義了邏輯上的一組pod以及用來存取那些pod的規範。
簡單來說就是在pod外擋一層虛擬層，方便另一組pod來認識這組pod群，不必認識單個單個pod。
[官方動機](https://kubernetes.io/docs/concepts/services-networking/service/#motivation)也寫說，當前端(一組pod)跟後端(另一組pod)在同一個cluster內時，前端要怎麼去發現以及保持追蹤後端的ip呢？
就要透過serive，也就是說service提供了讓這群pod被別組pod認識的方法。

## 建立Service
要建立service有幾種方式，這章會先用最簡單的方法來建立service，比較複雜的方法會留到下一章，而最簡單的方法，就是expose指令

### 範例pod
首先先用下面的yaml建立起一個pod
```
apiVersion: v1
kind: Pod
metadata:
  name: my-redis
  labels:
    app: redis
spec:
  containers:
  - name: pod-redis
    image: redis
    ports:
    - containerPort: 6379
```

### kubectl expose
再來執行下面指令
```
kubectl expose pod my-redis --type=LoadBalancer --name=my-redis-service
```

![https://ithelp.ithome.com.tw/upload/images/20210921/20129607dM0He1qbf9.png](https://ithelp.ithome.com.tw/upload/images/20210921/20129607dM0He1qbf9.png)
就可以看到名為my-redis-service的service被建立出來了，不過由於要能夠直接打到需要EXTERNAL-IP
而kind建立的cluster沒辦法直接提供，一個方法是使用線上雲端像是GCP或是AWS，另一個方法是用minikube，
可以下達
```
minukube service my-redis-service
```
他可以提供你EXTERNAL-IP

除了讓k8s幫你產生port，你也可以指定port
```
kubectl expose pod my-redis --type=LoadBalancer --name=my-redis-service --port=7788 --target-port=6379
```
這樣就可以指定port給service了。
不過這邊的範例都只有一個pod，其實用service的意義不大，因此在下一章會介紹Deployment，產生出多個pod的情境，才比較好瞭解service的效果。
