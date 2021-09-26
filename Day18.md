前面有特別提到,redis這類應用程式如果需要保持資料持久化,不會因為重啟pod造成資料消失,就會將redis改成stateful.

## 建立stateful redis
要建立Statefulset,就要先建立headless service.
```
apiVersion: v1
kind: Service
metadata:
  name: redis-service
  labels:
    app: redis
spec:
  ports:
  - name: redis-port
    port: 6379
  clusterIP: None
  selector:
    app: redis
    appCluster: redis-cluster
```

再來建立有 statefulset 的 redis 

```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  serviceName: "redis"
  replicas: 1
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis
        image: redis
        command: ["redis-server", "--appendonly", "yes"]
        ports:
        - containerPort: 6379
          name: web
        volumeMounts:
        - name: redis-aof
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: redis-aof
    spec:
      accessModes: [ "ReadWriteOnce" ]
      storageClassName: "gp2"
      resources:
        requests:
          storage: 1Gi
```

接下來可以試看看在redis內set一筆資料後,將redis的pod重啟
就可以看到資料還在,像下圖的操作.

## redis本身持久化設定
這邊特別提一下這段設定
```
- name: redis
    image: redis
    command: ["redis-server", "--appendonly", "yes"]
```

可以看到command那邊提到appendonly,這個是什麼呢?
redis要持久化,除了pod設定改成statefulset外,也必須特別設定持久化.
有兩種方式:
1. RDB(Redis DataBasa file):
    已指定間隔對資料進行snapshot儲存,然後在進行操作時,會fork出一個process進行操作,fork完成後主process     就可以不受影響,不過由於其間隔複製的特性,有時會丟失資料.
    
2. AOF(Append-Only File):
    透過fsync策略進行備份,有三種模式可以選
    1. 無fsync
    2. 一秒鐘一次fsync(預設)
    3. 每次執行寫入時fsync
    由於AOF的特性是將Redis的寫入操作記下來,因此比較容易閱讀,也比較容易進行分析.
    並且丟失程度比RDB要來的少.
    不過相較於RDB,他的資料量也比較大,以及fsync有開啟時,效能會比RDB差(一般使用情境下看不出影響).
